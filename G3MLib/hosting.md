# Host integration

## Embed the library in an application

A host application takes these steps around G3MLib:

1. Reference the `G3MLib` package (`net10.0`). See [Getting started](getting-started.md).
2. Set `ModdingMetadata.ToolName` and `ToolVersion` once, so that patches record your application. See [Tool name and version](#tool-name-and-version).
3. Point `XDeltaService.DefaultExecutablePath` at an xdelta executable if you create or apply binary patches. See [xdelta executable](#xdelta-executable).
4. Route logs and progress with `LogService.BeginScope` around each job. See [Logs and progress](#logs-and-progress).
5. Pass a `CancellationToken` to every long call and catch `OperationCanceledException`. See [Cancellation and threading](cancellation-and-threading.md).
6. Guard the loaded `GameMakerData` with its `Gate` (or a `ModelLock`) when background jobs and edits overlap.
7. Save with `PatchInputService.WriteDataFile` so that external audio groups travel with the data file.
8. To run scripts, create a `ScriptGlobals` (or a subclass that overrides dialogs and progress) and call `ScriptRunner.RunAsync`. See [Scripts and ScriptGlobals](script-globals.md).
9. Decide `ProjectContext.AllowScripts` for projects from untrusted sources.
10. Choose cache and temporary folders (`G3MCacheOptions`, the `tempDir` parameters), and keep them on a drive with enough free space.

The library has no window, game launcher, mod manager or xdelta executable. Messages and progress arrive through callbacks, possibly on a background thread.

## Logs and progress

Scope callbacks to the awaited operation:

```csharp
using G3MLib.Modding.Logging;
using G3MLib.Modding.Patching;

using var logging = LogService.BeginScope(
	message => Console.WriteLine($"{message.Level}: {message.Message}"),
	progress => Console.WriteLine($"{progress.Operation}: {progress.Current}/{progress.Total}"),
	verbose: true);
var result = await PatchService.ValidatePatchAsync("interface.g3mpatch");
Console.WriteLine(result.Success);
```

ModdingLogMessage exposes Level and Message. Levels are Trace, Information, Warning, and Error. ModdingProgress exposes Operation, Current, Total, and IsComplete. Handle completion explicitly and avoid dividing by zero when Total is zero.

`BeginScope(message, progress, verbose = null, suppress = null)` takes the two callbacks and two optional flags. Trace messages reach a callback only when `verbose` is on or a message callback is set. `suppress: true` stops progress reports. Without a flag the scope inherits the current value, and `LogService.Verbose` and `Suppress` set the process-wide values.

BeginScope routes messages for the current asynchronous operation and returns a disposable scope. Keep it alive until awaited work finishes. For a GUI, marshal callbacks to the UI thread; a callback is not guaranteed to arrive on that thread.

Global MessageLogged and ProgressReported events are available for process-wide listeners. Unsubscribe when the host listener is disposed. Prefer scopes when concurrent jobs require separate displays.

## Cancellation

Patch create, validate and apply, merge, diff, resource export, import and hashing, `GameMakerIO.ReadAsync` and `WriteAsync`, project export, and `ScriptRunner.RunAsync` take a `CancellationToken`. A cancelled call throws `OperationCanceledException` (a script run returns `ScriptRunResult.Cancelled`). What each call leaves behind is listed in [Cancellation and threading](cancellation-and-threading.md).

Pass the token that your Cancel button owns, and catch `OperationCanceledException` separately from real failures.

## Analysis cache

```csharp
using G3MLib.Modding.Caching;
using G3MLib.Modding.Patching;

var result = await PatchService.CreatePatchAsync(
	"original.win", "modified.win", "interface.g3mpatch",
	cacheOptions: G3MCacheOptions.FromDirectory("cache"));
Console.WriteLine(result.Success);
```

FromDirectory enables reading and writing in one directory. A custom G3MCacheOptions can set ReadDirectory and WriteDirectory separately. None disables cache use.

`G3MCacheOptions.CanRead` and `CanWrite` tell whether a directory is set. Cache entries record the source file's size, last write time and MD5 (`G3MCacheManifest`, schema `G3MCacheManifest.CurrentSchema`), and a read validates the source first. `G3MCacheService` exposes the readers and writers (`TryReadDataCache`, `TryReadDataInfoSnapshot`, `TryReadDataFileInfo`, `WriteDataCacheAsync`, `WriteDataInfoCacheAsync`, `BuildInfoSnapshot`) for a host that manages its own cache.

The cache stores reusable analysis, not a substitute for the actual DATA file. The source must remain available. Cache directories can be discarded when no operation uses them; subsequent jobs rebuild the needed analysis.

## xdelta executable

The library host supplies xdelta. Configure DefaultExecutablePath for a process-wide default, or use an operation scope:

```csharp
using G3MLib.Modding.XDelta;

using var executable = XDeltaService.UseExecutable("tools/xdelta.exe");
var result = await new XDeltaService().CreatePatchAsync(
	"original.bin", "modified.bin", "change.xdelta");
if (!result.Success)
{
	throw new InvalidOperationException(result.Error);
}
```

Use the executable for the actual OS and architecture. The explicit XDeltaService constructor also accepts an executable path and process timeout. Default operations use a five-minute timeout.

ApplyPatchAsync performs binary decoding; ExecuteRawAsync forwards an argument array. Arguments are passed directly rather than interpreted by a shell.

## Tool name and version

`ModdingMetadata.ToolName` (default `G3MLib`) and `ModdingMetadata.ToolVersion` (default the library version) are written into the `Tool` entry of every patch manifest. The `Library` entry always holds G3MLib and `ModdingMetadata.LibraryVersion`. Set the two settable values once at start-up:

```csharp
using G3MLib.Modding;

ModdingMetadata.ToolName = "MyModTool";
ModdingMetadata.ToolVersion = "2.0.0";
Console.WriteLine(ModdingMetadata.LibraryVersion);
```

## File writes

Data files, patch archives, project files and audio group files are written to a temporary file next to the target (`<target>.<id>.g3mtmp`) and swapped in, so a failed write keeps the old file. A symlinked target keeps its link, and temporary files of the same target older than one day (left by a crash) are removed at the next write. `GameMakerIO.WriteAsync` to a path needs an existing folder. See [Limits and defaults](limits-and-defaults.md#timing-and-file-behaviour).

## Output and concurrency

Choose explicit output paths, reserve enough disk space for temporary materialization, and keep companion audio files with written DATA. Treat result errors and thrown exceptions as failures requiring inspection before consuming output.

Do not edit one GameMakerData instance from concurrent jobs or share an output path between them. Scoped logging and xdelta selection avoid global routing conflicts; they do not make arbitrary model mutation thread-safe. Use `GameMakerData.Gate` or a `ModelLock` as described in [Cancellation and threading](cancellation-and-threading.md).

## Companion tools

The repository holds `G3MLib.Benchmarks` and `G3MLib.ScriptCompat`. See the [overview](README.md#g3mlib) for what they do.
