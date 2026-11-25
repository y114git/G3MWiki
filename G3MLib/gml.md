# GML editing

The DATA decompiler/compiler adapters use the loaded game's format and resource context. Decompiled text is editable source for a workflow, rather than a guaranteed copy of the author's original source.

## Decompile a code entry

```csharp
using G3MLib.Analyzer.Decompiler;
using G3MLib.DataFile;
using G3MLib.DataFile.Decompiler;

using var input = File.OpenRead("original.win");
using var data = GameMakerIO.Read(input);
var code = data.Code?.FirstOrDefault(entry => entry.ParentEntry is null)
	?? throw new InvalidDataException("No top-level code entry.");
var context = new GlobalDecompileContext(data);
var text = new DecompileContext(context, code,
	data.ToolInfo.DecompilerSettings).DecompileToString();
Console.WriteLine(text);
```

`DecompileContext.DecompileToSourceMap()` returns a `DecompiledSource(Text, Map)`. `SourceMap.Entries` lists every printed node with its text span and the bytecode it came from, and `SourceMap.Lines` converts offsets to line and column, so an editor can jump between text and instructions. `Disassembler.Disassemble(code, variables, locals, ignoreMissingCodeLocals, out lines)` returns the assembly text. The `out lines` list holds one `DisassemblyLine(InstructionOffset, TextOffset, Line)` per line, which maps the text to the instructions. `ScriptGlobals.GetDecompiledText` wraps the decompiler for scripts.

Child code entries can belong to a parent code entry; do not treat each child as an independent top-level replacement. Games without supported GML bytecode cannot provide the same decompilation/editing flow.

## Queue code changes

`CodeImportGroup` accepts a loaded model and optional shared GlobalDecompileContext/settings. Queue changes by a code object or exact code-entry name. The find and replace methods take `caseSensitive` (default `true`):

| Method | Change |
| --- | --- |
| QueueReplace | Replace the entry's source |
| QueueAppend | Add source after existing source |
| QueuePrepend | Add source before existing source |
| QueueFindReplace | Replace literal source text |
| QueueTrimmedLinesFindReplace | Match trimmed source lines |
| QueueRegexFindReplace | Use a regular expression replacement |

```csharp
using G3MLib.DataFile;
using G3MLib.DataFile.Compiler;
using G3MLib.Modding.Patching;

using var input = File.OpenRead("original.win");
using var data = GameMakerIO.Read(input);
var code = data.Code?.FirstOrDefault(entry => entry.ParentEntry is null)
	?? throw new InvalidDataException("No top-level code entry.");
var changes = new CodeImportGroup(data)
{
	ThrowOnNoOpFindReplace = true
};
changes.QueueFindReplace(code, "window_set_caption(\"Game\")",
	"window_set_caption(\"Mod test\")");
changes.Import();
input.Dispose();
PatchInputService.WriteDataFile(data, "output/code-edited.win", "original.win");
```

This example deliberately fails if that exact call is absent. Adapt the search to decompiled source from your own input rather than assuming every game contains it.

`Import()` throws on failed compilation by default. `Import(throwOnFailedCompile: false)` returns a CompileResult whose `Successful` value must be checked. Compile errors expose the code entry and GenerateDetailedMessage for reporting.

`Import(CodeImportMode mode, progress, cancellationToken)` returns a `CodeImportResult` with one result per queued operation, including the line and column of a compiler error:

| Mode | Effect |
| --- | --- |
| `Apply` | Compiles and links every operation into the data. |
| `DryRun` | Compiles only; nothing in the data changes. An operation counts as applied when it compiled and would apply. |
| `Stage` | Runs a dry run first and applies only when no operation failed. |

Use `Stage` when one failed edit should leave the whole data file untouched.

## Diagnostics and tokens

The compiler reports structured diagnostics as `GmlDiagnostic(Severity, Code, Message, Span, MacroName = null)` records. `Span` is a `SourceSpan` with start and end offsets, line and column, and `LineMap` converts offsets to positions. Codes are listed in [Limits and defaults](limits-and-defaults.md#compiler-diagnostics). Compile and lint fail when the compiler reports an error. A `CompileResult` has `Successful` and `Errors`; each `CompileError` has the `Code` entry, `BaseMessage`, `Diagnostic` and `GenerateDetailedMessage()`, and `PrintAllErrors(codeEntryNames)` joins them.

`GmlTokenizer.Tokenize(text, gms2OrLater = true)` returns `GmlToken(Kind, Span)` values, so an editor can highlight GML without compiling it.

The compiler accepts `return` directly before `}`, indexing the result of a call such as `f()[i]`, nested ternaries, chained `??`, and template strings in LTS data. An empty `#macro` does not swallow the next line.

`AutoCreateAssets` and `AutoCreateGlobalInitScripts` (both `true` by default) control creation associated with new names. Ambiguous event names, including collisions, can require explicit asset setup. Do not enable speculative name-based creation for edits to unrelated existing code. `GameMakerEventActions` (default `false`) writes new object event actions with an empty action name, as GameMaker does, instead of naming them after the code entry.

For replacement-only work, the lower-level `CompileGroup` provides `QueueCodeReplace` and `Compile`. `Check(cancellationToken)` compiles without linking and changes nothing, `Compile(cancellationToken)` has a cancel point before each operation, and `PersistLinkingLookups` (default `false`) keeps string and asset lookups between compiles; leave it off unless no data changes between compiles. Both editing interfaces require a synchronous MainThreadAction callback when the host supplies one: it must execute the supplied action before returning.

## Macros

GameMaker expands macros while compiling, so the data file keeps no record of them. When an entry is compiled, macros declared in other code entries must be supplied:

```csharp
using G3MLib.DataFile;
using G3MLib.DataFile.Compiler;

using var input = File.OpenRead("original.win");
using var data = GameMakerIO.Read(input);
var group = new CompileGroup(data);
group.AddMacros("#macro MAX_HP 100\n#macro MIN_HP 1");
group.Macros["START_ROOM"] = "0";
var code = data.Code!.First(entry => entry.ParentEntry is null);
group.QueueCodeReplace(code, "var hp = MAX_HP - MIN_HP;");
var result = group.Check();
Console.WriteLine(result.Successful);
```

- `CompileGroup.Macros` maps a name to the GML expression it stands for. `AddMacros(gmlSource)` reads the `#macro NAME value` declarations of GML source with the compiler's lexer and adds them (a name already present is kept). Line continuations and CRLF work, and `#macro` inside comments and strings is ignored.
- Macros declared with `#macro` in code queued in the same group are added automatically and do not override the table.
- `CodeImportGroup.Macros` is the same table for the import interface and is copied to its compile group.
- `CompileContext.ExternalMacros` (init-only) takes the same table for a single compile.
- A macro applies only where its name would otherwise be read as an instance variable. A declared local of the same name, a member access such as `a.NAME`, and macros declared by the code itself take precedence.
- A macro that names itself or grows without end is reported as an error.

## Compile context

`CompileContext(code, scriptKind, globalScriptName, gameContext)` is one compile of one code entry. `CompileScriptKind` is `Script`, `GlobalScript`, `RoomCreationCode`, `ObjectEvent` or `Timeline`. After `Parse()`, `Compile(initialPosition = 0)` and `Link()` it exposes `Errors`, `Diagnostics`, `HasErrors`, `Macros`, `Enums`, `OutputInstructions`, `OutputFunctionEntries`, `OutputGlobalFunctionNames` and `OutputLength`. Most hosts use `CodeImportGroup` or `CompileGroup` instead and never create one directly.

## Debugger data

`DebugDataGenerator.Generate(data, mode, progress = null, cancellationToken = default)` builds GameMaker 1.4 debugger data (`.yydebug`) with one source text, one debug info list and the locals of every code entry, in code list order. An entry that cannot be decompiled gets its error as a comment. `DebugDataMode` selects the source text: `Decompiled` (decompiled GML, one debug info pair per entry), `Assembler` (disassembly with a pair at stores, jumps and returns) or `FullAssembler` (a pair at every instruction). `GameMakerDebugIO.Write(stream, debugData)` saves the result, and `GameMakerDebugIO.Read` reads one.
