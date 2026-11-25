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

The warning callback receives the message and an `isImportant` flag. Keep both available if your interface distinguishes warnings that require attention from other reader messages. After the read, `data.LoadWarnings` holds every warning and `data.MayLoseDataOnSave` tells whether writing could lose data. See [Data integrity](data-integrity.md#read-warnings).

Retain warning information. Some unusual or unsupported files can produce warnings or throw during parsing. Hiding a warning does not make a later write valid.

`GameMakerData.CreateNew()` and `CreateNew2026LTS()` create an empty data file in memory.

## Read and write asynchronously

```csharp
using G3MLib.DataFile;

var progress = new Progress<GameMakerProgress>(
	step => Console.WriteLine($"{step.Chunk}: {step.Fraction:P0}"));
using var data = await GameMakerIO.ReadAsync("original.win", progress, cancellationToken);
Directory.CreateDirectory("output");
await GameMakerIO.WriteAsync("output/copy.win", data, progress, cancellationToken);
```

`ReadAsync` and `WriteAsync` run on a thread-pool thread and accept a stream or a path. Both also take the warning and message callbacks and `onlyGeneralInfo` (read) or a message callback (write). Progress is reported after each chunk and once more with fraction 1 when the work is done. A cancelled call throws `OperationCanceledException`.

The path overload of `WriteAsync` writes a temporary file next to the target and swaps it in on success, so a cancelled or failed write leaves an existing target untouched. The output folder must exist: this overload does not create it. A symlinked target keeps its link. A cancelled write to a stream leaves that stream partly written. `WriteAsync` does not handle audio groups; use `PatchInputService.WriteDataFile` below when you need that. Every data file write holds the data gate as a reader (see [Cancellation and threading](cancellation-and-threading.md#data-access-gate)).

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

`PatchInputService.WriteDataFile(data, outputPath, sourcePath = null)` creates the output folder, writes the file through a staging folder, and copies the external audio groups: groups already next to the output are kept, and missing ones are copied from the folder of `sourcePath`. Keep the companion files with the DATA result when copying it into a test installation.

`GameMakerIO.Write(stream, data, messageHandler)` provides raw serialization when the host manages the stream and companion files itself. It does not package a complete game or mod.

`PatchInputService.CopyDataFile(source, output)` copies DATA and its external audio groups. It is preferable to copying only the main DATA file for a workflow that needs those groups.

## Resource references

Resource objects refer to other resources and shared strings. Reordering, deleting, or creating collections can require more than assigning a numeric field. Use resource/project import, patch services, or the [reference tools](references.md) for changes with companion files and references.

`data["Rooms"]` provides a resource collection by its property name. Unknown names throw `MissingMemberException`; typed properties are preferable when the resource type is known at compile time. `data.ByName(name, ignoreCase = false)` finds a named resource in any list, `IndexOf(resource)` and `IndexOfByName(name)` give its index.

Dispose loaded DATA after use. Reopen the result with GameMakerIO and test with the matching game runner before distributing it.

## Model types

The model has one class per chunk (`GameMakerChunkROOM`, `GameMakerChunkSPRT`, and so on, reachable through `data.FORM`) and one class per resource in `G3MLib.DataFile.Models` (`GameMakerRoom`, `GameMakerSprite`, `GameMakerCode`, and so on). Resource classes derive from `GameMakerNamedResource` when they have a name. Names are `GameMakerString` values, so read the text from `Name.Content`.

| Property of `GameMakerData` | Content |
| --- | --- |
| `GeneralInfo`, `Options`, `Language`, `Tags`, `FeatureFlags` | Single objects |
| `Sprites`, `Backgrounds`, `Fonts`, `Sounds`, `Paths`, `Shaders`, `Timelines`, `GameObjects`, `Rooms`, `Scripts`, `Extensions`, `AnimationCurves`, `Sequences`, `ParticleSystems`, `ParticleSystemEmitters`, `FilterEffects`, `AudioGroups`, `TextureGroupInfo` | Resource lists |
| `Code`, `Variables`, `Functions`, `CodeLocals`, `Strings` | Code and string tables |
| `TexturePageItems`, `EmbeddedTextures`, `EmbeddedImages`, `EmbeddedAudio` | Texture and audio payloads |
| `GlobalInitScripts`, `GameEndScripts` | Global script lists |

Any of them can be `null` when the file has no such chunk.

Version helpers: `IsGameMaker2()`, `IsVersionAtLeast(major, minor = 0, release = 0, build = 0)`, `IsNonLTSVersionAtLeast(...)`, `IsLTSBranch()`, `IsYYC()`, `SetGMS2Version(...)` and `SetLTS(isLTS, source = null)`. `GeneralInfo.BranchSource` names the chunk that decided the LTS branch while reading. `data.ToolInfo.DecompilerSettings` holds the decompiler settings used by [GML editing](gml.md).

Faster name and string lookups are in `DataLookup`, and derived data is repaired with `DataRepair`. See [References and renaming](references.md#lookups-and-repair).

## Share one model between threads

`GameMakerData` does not lock itself for host edits. The library takes `GameMakerData.Gate` for its own bulk operations, and a host that runs background jobs while the user edits takes the same gate or keeps a `ModelLock`. See [Cancellation and threading](cancellation-and-threading.md).

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

The lock is not reentrant: taking it again on the same thread throws `LockRecursionException`. A job that must not block edits works on a snapshot, a second `GameMakerData` read from the same file. Do not edit one model from several jobs without a lock, and do not share an output path between jobs.
