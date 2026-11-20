# Scripts and ScriptGlobals

`.csx` scripts run through `ScriptRunner` against a `ScriptGlobals` object. Every host (a command line tool, a GUI, a project import) uses the same runner, so a script sees the same members everywhere. Scripts written for UndertaleModTool are translated before they compile.

Both types are in `G3MLib.DataFile.Scripting`.

## Run a script

```csharp
using G3MLib.DataFile;
using G3MLib.DataFile.Scripting;

using var data = await GameMakerIO.ReadAsync("original.win");
var globals = new ScriptGlobals(data, "original.win")
{
	MessageHandler = Console.WriteLine,
	WarningHandler = message => Console.Error.WriteLine(message),
	OutputDir = "output",
	InputDir = "input"
};
using var cancel = new CancellationTokenSource();
var result = await ScriptRunner.RunAsync(
	"scripts/tweak.csx", globals, cancel.Token, TimeSpan.FromMinutes(2));
if (result.Cancelled)
{
	Console.Error.WriteLine("Stopped: " + result.Error);
}
else if (!result.Success)
{
	Console.Error.WriteLine(result.Error);
}
```

`RunAsync(scriptPath, globals, cancellationToken = default, timeout = null)` reports every failure as a `ScriptRunResult` instead of throwing. A timeout that is zero or negative throws `ArgumentOutOfRangeException`; `Timeout.InfiniteTimeSpan` or `null` means no limit.

| ScriptRunResult member | Meaning |
| --- | --- |
| `Success` | The script ran to the end. |
| `Error` | A message ready to show to a user. `null` on success. |
| `Cancelled` | The token was cancelled or the timeout passed. `Success` is false then. |

Error texts:

| Case | `Error` |
| --- | --- |
| Compile failure | `Compilation error:` followed by the compiler diagnostics |
| Timeout | `Script timed out after <timeout>.` |
| Cancelled by the token | `Script was cancelled.` |
| `ScriptException` thrown by the script | the exception message |
| Any other exception | `Script execution failed: ` and a short description |

`ScriptRunner.Compile(scriptPath)` compiles without running and throws `CompilationErrorException` on errors. `ScriptRunner.CreateOptions(scriptPath)` returns the script options (standard imports and references, UTF-8, `#load` and `#r` resolved next to the script).

## Gate, threads and cancellation

- A run holds the write gate of `globals.Data` as it was when the run started. A script run without data holds no gate, and a data object the script creates (for example with `MakeNewDataFile`) is not gated. See [Cancellation and threading](cancellation-and-threading.md#data-access-gate).
- The script runs inside `globals.MainThreadAction`. The default runs it on the calling thread. A host that supplies another action must run the supplied callback before returning.
- The token ends when the host cancels, when the timeout passes (the wait for the gate counts), when a parent run is cancelled, or when the operation that started the script (a project import) is cancelled.
- Scripts read the token as `ScriptGlobals.CancellationToken`. `EnsureDataLoaded`, `ScriptMessage`, `SetProgressBar`, `SetProgress` and `AddProgress` throw `OperationCanceledException` once it is cancelled, so a script that never checks the token stops at its next call.

## ScriptGlobals members

`new ScriptGlobals(GameMakerData? data = null, string filePath = "", string? scriptPath = null)`. The class is not sealed. A host overrides the virtual members it supports.

### State and host settings

| Member | Meaning |
| --- | --- |
| `Data` | The data file. May be `null`. |
| `Project` | The `ProjectContext` of a project import, or `null`. |
| `FilePath` | Path of the data file. |
| `DataFilePath`, `OutputDir`, `InputDir` | Paths the host sets. Empty strings by default. |
| `ScriptPath` | Full path of the running script. A nested script sets its own path while it runs. |
| `ScriptRootPath` | Nested scripts must stay inside this folder. When unset, the folder of the first script that runs. |
| `ExePath` | Defaults to `AppContext.BaseDirectory`. |
| `CanSave`, `IsAppClosed` | Host flags. |
| `Highlighted`, `Selected` | Set by `ChangeSelection`. |
| `Verbose` | Virtual, `false` by default. |
| `CancellationToken` | See above. |
| `MainThreadAction` | `Action<Action>`, runs its argument synchronously. Default calls it directly. |
| `MessageHandler`, `WarningHandler` | Receive text from `ScriptMessage`, `SetUMTConsoleText`, `SimpleTextOutput` (message handler) and `ScriptWarning` (warning handler). |
| `ScriptExecutionSuccess`, `ScriptErrorMessage`, `ScriptErrorType` | Outcome of the last script. `ScriptErrorMessage` is empty when nothing failed. |

### Data and code helpers

| Member | Behaviour |
| --- | --- |
| `EnsureDataLoaded()` | Throws `ScriptException` ("No data file is currently loaded!") when `Data` is `null`. |
| `MakeNewDataFile()` | Returns `false` when `Data` is set. Otherwise creates an empty 2026 LTS data file, assigns it and returns `true`. |
| `GetDecompiledText(codeName or code, context = null, settings = null)` | Decompiled GML. Returns `/* DECOMPILER FAILED */` when decompilation throws. An unknown name throws `ScriptException`. |
| `GetDisassemblyText(codeName or code)` | Disassembly. A child entry returns a comment naming its parent. Returns `/* DISASSEMBLY FAILED */` on failure. |
| `AreFilesIdentical(file1, file2)` | Byte comparison. |
| `RunUMTScript(path)` | Runs a nested script. The path is relative to the calling script and must stay inside `ScriptRootPath`, otherwise `ScriptException`. A missing file or a failed nested run throws `ScriptException`. A cancelled nested run throws `OperationCanceledException`. Returns `true`. |
| `LintUMTScript(path)` | Compiles a script. Returns `false` and records the error on failure. |
| `ScriptError(error, title = "Error", SetConsoleText = true)` | Records the error and throws `ScriptException`. |

### Messages, dialogs and prompts

| Member | Default behaviour |
| --- | --- |
| `ScriptMessage(message)`, `SetUMTConsoleText(message)` | Calls `MessageHandler`. |
| `ScriptWarning(message)` | Calls `WarningHandler`. |
| `SimpleTextOutput(title, label, message, allowMultiline)` | Calls `ScriptMessage(message)`. |
| `ScriptQuestion(message)` | Returns `false`. |
| `ScriptInputDialog(title, label, defaultInput, cancelText, submitText, isMultiline, preventClose)` | Throws `ScriptException` ("This operation requires a UI host implementing IScriptInterface."). |
| `ScriptInputDialog(title, label, defaultInput, allowMultiline, showDialog)` | Short form. Returns `defaultInput` without asking when `showDialog` is `false`. Otherwise calls the seven-argument form with the labels `Cancel` and `OK` and `preventClose` false. |
| `SimpleTextInput`, `ScriptOpenURL`, `PromptChooseDirectory`, `PromptLoadFile`, `PromptSaveFile` | Throw the same `ScriptException`. |
| `ClickableSearchOutput(...)` (two overloads) | Returns a completed task. |
| `InitializeScriptDialog`, `SetFinishedMessage`, `EnableUI`, `StartProgressBarUpdater`, `StopProgressBarUpdater` | Do nothing. |
| `ChangeSelection(newSelection, inNewTab = false)` | Sets `Selected` and `Highlighted`. |
| `SyncBinding(...)`, `DisableAllSyncBindings()` | Static, accepted and ignored so that scripts for graphical hosts run unchanged. |

A host with a window overrides the dialog and prompt members. Throwing `ScriptCancelledException` from an override ends the script, and `ScriptRunResult.Error` holds its message (`Cancelled` stays false: that flag is for the token and the timeout).

### Progress

| Member | Behaviour |
| --- | --- |
| `SetProgressBar(message, status, progressValue, maxValue)`, `UpdateProgressBar(...)` | Stores the values and calls `ReportProgress`. |
| `SetProgressBar()` | Resets to zero of 100. |
| `UpdateProgressValue(value)`, `SetProgress(value)` | Sets the value. Values are clamped to `int`. |
| `UpdateProgressStatus(status)` | Sets the status text. |
| `AddProgress(amount)`, `IncrementProgress()`, `AddProgressParallel(amount)`, `IncrementProgressParallel()` | Add to the value. The parallel forms are the same operation, which is thread safe. |
| `GetProgress()` | Current value. |
| `HideProgressBar()` | Sets the value to zero. |
| `protected virtual ReportProgress(label, status, value, maximum)` | Override to show progress. |

## Embed in a host

```csharp
using G3MLib.DataFile;
using G3MLib.DataFile.Scripting;

public sealed class HostGlobals(GameMakerData data, string path) : ScriptGlobals(data, path)
{
	public override bool ScriptQuestion(string message) => AskUser(message);

	public override string ScriptInputDialog(string title, string label, string defaultInput,
		string cancelText, string submitText, bool isMultiline, bool preventClose) =>
		AskText(title, label, defaultInput) ?? throw new ScriptCancelledException("Input was cancelled.");

	protected override void ReportProgress(string label, string status, int value, double maximum) =>
		ShowProgress(label, status, value, maximum);

	private static bool AskUser(string message) => false;
	private static string? AskText(string title, string label, string defaultInput) => defaultInput;
	private static void ShowProgress(string? label, string status, int value, double maximum) { }
}
```

If the dialog opens a window on the thread that runs the script, a click handler on that thread shares the script's gate scope. Do not save or import from such a handler while the script runs.

## Exceptions

| Type | Use |
| --- | --- |
| `ScriptException` | A plain error that stops the script. It carries a message, not a stack trace. |
| `ScriptCancelledException` | A subclass for a user cancel inside the script. |

## See also

- [Projects and scripts](projects.md) for scripts that a project import starts, and `ProjectContext.AllowScripts`.
- [Cancellation and threading](cancellation-and-threading.md)
- [API index](api-index.md)
