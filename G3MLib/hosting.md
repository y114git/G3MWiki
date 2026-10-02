# Host integration

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

BeginScope routes messages for the current asynchronous operation and returns a disposable scope. Keep it alive until awaited work finishes. For a GUI, marshal callbacks to the UI thread; a callback is not guaranteed to arrive on that thread.

Global MessageLogged and ProgressReported events are available for process-wide listeners. Unsubscribe when the host listener is disposed. Prefer scopes when concurrent jobs require separate displays.

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

## Output and concurrency

Choose explicit output paths, reserve enough disk space for temporary materialization, and keep companion audio files with written DATA. Treat result errors and thrown exceptions as failures requiring inspection before consuming output.

Do not edit one GameMakerData instance from concurrent jobs or share an output path between them. Scoped logging and xdelta selection avoid global routing conflicts; they do not make arbitrary model mutation thread-safe.
