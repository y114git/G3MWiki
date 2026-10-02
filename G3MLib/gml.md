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

Child code entries can belong to a parent code entry; do not treat each child as an independent top-level replacement. Games without supported GML bytecode cannot provide the same decompilation/editing flow.

## Queue code changes

`CodeImportGroup` accepts a loaded model and optional shared GlobalDecompileContext/settings. Queue changes by a code object or exact code-entry name:

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

AutoCreateAssets and AutoCreateGlobalInitScripts control creation associated with new names. Ambiguous event names, including collisions, can require explicit asset setup. Do not enable speculative name-based creation for edits to unrelated existing code.

For replacement-only work, the lower-level CompileGroup provides QueueCodeReplace and Compile. Both editing interfaces require a synchronous MainThreadAction callback when the host supplies one: it must execute the supplied action before returning.
