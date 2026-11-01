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

## Batch export with progress and cancellation

Each `Export<Type>` method also has a form that takes a progress reporter and a cancellation token and returns one `ItemResult` per resource, with its name, a `ResultKind`, and a message for everything except a plain write:

```csharp
using G3MLib.Modding.Resources;

var progress = new Progress<BatchProgress>(
	step => Console.WriteLine($"{step.Done}/{step.Total} {step.Item}"));
var results = ResourceExportService.ExportTexturePageItems(
	data, "exported/TexturePageItems", progress, cancellationToken);
foreach (var result in results.Where(item => item.Message is not null))
{
	Console.Error.WriteLine($"{result.Name}: {result.Kind} {result.Message}");
}
```

A resource that fails is reported and skipped, so one bad item does not stop the run. Cancelling an export removes what the run wrote; cancelling an import deletes nothing, because the input folder is never changed. Import has the same batch form.

`FrameExporter.ExportPng` writes sprite frames as PNG files to paths you choose. It groups frames by texture page and drops each page from memory when its frames are done, so a large game does not need every page loaded at once. Exporting every sprite of a large game takes about 18 seconds in 1.1.0 instead of about 52.

## Sprite import and texture pages

Sprite import now uses the shared texture packer. New sprites are created in file name order with a 2 pixel border, and the border pixels are generated, so page layouts differ from those 1.0.1 produced. Importing a large sprite set is about 39 percent faster. Sound import writes each audio group file once per batch instead of once per sound.

`TexturePageRepacker` adds or replaces sprite frames and repacks texture pages:

```csharp
using G3MLib.DataFile.Util;

var repacker = new TexturePageRepacker(data);
var item = repacker.Add(newFrame);
repacker.Replace(existingItem, replacementFrame);
repacker.Repack();
```

`Plan(RepackOptions)` computes the result without changing the model; `Apply(plan)` then applies it. `RepackOptions` sets the page size, the border, whether pages are powers of two, the maximum item size, which texture groups to repack, a predicate for items that stay on their own page, and a group key. The plan reports the page count before and after. `PageEditor` edits pages directly. Planning decodes every texture page, so for a large game it takes about as long as a full sprite export.

## Audio group files

`AudioGroupFile` reads and edits an external audio group file:

```csharp
using G3MLib.Modding.Resources;

using var group = AudioGroupFile.Open("audiogroup1.dat");
var id = group.Add(File.ReadAllBytes("new-sound.ogg"));
group.Replace(0, File.ReadAllBytes("other-sound.ogg"));
group.Remove(1);
group.Compact();
group.Save("output/audiogroup1.dat", backup: true);
```

`Remove` empties an entry but keeps its id. `Compact` drops the empty entries, renumbers the rest, and returns a map from old id to new id for every kept entry. Update the sound resources that point at renumbered entries, or the game plays the wrong sound.

## GameMaker project files

G3MLib reads and writes GameMaker `.yy` files for sprites, sounds, scripts, objects, and rooms. These files are not the same as a [G3M project](projects.md) folder.

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
