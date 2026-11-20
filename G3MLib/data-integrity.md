# Data integrity and hashing

This page covers the checks that tell you whether a loaded data file is complete, what the reader reports about a file it could not fully model, and the hash helpers that compare files and resources.

## Read warnings

```csharp
using G3MLib.DataFile;

using var data = await GameMakerIO.ReadAsync("original.win");
foreach (var warning in data.LoadWarnings)
{
	Console.Error.WriteLine($"{warning.Chunk} at {warning.Offset}: {warning.Message}");
}
if (data.MayLoseDataOnSave)
{
	Console.Error.WriteLine("Saving this file may not reproduce everything that was read.");
}
```

| Member of `GameMakerData` | Meaning |
| --- | --- |
| `LoadWarnings` | A snapshot of the warnings the reader raised, in order. Empty for a new data file and for a file that read cleanly. |
| `MayLoseDataOnSave` | `true` when the reader skipped or could not model data: an unknown chunk, an unknown or skipped object, an unsupported bytecode version, or an important warning. |
| `UnsupportedBytecodeVersion` | `true` for a bytecode version the library does not support. |

`LoadWarning(Chunk, Message, IsImportant, Offset = 0)` is a record. The `warningHandler` of `GameMakerIO.Read` receives the same text as it is raised.

`GameMakerGeneralInfo.BranchSource` names the chunk whose format decided the LTS branch while reading. It is `null` when the branch was not detected (a new data file, or one set by hand), and it is the evidence for the branch, because the file does not store it. `GameMakerGeneralInfo.VersionSource` does the same for the runtime version: it is `null` when the version is the one GEN8 stored or was set by hand. `GameMakerData.SetLTS(bool isLTS, string source = null)` sets the branch and its source.

## Integrity check

`DataIntegrityService` in `G3MLib.Modding.Integrity` has two entry points:

| Method | Effect |
| --- | --- |
| `RepairAndValidate(data)` | Applies the repairs the library knows, then validates. |
| `ValidateOnly(data)` | Validates and changes nothing. |

```csharp
using G3MLib.DataFile;
using G3MLib.Modding.Integrity;

using var data = await GameMakerIO.ReadAsync("original.win");
var result = DataIntegrityService.ValidateOnly(data);
foreach (var issue in result.Issues)
{
	Console.WriteLine($"{issue.Severity} {issue.Code} [{issue.Chunk}] {issue.Resource}: {issue.Message}");
}
Console.WriteLine(result.Success);
```

`DataIntegrityResult`:

| Member | Content |
| --- | --- |
| `Errors` | Messages of problems that were found and not fixed. |
| `Repairs` | Messages of fixes that were applied. |
| `Issues` | Every error and repair as an `IntegrityIssue`, in the order found. |
| `Success` | `true` when `Errors` is empty. |

`IntegrityIssue(Severity, Code, Chunk, Resource, Message)`: `Severity` is `IntegritySeverity.Repair` or `IntegritySeverity.Error`. `Code` is a stable id of the form `CHUNK.Problem`, such as `ROOM.MissingSprite`. `Chunk` is the four letter chunk name. `Resource` is the name of the owning resource and is `null` for repairs. `Message` is the same text as in `Errors` or `Repairs`.

Codes the check reports as errors:

| Chunk | Codes |
| --- | --- |
| BGND | `MissingSprite`, `MissingTexturePageItem` |
| CODE | `ChildMissingFromList`, `ChildWrongParent`, `MissingFunction`, `MissingParent`, `ParentLacksChild` |
| FONT | `MissingTexturePageItem` |
| GEN8 | `RoomOrderMissingRoom` |
| GLOB | `MissingCode` |
| OBJT | `MissingCode`, `MissingCollisionObject`, `MissingParent`, `MissingSprite`, `MissingTextureMask`, `ParentCycle`, `TooFewEventLists` |
| PSEM | `MissingSpawnOnDeath`, `MissingSpawnOnUpdate`, `MissingSprite` |
| ROOM | `InvalidTileData`, `MissingCreationCode`, `MissingInstanceObject`, `MissingLayerSprite`, `MissingParticleSystem`, `MissingPreCreateCode`, `MissingSequence`, `MissingSprite`, `MissingTileBackground`, `MissingTileSprite`, `MissingViewObject` |
| SCPT | `MissingCode` |
| SOND | `GroupIdOutOfRange`, `MissingAudioGroup` |
| SPRT | `MissingTexturePageItem` |
| TPAG | `MissingTexture` |

Codes `RepairAndValidate` reports as repairs: `CODE.RelinkedFunctionRefs`, `CODE.RelinkedLocalChildFunctions`, `CODE.RelinkedParent`, `CODE.ReorderedSerialization`, `CODE.RestoredChildLinks`, `FUNC.RemovedInvalid`, `FUNC.RestoredMissing`, `GEN8.Branch`, `ROOM.RelinkedCode`, `SCPT.CreatedForCode`, `SCPT.RelinkedCode`, `SOND.ClearedEmptyType`, `SOND.ClearedExternalAudio`, `SOND.RelinkedBuiltinAudio`.

The library runs the check itself in these places:

- `G3MProjectService.Build` runs `RepairAndValidate` after importing and adds the errors to `G3MProjectResult.Warnings`.
- Patch apply validates the data it wrote. Merge repairs the original data it works on.
- `DataRepair` (see [References and renaming](references.md#lookups-and-repair)) fixes code locals and variable counters after code edits.

The check also logs the first twelve repairs and errors through `LogService`.

## Hashing

| Helper | Use |
| --- | --- |
| `ChunkDigests.Compute(stream, algorithm = null, token = default)` | Hashes the body of every chunk of a data file without parsing it. SHA-256 by default. The stream must be readable and seekable and positioned at the start. Returns one `ChunkDigest(Name, Offset, Length, Hash)` per chunk in file order. Offsets are relative to the start of the FORM chunk. The hash is lower case hex. |
| `ChunkDigests.Compare(a, b)` | Names of the chunks added (only in `b`), changed, or removed (only in `a`). Digests must use the same algorithm. Names found in `b` come first, then the removed ones. |
| `FileHash.ComputeMd5(path)`, `ComputeMd5Async(path, token)` | Lower case hex MD5 of a file. |
| `FileHash.HaveSameSize(a, b)` | Compares file sizes. |
| `HashService.ComputeFileHash`, `ComputeFileHashAsync`, `FileSizesMatch` | The same helpers under the older names. |
| `ResourceHashService.HashAll(data, dataFilePath = null)` | Hash per resource: a dictionary of resource type to a dictionary of resource name to hash. Patch creation and diff use it. |

```csharp
using G3MLib.Modding.Hashing;

using var first = File.OpenRead("original.win");
using var second = File.OpenRead("modified.win");
var changed = ChunkDigests.Compare(
	ChunkDigests.Compute(first), ChunkDigests.Compute(second));
Console.WriteLine(string.Join(", ", changed));
```

## See also

- [DATA files](data-files.md)
- [Patch API](patches.md)
- [API index](api-index.md)
