# Resource export and import

The resource services in `G3MLib.Modding.Resources` write editable payloads and metadata from a loaded DATA model. Import changes the model; save it explicitly afterward.

## Resource type names

Export and import use exact type names. `ResourceTypeRegistry.AllTypes` (in `G3MLib.Modding.Internal`) lists them, and `ResourceTypeRegistry.ImportOrder` is the order a whole tree is imported in.

| Family | Type names |
| --- | --- |
| Game metadata | GeneralInfo, Options, Language, FeatureFlags, Tags |
| Code and scripts | CodeEntries, Scripts, GlobalScripts |
| Graphics | Sprites, Backgrounds, Fonts, EmbeddedImages, EmbeddedTextures, TexturePageItems, TextureGroupInfo, Tilesets |
| Audio | Sounds, AudioGroups, EmbeddedAudio |
| Game behavior | GameObjects, Rooms, Paths, Timelines, Sequences, AnimationCurves |
| Other supported assets | Shaders, Extensions, FilterEffects, ParticleSystems, ParticleSystemEmitters |

Types absent from an input cannot supply assets to export. Some resources create several files, such as metadata plus images or audio. Preserve their relative paths.

## Export selected types

```csharp
using G3MLib.DataFile;
using G3MLib.Modding.Resources;

using var input = File.OpenRead("original.win");
using var data = GameMakerIO.Read(input);
ResourceExportService.ExportTypes(
	data, "exported", "original.win", new[] { "Rooms", "Sprites" });
```

`ExportTypes(data, outputDir, dataFilePath, resourceTypes[, cancellationToken])` writes each type into `outputDir/<Type>`. It holds the data gate as a reader. `ExportSelectiveExceptCode(data, outputDir, dataFilePath, changedNames[, cancellationToken])` writes only the named resources per type and leaves out code entries.

### Per-type methods and name filters

Every type has a focused method. Those marked with a filter take a `HashSet<string>? filter` of resource names; `null` exports everything.

| Method | Filter | Notes |
| --- | --- | --- |
| `ExportGeneralInfo`, `ExportOptions`, `ExportGlobalScripts`, `ExportFeatureFlags`, `ExportLanguage`, `ExportTags`, `ExportTextureGroupInfo`, `ExportTexturePageItems` | none | One file or set per call. |
| `ExportScripts`, `ExportEmbeddedImages`, `ExportFilterEffects`, `ExportAudioGroups`, `ExportEmbeddedTextures`, `ExportSprites`, `ExportBackgrounds`, `ExportFonts`, `ExportEmbeddedAudio`, `ExportPaths`, `ExportTilesets`, `ExportShaders`, `ExportTimelines`, `ExportAnimationCurves`, `ExportSequences`, `ExportParticleSystems`, `ExportParticleSystemEmitters`, `ExportGameObjects`, `ExportRooms`, `ExportExtensions` | name filter | `ExportExtensions` takes a filter like the others, so one extension can be exported. |
| `ExportSounds(data, outputDir, dataFilePath, filter)` | name filter | `dataFilePath` locates companion audio group files. With `null`, there is no group file to read and the audio held in the data file itself (group 0) is written into each sound folder. `Import("Sounds", data, inputDir)` reads it back. |
| `ExportCodeEntries(data, outputDir, maxDegreeOfParallelism = -1)` | none | One folder per entry with a `.gml` file and, when needed, an `.asm` file. |
| `ExportCodeEntriesSelective(data, outputDir, entryNames)` | entry names | |
| `ExportCodeEntriesToMemory(data, entryNames)` | entry names | Returns the GML and assembly text per entry instead of writing files. |
| `ExportAssetOrder(data, outputDir, ...)` | sections | Writes the ordering helpers that `ImportAssetOrder` reads. |

Export folder names are made safe for the file system. A name that needed a character replaced (such as `a:b`) gets a 12 character hash suffix (`a_b__<hash>`), so `a:b` and `a_b` get different folders. A name that already ends in `__` plus 12 lowercase hex digits gets a hash suffix as well, so it cannot equal the generated folder of another resource. The names `.` and `..`, trailing dots or spaces and Windows device names are made safe the same way, and a colon is replaced on every platform. Exports from an earlier version keep their folder names and still import. Import reads the resource name from the JSON `name`, so `a:b` imports as `a:b` and not as its folder name; old exports without a `name` import by their folder name.

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

`ResultKind` is `Written`, `Skipped`, `Warning` or `Failed`. The batch forms take `ISet<string>? filter` where the plain forms take a `HashSet<string>`. A resource that fails is reported and skipped, so one bad item does not stop the run. Cancelling an export removes what the run wrote; cancelling an import deletes nothing, because the input folder is never changed. Import has the same batch form. See [Cancellation and threading](cancellation-and-threading.md).

`FrameExporter.ExportPng(jobs, maxPagesInFlight, progress, cancellationToken)` writes sprite frames as PNG files to paths you choose. Each `FrameJob(Item, Path, Padding)` names a texture page item and its target path. It groups frames by texture page and drops each page from memory when its frames are done, so a large game does not need every page loaded at once.

## Sprite import and texture pages

Sprite import uses the shared texture packer. New sprites are created in file name order with a 2 pixel border, and the border pixels are generated. Sound import writes each audio group file once per batch instead of once per sound.

`TexturePageRepacker` adds or replaces sprite frames and repacks texture pages:

```csharp
using G3MLib.DataFile.Util;

var repacker = new TexturePageRepacker(data);
var item = repacker.Add(newFrame);
repacker.Replace(existingItem, replacementFrame);
repacker.Repack();
```

`Plan(RepackOptions)` computes the result without changing the model; `Apply(plan)` then applies it. `RepackOptions` sets the page size (`MaxPageSize`), the `Border`, whether pages are powers of two (`PowerOfTwo`, default true), `MaxItemSize`, which texture groups to repack (`Groups`), a predicate for items that stay on their own page, and a group key. The plan reports the page count before and after (`PageCount`), `GroupCount`, and `Pages` with the size, fill ratio and item count of every planned page (`RepackPagePlan`). `PageEditor` edits pages directly. Planning decodes every texture page, so for a large game it takes about as long as a full sprite export.

`GameMakerTexturePageItem.ReplaceTexture(image)` swaps an item's image. `PrepareReplace(image)` does the image work and returns a `PreparedTextureReplace`; its `Commit()` only swaps the page image, so a host can prepare outside a write lock. `Commit()` throws `InvalidOperationException` when the page image changed or the item was pointed at another page or cleared.

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

`Remove` empties an entry but keeps its id. `Compact` drops the empty entries, renumbers the rest, and returns a map from old id to new id for every kept entry. Update the sound resources that point at renumbered entries, or the game plays the wrong sound: `SoundIdRemap.Apply(data, groupId, map)` does that. `GetEntry(id)` and `Count` read entries.

`Save` writes through a temporary file. Saving over the opened file throws an `IOException` that says the file was saved when reopening it fails; dispose the instance then. The path comparison is case sensitive on Linux.

`AudioGroupReader(data, dataFilePath)` finds the audio of a sound: `GetAudio(sound, out AudioSource source)` reads from the data file or from the group file. A `null` data file path reads only the audio held in the data file (group 0).

## GameMaker project files

G3MLib reads and writes GameMaker `.yy` files for sprites, sounds, scripts, objects, and rooms (`YySprite`, `YySound`, `YyScript`, `YyObject`, `YyRoom`, with `YyFile.Parse` and `YyFile.Write`). These files are not the same as a [G3M project](projects.md) folder.

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

Import takes the directory containing that type's payloads, not an individual JSON filename. For Rooms, use `exported/Rooms`; for Sprites, use `exported/Sprites`. Passing the export root also works when it holds a folder named after the type. Metadata written directly at the export root uses that root instead.

Unknown type names return false. A true return value means that the type has an importer, not that every supplied asset was imported. Every import holds the data gate as a writer.

| Overload | Result |
| --- | --- |
| `Import(type, data, inputDir)` | `bool`. |
| `Import(type, data, inputDir, cancellationToken)` | `bool`. |
| `Import(type, data, inputDir, progress, cancellationToken)` | One `ItemResult` per item. An unknown type throws `ArgumentException`. |
| `Import(type, data, inputDir, only, progress = null, cancellationToken = default)` | Only the named entries of one type. `only = null` imports everything. |
| `Import(data, inputDir, only, progress = null, cancellationToken = default)` | Named entries per type, in dependency order. Every type is checked before anything is imported, so an unsupported type does not leave a half-applied import. |

Types that do not support a name filter: `Language`, `Options`, `GlobalScripts`, `FeatureFlags`, `Tags`, `GeneralInfo` and `TexturePageItems`. Asking for a filter on them throws `NotSupportedException`. Filters match the resource name and the folder name, in both the current and the earlier folder name form.

### Code entries

`Import("CodeEntries", data, inputDir)` compiles the `.gml` file of each exported code entry folder. Entries get one result each in the batch forms, and a name filter selects entries. Entries that have only an `.asm` file are skipped (the library cannot assemble them), as are folders without a `.gml` file and entries whose name occurs more than once in the data file. In the plain form, a compile failure throws `InvalidOperationException` with the failures. Child entries are rebuilt by the compiler from their root entry. A project import uses the same path. For edits made from text in memory, use [CodeImportGroup](gml.md#queue-code-changes).

A whole exported resource tree can require dependency order, texture coordination, asset ordering, and code compilation. Use PatchService, `G3MProjectService` or ProjectContext for complete changes instead of importing every directory in arbitrary order.

`ResourceImportService.ImportAssetOrder(data, inputDir[, cancellationToken])` applies the exported ordering information and holds the gate as a writer. Do not reorder a resource list independently of references that use its indices.

Exported assets belong to their original rights holders. A library export is not permission to redistribute a game's resources.
