# DATA files

`G3MLib.DataFile.GameMakerData` contains typed resource collections such as Rooms, Sprites, Sounds, GameObjects, Code, and Strings. `GeneralInfo` describes the game and resource format. Availability depends on the chunks present in the file.

## Read

```csharp
using G3MLib.DataFile;

using var input = File.OpenRead("original.win");
using var data = GameMakerIO.Read(input,
	warningHandler: (warning, isImportant) => Console.Error.WriteLine(warning));
```

`GameMakerIO.Read` accepts a readable stream, optional warning and message callbacks, and `onlyGeneralInfo`. The latter requests a partial metadata read; do not use that partial model to modify or rewrite a full game file.

The warning callback receives the message and an `isImportant` flag. Keep both available if your interface distinguishes warnings that require attention from other reader messages.

Retain warning information. Some unusual or unsupported files can produce warnings or throw during parsing. Hiding a warning does not make a later write valid.

## Read and write asynchronously

```csharp
using G3MLib.DataFile;

var progress = new Progress<GameMakerProgress>(
	step => Console.WriteLine($"{step.Chunk}: {step.Fraction:P0}"));
using var data = await GameMakerIO.ReadAsync("original.win", progress, cancellationToken);
await GameMakerIO.WriteAsync("output/copy.win", data, progress, cancellationToken);
```

`ReadAsync` and `WriteAsync` run on a thread-pool thread and accept a stream or a path. Progress is reported after each chunk and once more with fraction 1 when the work is done. A cancelled call throws `OperationCanceledException`. The path overload of `WriteAsync` writes a temporary file and moves it over the target on success, so a cancelled or failed write leaves an existing target untouched. A cancelled write to a stream leaves that stream partly written. `WriteAsync` does not validate the result or handle audio groups; use `PatchInputService.WriteDataFile` below when you need that.

Chunks the library does not know are kept and written back unchanged.

## Edit and write

```csharp
using G3MLib.DataFile;
using G3MLib.Modding.Patching;

using var input = File.OpenRead("original.win");
using var data = GameMakerIO.Read(input);
if (data.Rooms is null || data.Rooms.Count == 0)
{
	throw new InvalidDataException("Expected at least one room.");
}
data.Rooms[0].Width += 16;
input.Dispose();
PatchInputService.WriteDataFile(data, "output/edited.win", "original.win");
```

WriteDataFile writes and validates the output and handles required external audio groups from the output/source locations. Keep the companion files with the DATA result when copying it into a test installation.

`GameMakerIO.Write(stream, data, messageHandler)` provides raw serialization when the host manages the stream and companion files itself. It does not package a complete game or mod.

`PatchInputService.CopyDataFile(source, output)` copies DATA and its external audio groups. It is preferable to copying only the main DATA file for a workflow that needs those groups.

## Resource references

Resource objects refer to other resources and shared strings. Reordering, deleting, or creating collections can require more than assigning a numeric field. Use resource/project import or patch services for changes with companion files and references.

`data["Rooms"]` provides a resource collection by its property name. Unknown names throw; typed properties are preferable when the resource type is known at compile time.

Dispose loaded DATA after use. Reopen the result with GameMakerIO and test with the matching game runner before distributing it.

## Share one model between threads

`GameMakerData` does not lock. A host that runs background jobs while the user edits keeps one `ModelLock` next to the model. A job that reads the model (decompile, export, search, diff, patch) holds `Read()` while it works, and every edit holds `Write()`. Several readers run together; a writer runs alone.

```csharp
using var modelLock = new ModelLock();

using (modelLock.Read())
{
	// decompile, export, search
}
using (modelLock.Write())
{
	data.Rooms![0].Width += 16;
}
```

The lock is not reentrant: taking it again on the same thread throws `LockRecursionException`. A job that must not block edits works on a snapshot, a second `GameMakerData` read from the same file. Do not edit one model from several jobs without this lock, and do not share an output path between jobs.
