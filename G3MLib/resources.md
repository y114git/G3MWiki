# Resource export and import

The resource services in `G3MLib.Modding.Resources` write editable payloads and metadata from a loaded DATA model. Import changes the model; save it explicitly afterward.

## Export selected types

```csharp
using G3MLib.DataFile;
using G3MLib.Modding.Resources;

using var input = File.OpenRead("original.win");
using var data = GameMakerIO.Read(input);
ResourceExportService.ExportTypes(
	data, "exported", "original.win", new[] { "Rooms", "Sprites" });
```

Use exact resource-type names. Supported families include:

| Family | Type names |
| --- | --- |
| Game metadata | GeneralInfo, Options, Language, FeatureFlags, Tags |
| Code and scripts | CodeEntries, Scripts, GlobalScripts |
| Graphics | Sprites, Backgrounds, Fonts, EmbeddedImages, EmbeddedTextures, TexturePageItems, TextureGroupInfo, Tilesets |
| Audio | Sounds, AudioGroups, EmbeddedAudio |
| Game behavior | GameObjects, Rooms, Paths, Timelines, Sequences, AnimationCurves |
| Other supported assets | Shaders, Extensions, FilterEffects, ParticleSystems, ParticleSystemEmitters |

Types absent from an input cannot supply assets to export. Some resources create several files, such as metadata plus images or audio. Preserve their relative paths.

Specific methods such as ExportRooms, ExportSprites, ExportFonts, ExportSounds, and ExportCodeEntries provide focused export operations. Many accept a set of resource names for selective export; ExportSounds also needs the source DATA path for companion audio.

## Import a type

```csharp
using G3MLib.DataFile;
using G3MLib.Modding.Patching;
using G3MLib.Modding.Resources;

using var input = File.OpenRead("original.win");
using var data = GameMakerIO.Read(input);
if (!ResourceImportService.Import("Rooms", data, "exported/Rooms"))
{
	throw new InvalidOperationException("Unsupported resource type.");
}
input.Dispose();
PatchInputService.WriteDataFile(data, "output/rooms-edited.win", "original.win");
```

Import takes the directory containing that type's payloads, not an individual JSON filename. For Rooms, use `exported/Rooms`; for Sprites, use `exported/Sprites`. Metadata written directly at the export root uses that root instead.

Unknown type names return false. A true return value means that the type has an importer, not that every supplied asset was imported. Some importers report invalid entries through logs, and others throw. Check the output and log before saving.

`CodeEntries` is an export type, but it has no matching generic Import case. Use [CodeImportGroup](gml.md) to compile edited code, or PatchService to apply a complete resource patch.

A whole exported resource tree can require dependency order, texture coordination, asset ordering, and code compilation. Use PatchService or ProjectContext for complete changes instead of importing every directory in arbitrary order.

`ImportAssetOrder` handles the exported ordering information. Do not reorder a resource list independently of references that use its indices.

Exported assets belong to their original rights holders. A library export is not permission to redistribute a game's resources.
