# CSX scripting

A `.csx` file is a C# script that runs against optional loaded GameMaker DATA. Both G3MTool applications run scripts through the G3MLib 1.1.7 script runner, so a script sees the same globals and helpers in the CLI and in the GUI. Only the interactive helpers differ (see [CLI and GUI behavior](#cli-and-gui-behavior)).

A script has the permissions of the user who runs it: it can read and write files, start programs, and use the network. G3MTool does not sandbox it. Run only scripts you trust.

## First script

Save this as `resize-room.csx`:

```csharp
EnsureDataLoaded();
if (Data.Rooms.Count == 0)
{
    ScriptError("The data file has no rooms.");
}
Data.Rooms[0].Width += 16;
ScriptMessage("Increased the first room's width by 16 pixels.");
```

Run it on a copy of the DATA file you want to change:

```text
G3MTool execute resize-room.csx --data original.win --output test.win --verbose
```

After the script succeeds, G3MTool writes `Data` to `test.win` (through a temporary file) and keeps the original untouched. Without `--output <file>`, nothing is saved. Test the result in a separate game installation.

A script can also be an input of `patch create`, `patch apply`, `patch merge`, and the batch commands. G3MTool then runs it against the original and uses the saved result as the modified DATA. See [patch](commands/patch.md).

## Globals

These names are available in every script.

| Name | Type | Meaning |
| --- | --- | --- |
| `Data` | `GameMakerData` | The loaded DATA, or null when none was given. Assign it or call `MakeNewDataFile()` to create one |
| `FilePath` | string | The DATA path, or the script path when no DATA is loaded |
| `DataFilePath` | string | The DATA path, or an empty string |
| `ScriptPath` | string | Full path of the running script (inside a nested script, that script) |
| `ExePath` | string | Folder of the script. It is not the folder of the G3MTool executable |
| `InputDir` | string | CLI: the first script argument (`--input`, or the first value after `--`). GUI: the first line of **Script arguments**. Empty when none |
| `OutputDir` | string | Folder for exports. CLI: see [execute](commands/execute.md#c-script). GUI: the folder of **Output data**. Empty when none |
| `Verbose` | bool | CLI: whether `--verbose` is set. GUI: always true |
| `CancellationToken` | `CancellationToken` | Ends on Cancel, Ctrl+C, or timeout |
| `MainThreadAction` | `Action<Action>` | Runs an action through the host. Both applications run it directly on the script thread |
| `ScriptRootPath` | string | Folder of the first script; nested scripts must stay inside it |
| `ScriptExecutionSuccess`, `ScriptErrorMessage`, `ScriptErrorType` | bool, string, string | State of the last error |

The CLI passes the script's own folder as `ExePath` and `ScriptPath` as the full path. Scripts written for UndertaleModTool are accepted: identifiers starting with `Undertale` are rewritten to the matching `GameMaker` names before compiling.

### Namespaces and assemblies

These namespaces are imported, so no `using` line is needed:

`G3MLib.DataFile`, `G3MLib.DataFile.Models`, `G3MLib.DataFile.Decompiler`, `G3MLib.DataFile.Scripting`, `G3MLib.DataFile.Compiler`, `G3MLib.DataFile.Util`, `System`, `System.IO`, `System.Collections.Generic`, `System.Linq`, `System.Text`, `System.Text.Json`, `System.Text.RegularExpressions`, `System.Threading.Tasks`, `ImageMagick`.

Referenced assemblies: G3MLib, the .NET regular expression, JSON, LINQ (parallel), and timer assemblies, Magick.NET, and the G3MLib decompiler. Add a `using` for anything else in the base library, for example `using System.Threading;`. `#load "file.csx"` and `#r "file.dll"` are resolved from the script's folder; ship those files with the script. Source files are read as UTF-8.

## Helpers

| Helper | Behavior |
| --- | --- |
| `EnsureDataLoaded()` | Throws `No data file is currently loaded!` when `Data` is null. Also checks cancellation |
| `MakeNewDataFile()` | Creates an empty DATA when `Data` is null and returns true; returns false when DATA is loaded |
| `ScriptMessage(text)` | Writes a message. CLI: shown only with `--verbose` as `[Script] <text>`. GUI: Activity line. Also checks cancellation |
| `ScriptWarning(text)` | Writes a warning (`[Script Warning] <text>` in the CLI log) |
| `ScriptError(text)` | Fails the script with that text as the error |
| `ScriptQuestion(text)` | Yes/no question |
| `ScriptInputDialog(title, label, default, cancelText, submitText, multiline, preventClose)` | Text input |
| `SimpleTextInput(title, label, default, allowMultiline, showDialog = true)` | Text input |
| `PromptLoadFile(ext, filter)`, `PromptSaveFile(ext, filter)`, `PromptChooseDirectory()` | File and folder pickers |
| `RunUMTScript(path)` | Runs another script with the same globals. The path is relative to the calling script and must stay inside `ScriptRootPath`, else `Nested script path escapes the script directory.`; a missing file gives `Script was not found: <path>` |
| `LintUMTScript(path)` | Compiles a script without running it; returns false on errors |
| `GetDecompiledText(nameOrEntry)` | GML text of a code entry. `/* DECOMPILER FAILED */` on failure |
| `GetDisassemblyText(nameOrEntry)` | Assembly text of a code entry |
| `AreFilesIdentical(a, b)` | Byte comparison of two files |
| `SetProgressBar(...)`, `UpdateProgressBar(...)`, `UpdateProgressValue`, `UpdateProgressStatus`, `AddProgress`, `IncrementProgress`, `AddProgressParallel`, `IncrementProgressParallel`, `SetProgress`, `GetProgress`, `HideProgressBar` | Report progress. CLI: shown only with `--verbose`. GUI: moves the progress bar |
| `StartProgressBarUpdater()`, `StopProgressBarUpdater()` | Accepted, no effect |
| `SimpleTextOutput`, `SetUMTConsoleText` | Same as `ScriptMessage` |
| `ScriptOpenURL(url)` | Fails: `This operation requires a UI host implementing IScriptInterface.` |
| `SyncBinding`, `DisableAllSyncBindings`, `EnableUI`, `InitializeScriptDialog`, `SetFinishedMessage` | Accepted and ignored, so existing scripts run unchanged |

### CLI and GUI behavior

| Call | CLI | GUI |
| --- | --- | --- |
| `ScriptQuestion` | Logs `[Script Question] <text>` and returns true | Dialog with **Yes** and **No** |
| `ScriptInputDialog`, `SimpleTextInput` | Logs `[Script Input] <title>: <label> (default: <value>)` and returns the default | Dialog with **OK** and **Cancel**; Cancel returns null |
| `PromptLoadFile`, `PromptSaveFile`, `PromptChooseDirectory` | Returns null | Native dialog; Cancel returns null |
| Script timeout | `--timeout <seconds>` | none |

A script that must work in both hosts should handle a null return and should not depend on a dialog for required data. Pass such values as a file next to the script, or through `InputDir`.

## Running, cancelling, and timing out

1. The runner compiles the script. A compile error ends the run with `Compilation error:` followed by the compiler lines, for example `bad.csx(1,9): error CS1525: Invalid expression term ';'`.
2. While the script runs, it holds exclusive access to `Data`.
3. A script exception ends the run with `Script execution failed: <reason>`. `ScriptError(text)` ends it with exactly `text`.
4. On success, the host writes `Data` when an output file was requested.

Cancellation is cooperative. Cancel (GUI), Ctrl+C (CLI), or the `--timeout` limit signals `CancellationToken`. `EnsureDataLoaded`, `ScriptMessage`, the progress helpers, and the library's bulk operations stop with an exception, so a script ends at its next library call. Check `CancellationToken.ThrowIfCancellationRequested()` in long loops of your own. A timeout ends with `Script timed out after <hh:mm:ss>.` and CLI exit code `1`. Cancelling ends with exit code `130` in the CLI. The host does not save `Data` after a failed, cancelled, or timed-out run. Files the script created or overwrote itself, and other side effects, are not rolled back.

## Examples

Export names and sizes to a folder (run with `--output exports/`):

```csharp
EnsureDataLoaded();
var lines = new List<string>();
foreach (var room in Data.Rooms)
{
    CancellationToken.ThrowIfCancellationRequested();
    lines.Add($"{room.Name.Content}\t{room.Width}x{room.Height}");
}
File.WriteAllLines(Path.Combine(OutputDir, "rooms.txt"), lines);
ScriptMessage($"Wrote {lines.Count} rooms to {OutputDir}");
```

```text
G3MTool execute list-rooms.csx --data original.win --output exports/
```

Run another script from a script:

```csharp
// parent.csx
RunUMTScript("child.csx");
```

```csharp
// child.csx, in the same folder
ScriptMessage("child ran, input folder is '" + InputDir + "'");
```

Ask for a value and fall back when no dialog is available:

```csharp
if (!ScriptQuestion("Continue?")) return;
var name = SimpleTextInput("Name", "Room name", "room_new", false);
if (name is null)
{
    ScriptWarning("Cancelled by the user.");
    return;
}
ScriptMessage("name = " + name);
```

Use a script as a patch input:

```text
G3MTool patch create original.win resize-room.csx resize-room.g3mpatch
```

## Bundled scripts

The project source holds ready-made export and import scripts in [`G3MToolCLI/Assets/scripts`](https://github.com/y114git/G3MTool/tree/main/G3MToolCLI/Assets/scripts). Download the ones you need and run them with their file paths. Export scripts create a subfolder named after the resource type inside `OutputDir`; import scripts read the folder you pass with `--input`.

| Export | Folder created in `OutputDir` |
| --- | --- |
| `ExportAudioGroups`, `ExportBackgrounds`, `ExportCodeEntries`, `ExportEmbeddedTextures`, `ExportExtensions`, `ExportFonts`, `ExportGameObjects`, `ExportPaths`, `ExportRooms`, `ExportShaders`, `ExportSounds`, `ExportSprites`, `ExportTextureGroupInfo`, `ExportTexturePageItems`, `ExportTimelines` | `AudioGroups`, `Backgrounds`, `CodeEntries`, `EmbeddedTextures`, `Extensions`, `Fonts`, `GameObjects`, `Paths`, `Rooms`, `Shaders`, `Sounds`, `Sprites`, `TextureGroupInfo`, `TexturePageItems`, `Timelines` (same order) |

Other scripts in the folder: `ExportAssetOrder`, `ExportGeneralInfo`, `ExportTilesets`, and the matching `Import*` scripts. An import script ends with `InputDir is not set.` or `Input directory does not exist: <path>` when `--input` is missing or wrong.

```text
G3MTool execute ExportSprites.csx --data original.win --output exports/
G3MTool execute ImportSprites.csx --data original.win --input exports/Sprites --output imported.win
```

See also: [execute](commands/execute.md), [GUI](gui.md).
