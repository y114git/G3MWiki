# CSX scripting

A `.csx` file runs C# against optional loaded GameMaker DATA. The script has local file, process, and network permissions. G3MTool does not sandbox it.

## First script

Save this as `resize-room.csx`:

```csharp
EnsureDataLoaded();
if (Data!.Rooms.Count == 0)
{
	ScriptError("The data file has no rooms.");
}
Data.Rooms[0].Width += 16;
ScriptMessage("Increased the first room's width by 16 pixels.");
```

Run it on a copy of the intended game's DATA:

```text
G3MTool execute resize-room.csx --data original.win --output test.win --verbose
```

The runner writes Data after successful execution. Inspect the result and test it in a separate game installation. A script can also be supplied to patch create/apply/merge as an input that produces modified DATA.

## Globals

| Name | Purpose |
| --- | --- |
| `Data` | Loaded `GameMakerData`, or null without Data input |
| `FilePath` | Input DATA path, or script path when no DATA is loaded |
| `DataFilePath` | DATA input path, or an empty string |
| `ScriptPath` | Executing script path |
| `ExePath` | Script's directory; it is not necessarily the G3MTool executable directory |
| `InputDir` | First script argument, including the value supplied by Input |
| `OutputDir` | Directory derived from the output path |
| `MainThreadAction` | Executes an action through the host; the CLI executes it synchronously |
| `Verbose` | Whether detailed logging is enabled |

The environment imports common System namespaces and G3MLib DATA/model, utility, compiler, decompiler, and scripting namespaces. Use explicit `using` statements when referencing additional public APIs.

## Host helpers

`EnsureDataLoaded()` rejects a missing DATA model. `ScriptError(message)` fails the script. `ScriptMessage` and `ScriptWarning` write log messages; use Verbose when you need their CLI trace output.

`GetDisassemblyText` accepts a code name or code entry. `RunUMTScript(path)` runs another script with the current globals. Relative script dependencies can use `#load` resolved from the script directory; include those files when distributing the script.

Progress helpers include SetProgressBar, UpdateProgressBar, AddProgress, IncrementProgress, IncrementProgressParallel, StartProgressBarUpdater, and StopProgressBarUpdater. They report progress rather than changing game data.

## Interaction limits

In the CLI, `ScriptQuestion` logs the question and returns true. ScriptInputDialog and SimpleTextInput return the supplied default. File and directory prompts return null. A script expecting a desktop dialog must handle those results or accept explicit input paths.

The [GUI](gui.md) provides interactive script questions, text input, and file or folder selection. Test a script in its intended host. The same helper can return a default in the CLI and open a dialog in the GUI.

Reference scripts for exporting and importing resource types are available in [the project source](https://github.com/y114git/G3MTool/tree/main/G3MToolCLI/Assets/scripts). Save the scripts you need locally and invoke their actual file paths.
