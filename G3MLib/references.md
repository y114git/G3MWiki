# References and renaming

`ReferenceIndex` records which resources, code entries, and strings refer to which others. `RenameService` uses it to rename resources, variables, and functions together with every place that uses them. `ReferenceIndex.ClearUsages` prepares a resource for deletion, and `ResourceCloneService` copies a resource under a new name.

All types are in `G3MLib.DataFile.References`.

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

`Build` accepts an `IProgress<BatchProgress>` and a `CancellationToken`. `FindUsages(target)` returns every `Reference` to a resource object, `FindVariableUsages(name)` and `FindFunctionUsages(name)` search by name, and `FindUsages(target, ownerType, member)` yields lazily. After an edit, `Update(changedOwners)` refreshes only those owners instead of rebuilding the index. `ReferenceCount` and `Data` describe the index.

`Reference(Target, Owner, Holder, Member, InstructionOffset = -1)` is a record struct. `OwnerName` is the name of the owning resource. `InstructionOffset` is set for a use inside code.

Other lookups:

| Member | Result |
| --- | --- |
| `ReferenceIndex.FindTileUses(data, tileSet, tileIndex)` | Every tile layer cell that uses a tile of a tileset, as `TileUse(Room, Layer, X, Y)`. |
| `index.ClassifyString(GameMakerString)` | A `StringUse`: `ResourceName`, `CodeLiteral`, `VariableName`, `FunctionName`, `Other` or `Unused`. |
| `CodeScan.Scan(data, code)` | The variables, functions, string literals and asset references of one code entry as a `CodeScanResult`. |
| `AudioIndex.Unreferenced(data, groupId, entryCount)` | Ids of audio entries in a group that no sound uses. |

## Find unreferenced items

```csharp
var unused = index.FindUnreferenced(
	UnreferencedKind.Assets | UnreferencedKind.Strings, transitive: true);
```

`UnreferencedKind` combines `Assets`, `Strings`, `Variables`, `Functions`, `EmbeddedAudio`, `TexturePageItems`, and `EmptyPages`. With `transitive: true`, an item that only unused items refer to also counts as unused. An unreferenced item is not always safe to delete: a game can look up an asset by name at run time. `FindUnreferenced(params Type[] assetTypes)` returns unused assets of the given types (all asset types when none are given).

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

For many renames, build a plan first. `RenameService.PlanRenames(data, requests, index, progress, cancellationToken)` takes `RenameRequest` values (`RenameRequest.ForResource(resource, newName)`, `ForVariable(old, new)`, `ForFunction(old, new)`), checks them together, and returns a `RenamePlan` with `Changes`, `Conflicts` (`RenameConflict(Request, Reason, Message)`) and `Uses`. `RenameService.Apply(plan)` then applies it and returns the change set. A `RenameChange` records the `GameMakerString`, the old and new value and the location. `RenameChangeSet.Uses` lists the uses it touched.

Check a name before you ask for it:

| Member | Result |
| --- | --- |
| `RenameService.IsValidName(name)` | `true` when the text is an identifier (letters, digits and underscores, not starting with a digit) and not a GML keyword. Names of built-in functions and variables are not checked. |
| `RenameService.IsNameInUse(data, name)` | `true` when an asset or a code entry has the name. The comparison ignores case, because GameMaker treats names that differ only in case as one. |

## Clear usages before a delete

```csharp
var cleared = index.ClearUsages(sprite);
foreach (var use in cleared.Skipped)
{
	Console.Error.WriteLine($"Still used in code: {use.OwnerName}");
}
if (cleared.Skipped.Count == 0)
{
	data.Sprites!.Remove(sprite);
}
else
{
	cleared.Undo();
	index.Update(cleared.ChangedOwners);
}
```

`Undo` restores the model but not the index, so the example updates the index for the changed owners afterwards. This also applies when `Skipped` is not empty, because the uses that were cleared before the undo are still in `Cleared`.

`ClearUsages(target)` sets every property of the model that points at the target to none and removes the target from lists that hold it (a texture group, the room order). That makes the target safe to remove from its own list. Uses inside code are left alone and returned in `ClearedReferences.Skipped`: an instruction cannot lose its operand safely, so change that code first.

| ClearedReferences member | Meaning |
| --- | --- |
| `Cleared` | The uses that were cleared. |
| `Skipped` | The uses in code that were left alone. |
| `ChangedOwners` | The owners whose references were cleared, once each. Pass them to `ReferenceIndex.Update` after `Undo`, and refresh them in a UI. |
| `Undo()` | Puts the properties and list entries back, last change first. |

If one use cannot be cleared, the ones already cleared are put back and `InvalidOperationException` is thrown. The call updates the index for the changed owners and holds the data gate as a writer. `Undo` does not take the gate: call it inside a write scope when a save or script can run at the same time. See [Cancellation and threading](cancellation-and-threading.md#data-access-gate).

## Clone a resource

```csharp
var clone = ResourceCloneService.Clone(data, sprite, "spr_player_copy",
	timeout: TimeSpan.FromSeconds(5));
Console.WriteLine($"Added {clone.Added.Count} objects; shares {clone.SharedCode.Count} code entries");

clone.Undo();
```

`Clone(data, source, newName, timeout = null, cancellationToken = default)` copies a resource inside one data file, without an export folder, and adds the copy to its list. It supports objects, scripts, sprites, paths, animation curves, sounds, fonts, shaders, timelines, backgrounds, rooms, sequences, particle systems and extensions.

- The copy owns its events, layers, instances, frames, tracks and masks. It keeps pointing at the same other resources, texture page items (so a copied sprite shares pixels until you replace them), variables, functions and strings.
- Code entries and code locals whose names derive from the resource name are copied and renamed with the `RenameService` rules. The function entries of a copied script or object are copied too, so a copied script can be called. Child code entries get unique names. Embedded sound audio is copied.
- A room copy gets new instance ids above every id in the file, even when `GeneralInfo.LastObj` is lower, and joins the room order.
- `ResourceClone.Resource` is the copy, `Added` lists every object the call added (the resource, its code entries and locals, audio entries, particle emitters, and strings new to the file), and `SharedCode` lists code entries the copy points at but does not own, because their names do not follow the resource name. Replacing their code changes the source too.
- `ResourceClone.Undo()` removes everything in `Added`. Nothing else may refer to the copy yet: use `ClearUsages` first if it does. Edits made after the clone are kept: instance and tile ids are rewound only while they still hold the values the clone set, and a string is removed only while it is the last one in the file.

Errors: `RenameException` for an invalid or used name, `ArgumentException` when the source is not in the data file, `TimeoutException` when the gate is not free in time, and `NotSupportedException` for code entries, functions, variables, locals, audio groups, texture groups, filter effects, embedded audio and embedded textures. The call holds the gate as a writer, so a host on a UI thread should pass a timeout or a token. The token stops only the wait for the gate.

## Lookups and repair

`DataLookup.Build(data)` makes hash indexes over the string table and the named resource lists. `FindString`, `GetOrAddString`, and `TryGet<T>(name, out resource, out index)` use them instead of a linear scan; `Add<T>(item)` registers a resource you added. When several entries share a name, the first one wins. After you rename a resource by editing `GameMakerString.Content` yourself, call `DataLookup.Build(data)` again; a stale name index cannot find a resource under its new name.

`DataRepair` fixes derived data after code edits: `BuildCodeLocals` and `RestoreMissingCodeLocals` rebuild the local-variable tables of code entries that lost them, and `RecomputeVariableCounters` with `ApplyVariableCounters` recalculate and store the variable counters (`VariableCounters(VarCount1, VarCount2, MaxLocalVarCount)`).
