# Patch API

Patch services are in `G3MLib.Modding.Patching`. Their results expose `Success` and `Error`; check those values before using a generated output.

## Create

```csharp
using G3MLib.Modding.Patching;

var result = await PatchService.CreatePatchAsync(
	"original.win", "modified.win", "interface.g3mpatch");
if (!result.Success)
{
	throw new InvalidOperationException(result.Error);
}
Console.WriteLine(result.Statistics?.TotalChanged);
```

The first two arguments are readable DATA files. The output is a resource patch archive. Optional `includeXdeltaFallback: true` embeds a binary fallback and requires a configured xdelta executable. Optional `cacheOptions` controls reusable analysis.

Optional precomputed inputs (`precomputedOriginalHashes`, `precomputedOriginalInfo`, `precomputedOriginalNameCounts`, `precomputedOriginalOrderedNames`, `precomputedModifiedData`) are available for a host that already has the corresponding analysis. The four `precomputedOriginal...` inputs must describe the supplied original file. `precomputedModifiedData` must be the loaded data of the supplied modified file. Omit all of them in ordinary usage. `PatchService.GetResourceNameCountsForReuse(data)` and `GetOrderSensitiveResourceNamesForReuse(data)` produce the name inputs from a loaded model.

| PatchCreateResult | Content |
| --- | --- |
| `Success`, `Error` | Outcome. |
| `Message` | Extra text, when there is any. |
| `Statistics` | A `PatchStatistics`: `TotalChanged`, `TotalNew`, `TotalDeleted`, `TotalChangedFiles`, `TotalNewFiles`. |

## Validate

```csharp
using G3MLib.Modding.Patching;

var validation = await PatchService.ValidatePatchAsync(
	"interface.g3mpatch", dataPath: "original.win");
if (!validation.Success)
{
	throw new InvalidOperationException(validation.Error);
}
Console.WriteLine(validation.Manifest?.Original?.Filename);
```

The optional original adds compatibility checks. Without it, the service validates the package rather than proving compatibility with an arbitrary game file. The successful result provides the parsed Manifest.

## Apply

```csharp
using G3MLib.Modding.Patching;

var applied = await PatchService.ApplyPatchAsync(
	"original.win", "interface.g3mpatch", "output/patched.win");
if (!applied.Success)
{
	throw new InvalidOperationException(applied.Error);
}
```

`allowXdeltaFallback` defaults to false and controls trying the embedded binary alternative first. `verifyModifiedHash` defaults to true; retain normal checking unless the host has a specific, documented reason to change it. The check compares the output with the manifest's modified hash only when `allowXdeltaFallback` is also true, so with the default `false` it has no effect. Resource reconstruction and binary reproduction have different guarantees.

The output can include external audio groups. Test it with the corresponding runner and resources rather than assuming the main `.win` file is the whole game.

Create, validate, and apply accept an optional `cancellationToken` as their last parameter. A cancelled apply keeps an existing output as it was. See [Cancellation and threading](cancellation-and-threading.md).

| PatchApplyResult | Content |
| --- | --- |
| `Success`, `Error` | Outcome. A failure caught inside the call is returned here, not thrown. |
| `AppliedCount`, `FailedCount` | Counts of applied and failed operations. |
| `Warnings` | Non-fatal problems. |

## Apply to a loaded model

```csharp
using G3MLib.DataFile;
using G3MLib.Modding.Patching;

using var data = await GameMakerIO.ReadAsync("original.win");
var applied = await PatchService.ApplyPatchAsync(data, "interface.g3mpatch",
	new PatchApplyOptions { ValidateBeforeApply = true });
if (!applied.Success)
{
	throw new InvalidOperationException(applied.Error);
}
PatchInputService.WriteDataFile(data, "output/patched.win", "original.win");
```

`ApplyPatchAsync(GameMakerData data, string patchPath, PatchApplyOptions? options = null, IProgress<BatchProgress>? progress = null, CancellationToken cancellationToken = default)` patches the model in place and writes no data file. A second overload takes a `Stream` instead of a path: the caller keeps ownership of the stream, and seekable and non-seekable streams both work.

- With `ValidateBeforeApply` (default `true`), the manifest, every JSON file, the resource types and the required helpers are checked before the model is changed, so an invalid patch is refused cleanly.
- If the apply fails or is cancelled after the resource import has begun, the model may be partly changed. Discard it or reload it.
- Nothing is written to disk, so audio groups stored outside the data file are skipped. `PatchApplyOptions.DataFilePath` records the path of the data file and does not change that. Save the model with `PatchInputService.WriteDataFile` and keep the audio group files with it.
- A patch with code entries needs `variables_functions.json` or `code_patch_metadata.json` in its helper folder, otherwise validation fails.

## Read a patch archive

`PatchFileSystem.LoadFromZip(path or stream, loadExactPayloads = true, skipAsmBackedGml = false)` loads a patch archive into memory. The loader enforces these rules and throws `InvalidDataException` when one is broken:

- An entry name must be relative, hold no drive or stream colon, and have no `..` segment.
- The archive's entries may declare at most `PatchFileSystem.MaxTotalPayloadBytes` in total (4 GiB by default, settable, `long.MaxValue` removes the limit). One entry or the whole archive may not exceed `PatchFileSystem.MaxPayloadBytes` (2,147,483,591 bytes).
- `g3mpatch.json` may be at most `PatchFileSystem.MaxManifestBytes` (64 MiB).
- A corrupt `g3mpatch.json`, or one that is JSON `null` or empty, is refused with the patch path in the message. Apply, diff and merge turn this into an error result.

Members of a loaded `PatchFileSystem`:

| Member | Content |
| --- | --- |
| `Manifest` | The parsed `G3MPatchManifest`. |
| `HelpersPrefix` | Folder name of the helper files, `Helpers` by default. |
| `GmlEntries`, `AsmEntries`, `AsmEntryPaths`, `CodeEntryLogicalNames` | Code entries of the archive. `CodeEntryCount` and `HasCodeEntries()` summarize them. |
| `ExactPatchBytes`, `ExactPatchPath` | The embedded binary fallback, when loaded. |
| `FileCount`, `GetAllFilePaths()`, `GetAllFiles()` | All files. |
| `ReadAllBytes`, `ReadAllText`, `ReadAllLines`, `FileExists`, `DirectoryExists`, `GetDirectories`, `GetFiles`, `TryGetFile` | Read access with archive-relative paths (case insensitive). |
| `AddFile`, `AddTextFile`, `RemoveFile`, `AddGmlEntry`, `AddAsmEntry`, `RemoveGmlEntry`, `RemoveAsmEntry`, `RebuildDirectoryIndex` | Build or change an archive in memory. |
| `HasResourceType(type)`, `GetResourceTypes()` | Which resource types the archive holds. |
| `SaveToZip(outputPath, manifest = null)` | Writes the archive through a temporary file and creates the folder. |
| `ReleaseFileData()`, `ReleaseCodeEntries()` | Free memory after use. |

## Manifest

`g3mpatch.json` deserializes to `G3MPatchManifest`:

| Property | Content |
| --- | --- |
| `CreatedAt` | Creation time. |
| `Tool`, `Library` | `ToolInfo` with `Name` and `Version`. `Tool` is the application (`ModdingMetadata.ToolName`, `ToolVersion`), `Library` is G3MLib and its version. |
| `Original`, `Modified` | `DataFileInfo`: `Filename`, `Size`, `Md5`, `BytecodeVersion`, `GmsVersion`, `GeneralInfo` (a `GeneralInfoData` with the display name, name, file name, config, game id, version numbers, window size and similar fields). |
| `Resources` | Per resource type, a `ResourceTypeChanges` with `Changed` and `New` (lists of `ResourceChange` with `Name` and `Files`), `Deleted` (names) and `HasChanges`. |
| `Statistics` | The `PatchStatistics` totals. |
| `ApplyPlan` | A `PatchApplyPlan`: `Mode` and flags for the pipelines a patch needs (`RequiresCodePipeline`, `RequiresTexturePipeline`, `RequiresAssetReorder`, `RequiresHeavyFinalize`, `SupportsDirectResourceApply`) with `SimpleResourceTypes` and `HeavyResourceTypes`. |

Set `ModdingMetadata.ToolName` and `ToolVersion` once at start-up so that patches record your application. See [Host integration](hosting.md#tool-name-and-version).

## Normalize another input

`EnsureG3MPatchAsync(originalPath, inputPath, tempDir, ..., cacheOptions)` returns a resource-patch path. `PatchInputService.MaterializeDataAsync(originalPath, inputPath, tempDir)` returns derived DATA for supported DATA/resource/binary patch inputs. `PatchInputService.IsXDelta(path)` tests the file extension, `CopyExternalAudioGroups(sourceDataPath, materializedDataPath)` copies audio group files, and `WriteDataFile` and `CopyDataFile` are described in [DATA files](data-files.md#edit-and-write).

Provide a dedicated temporary directory and retain it until the returned file has been consumed. These methods do not provide the G3MTool CLI's CSX execution layer. A library host must execute scripts through its selected script environment (see [Scripts and ScriptGlobals](script-globals.md)) before passing their DATA result to ordinary patch services.

The [patch format](../G3MTool/patch-format.md) describes the archive's user-relevant contents. It is separate from a G3M mod package.
