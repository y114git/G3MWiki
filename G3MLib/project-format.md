# Project format reference

A G3M project is a `<Name>.g3mproj` file plus one folder per resource, laid out like a GameMaker project. `G3MProjectService` in `G3MLib.DataFile.Project` writes it from a data file, updates single resources, and rebuilds a data file from it. For a walkthrough see [Projects and scripts](projects.md#g3m-project-g3mproj).

The format version is `G3MProjectService.FormatVersion` (1). The file extension is `G3MProjectService.FileExtension` (`.g3mproj`). A project with a newer format version, or one that cannot be parsed, makes `Build`, `Update` and `OpenAsync` throw `ProjectException`.

## Folder layout

Each modding resource type has one folder in the project folder. Resource files come from the same serializers as the [resource export](resources.md), so a resource folder has the same content in a project, an export and a patch.

| Resource type | Folder |
| --- | --- |
| Sprites | `sprites` |
| Backgrounds | `backgrounds` |
| Fonts | `fonts` |
| Sounds | `sounds` |
| Paths | `paths` |
| Tilesets | `tilesets` |
| Shaders | `shaders` |
| Timelines | `timelines` |
| GameObjects | `objects` |
| AnimationCurves | `animcurves` |
| ParticleSystemEmitters | `emitters` |
| ParticleSystems | `particles` |
| Sequences | `sequences` |
| Rooms | `rooms` |
| Extensions | `extensions` |
| Scripts | `scripts` |
| CodeEntries | `code` |
| EmbeddedTextures | `textures` |
| EmbeddedAudio | `audio` |
| EmbeddedImages | `images` |
| Strings | `strings` |
| Variables | `variables` |
| Functions | `functions` |
| UILayers | `uilayers` |
| GeneralInfo | `options/GeneralInfo` |
| Options | `options/Options` |
| GlobalScripts | `options/GlobalScripts` |
| Language | `options/Language` |
| FeatureFlags | `options/FeatureFlags` |
| Tags | `options/Tags` |
| FilterEffects | `options/FilterEffects` |
| AudioGroups | `options/AudioGroups` |
| TextureGroupInfo | `options/TextureGroupInfo` |

Texture page items have no folder: they are derived from sprites and fonts. Text files in a project use the extensions `.json`, `.gml` and `.asm`. A resource whose name is shared by several resources has a folder name with an `__idxNNNN` suffix.

## The project file

The file is indented JSON in UTF-8 without a byte order mark. Property names are camelCase, except `Folders`, `AudioGroups` and `TextureGroups`.

| Property | Content |
| --- | --- |
| `resourceType` | Always `G3MProject`. |
| `name` | Project name. |
| `formatVersion` | `1`. |
| `mode` | `full` or `delta`. |
| `baseFile` | `name`, `size` and `md5` of the data file the project was exported from. |
| `resources` | One entry per resource: `id` (`name` and `path`), `type` and `hash`. The hash is SHA-256 over the resource's files as exported. |
| `Folders` | Folder names carried over from an existing project file. Empty for a new project. |
| `deleted` | Resources to remove on build: `type` and `name`. |
| `resourceOrder` | Resource names per type in data file order, so a build restores the order. |
| `AudioGroups`, `TextureGroups` | Names of the audio groups and texture groups. |

`G3MProject` (returned by `OpenAsync`) exposes the same data read only: `Name`, `FormatVersion`, `Mode`, `Directory`, `BaseFile` (a `G3MProjectBaseFile` with `Name`, `Size`, `Md5`), `Resources` (`G3MProjectResource` with `Name`, `Path`, `Type`, `Hash`), `Folders`, `ResourceOrder`, `AudioGroups` and `TextureGroups`, plus `OfType(type)`, `Find(path)` and `GetPath(resource)`.

## Export and update

```csharp
using G3MLib.DataFile;
using G3MLib.DataFile.Project;

using var data = await GameMakerIO.ReadAsync("original.win");
var result = G3MProjectService.Export(data, "project", "original.win",
	new G3MProjectOptions { Name = "Interface" });
Console.WriteLine($"{result.Resources} resources: {result.Path}");
foreach (var warning in result.Warnings)
{
	Console.Error.WriteLine(warning);
}
```

Signature: `Export(GameMakerData data, string projectDirectory, string? baseDataFilePath, G3MProjectOptions options, IProgress<BatchProgress>? progress = null, CancellationToken cancellationToken = default)`. `Update` has the same parameters.

| G3MProjectOptions | Default | Effect |
| --- | --- | --- |
| `Name` | `Project` | Project name and file name (`<Name>.g3mproj`). |
| `Mode` | `ProjectExportMode.Full` | `Full` writes every resource. `Delta` writes only the resources in `Only`. |
| `Only` | `null` | Resource names per type, for example `"Sprites"`. A type without an entry is skipped. Required for `Delta` and for `Update`, otherwise `ArgumentException`. |
| `Deleted` | `null` | Resources removed from the game on build, recorded in the project file. `null` keeps the deletions already recorded. |

| G3MProjectResult | Meaning |
| --- | --- |
| `Path` | The project file (export) or the written data file (build). |
| `Resources` | Resources written (export, update) or imported (build). |
| `Unchanged` | Resources a build left alone. |
| `Warnings` | Non-fatal problems. A resource that cannot be exported is listed here. |

Behaviour:

- Export holds the data gate as a reader. See [Cancellation and threading](cancellation-and-threading.md#data-access-gate).
- Resources are written into a `.staging-<id>` folder inside the project folder and moved into place at the end. A cancelled export leaves the project folder as it was, and a project folder the call created is removed again.
- Moving the staging folders and deleting the replaced ones is retried for a short time, so a virus scanner or a sync tool that holds fresh files does not fail the export. A folder that still cannot be renamed is copied, and a leftover staging folder never turns a finished export into a failure.
- The project file is written through a temporary file, so a failed write keeps the old file.
- `Update` writes only the resources named in `Only`, with the files a full export writes for them. It keeps the project's mode. A name in `Only` that the data no longer has (an old name after a rename, or a deleted resource) is removed from the project. Types whose files hold a whole list (strings, variables, functions) are rewritten when named, whatever names are listed. A name shared by several resources matches all of them. Folders of other resources keep their recorded hashes, so a file edited by hand still counts as changed on the next build. `Update` throws `ProjectException` when the folder has no `<Name>.g3mproj`.

## Build

`Build(string projectFile, string baseDataFilePath, string outputDataFilePath)` reads the project file and imports the changed resources over the base data file, then writes `outputDataFilePath` (missing folders are created, the write goes through a temporary file).

- When the base file matches the recorded size and MD5 and the project mode is `full`, resources whose files still match their recorded hash are skipped, so an untouched project rebuilds the base file byte for byte. `G3MProjectResult.Unchanged` counts them.
- When the base file differs, a warning says so and every resource is imported over it.
- Resources listed in `deleted` are removed first. A deleted name the data file does not have adds a warning.
- A resource in the project file whose folder is missing stays in the data file and adds a warning.
- After importing, the integrity check repairs and validates the data. Its errors are added to `Warnings`. See [Data integrity](data-integrity.md).
- Build holds the data gate as a writer.

## Open for an editor

`G3MProjectService.OpenAsync(projectFile, progress = null, cancellationToken = default)` returns a `G3MProject` without reading the resource folders, so the time depends on the project file only. Progress ends at fraction 1.

## See also

- [Projects and scripts](projects.md)
- [Resource export and import](resources.md)
- [API index](api-index.md)
