# References and renaming

`ReferenceIndex` records which resources, code entries, and strings refer to which others. `RenameService` uses it to rename resources, variables, and functions together with every place that uses them.

## Build the index and find usages

```csharp
using G3MLib.DataFile;
using G3MLib.DataFile.References;

using var data = await GameMakerIO.ReadAsync("original.win");
var index = ReferenceIndex.Build(data, cancellationToken: cancellationToken);

foreach (var use in index.FindVariableUsages("score"))
{
	Console.WriteLine($"{use.OwnerName}: {use.Member}");
}
```

`Build` accepts an `IProgress<BatchProgress>` and a `CancellationToken`. `FindUsages(target)` returns every `Reference` to a resource object, `FindVariableUsages(name)` and `FindFunctionUsages(name)` search by name, and `FindUsages(target, ownerType, member)` yields lazily. After an edit, `Update(changedOwners)` refreshes only those owners instead of rebuilding the index.

## Find unreferenced items

```csharp
var unused = index.FindUnreferenced(
	UnreferencedKind.Assets | UnreferencedKind.Strings, transitive: true);
```

`UnreferencedKind` combines `Assets`, `Strings`, `Variables`, `Functions`, `EmbeddedAudio`, `TexturePageItems`, and `EmptyPages`. With `transitive: true`, an item that only unused items refer to also counts as unused. An unreferenced item is not always safe to delete: a game can look up an asset by name at run time.

## Rename

```csharp
using G3MLib.DataFile.References;

var sprite = data.Sprites!.First(item => item.Name.Content == "spr_hero");
var changes = RenameService.RenameResource(data, sprite, "spr_player", index);
Console.WriteLine($"{changes.Count} places changed");

changes.Undo();
changes.Redo();
```

`RenameResource`, `RenameVariable`, and `RenameFunction` change the name and every use, and return a `RenameChangeSet` that can be undone and redone. A refused rename throws `RenameException` before anything changes. Its `Reason` is one of:

| Reason | Meaning |
| --- | --- |
| `InvalidName` | The new name is empty, not an identifier, or a GML keyword. |
| `NameInUse` | Another asset, variable, function, or code entry already has the name. |
| `NotFound` | Nothing has the old name. |
| `Builtin` | The name belongs to a GameMaker built-in or an extension. |
| `Overlap` | Two requests in one batch change the same name. |

For many renames, build a plan first. `RenameService.PlanRenames(data, requests, index, progress, cancellationToken)` takes `RenameRequest` values (for example `RenameRequest.ForResource(resource, newName)`), checks them together, and returns a `RenamePlan`. `RenameService.Apply(plan)` then applies it and returns the change set.

## Lookups and repair

`DataLookup.Build(data)` makes hash indexes over the string table and the named resource lists. `FindString`, `GetOrAddString`, and `TryGet<T>(name, out resource, out index)` use them instead of a linear scan. When several entries share a name, the first one wins. After you rename a resource by editing `GameMakerString.Content` yourself, call `DataLookup.Build(data)` again; a stale name index cannot find a resource under its new name.

`DataRepair` fixes derived data after code edits: `RestoreMissingCodeLocals` rebuilds the local-variable tables of code entries that lost them, and `RecomputeVariableCounters` with `ApplyVariableCounters` recalculate and store the variable counters.
