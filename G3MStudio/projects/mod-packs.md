# Mod packs and Compare Data Files

A mod pack (`.g3mpack`) holds your changes as resource-level edits that can be re-applied to other versions of the same game. **Compare Data Files...** shows the differences between two data files and is where packs and mods are made from a chosen set of changes. G3MStudio packs are separate from the G3M mod ZIP described in [Mods](mods.md).

## Compare Data Files...

Command: **Mods > Compare Data Files...**

1. With a file open it is the first file (the left side). Without one you pick both: "Choose the first data file", then "Choose the data file to compare with".
2. G3MStudio compares them ("Comparing data files") and shows "Comparison done: {count} differences."
3. The window **Compare Data Files: {left} and {right}** opens. It shows "{added} added, {removed} removed, {changed} changed. {shown} shown." or "The two files have no differences."

Window controls:

| Control | Purpose |
|---|---|
| **Filter by name**, **All types** | Narrow the list. |
| Kind labels | **Added**, **Removed**, **Changed**. |
| Tabs | **Properties**, **Code**, **Picture**, **Room**. |
| **Side by side**, **Unified**, **Ignore white space** | Code view options. Their defaults come from the Settings diff options. |
| **Overlay**, **Swipe**, **Difference**, **Frame** | Picture comparison modes. "{count} pixels differ." or "There is no picture to compare." |
| **Apply this change to my file** | Applies the selected change to the open file. One undo step. "Applied '{name}' to your file. Undo reverts it." |
| **Copy resource across...** | Copies the resource into the open file (see [Copy to Other Data File](copy-to.md)). |
| **Export shown changes as pack...** | Writes the shown changes as a pack. |
| **Create G3M mod...** | Writes a G3M mod ZIP from the changes (see [Mods](mods.md)). |
| **Refresh** | Compares again. |
| **Save report...** | Writes a comparison report ("Report saved to {path}. Differences: {count}."). |

Rules:

- **Apply this change** and **Copy resource across...** work only when the left file is the file open in the main window: "This file is not being edited here, so the change cannot be applied."
- New or removed code travels with its object, script or room: "New or removed code travels with its object, script or room. Apply that item instead."
- Select an item first: "Select an item on the left first."

## Export Mod Pack...

Command `mods.exportPack`, reached from **File > Transfer Resources...** (group "Mods": "Save the shown changes as a mod pack.") or the command palette.

1. G3MStudio needs the original data file (the [mod base](mods.md#the-mod-base) or "Choose the original data file (before your changes)").
2. It compares the original with the open file.
3. The dialog **Export Mod Pack** asks for **Name**, **Version** and **Authors (separated by commas)** and says "The pack will hold {count} changes."
4. Pick the output in "Save the mod pack as" and press **Export**.

Result: "Mod pack written to {path}: {count} changes." Changes that could not go into the pack are named: "Left out: {count}. First: {first}". With no differences: "There are no differences to put in a pack."

A pack is a ZIP with `manifest.json` (name, version, authors and the SHA-256 of the data file the changes were made against), `changes.json`, a payload ZIP per added or replaced resource and the GML texts for code changes (the new text and the base text for a three-way merge). Each change is `add`, `replace`, `remove` or `code`. Members are read in memory and never extracted to disk.

## Apply Mod Pack...

Command: **Mods > Apply Mod Pack...** Needs an open file.

1. Choose the pack ("Choose a mod pack").
2. The window **Apply Mod Pack: {name} {version}** lists every change with a state and a summary "{apply} to apply, {done} already there, {conflicts} conflicts, {missing} cannot be applied. Nothing changes until you press Apply."

| State | Meaning |
|---|---|
| **Apply** | The change fits and will be applied. |
| **Already applied** | The file already has it. |
| **Conflict** | The resource changed in your file too. |
| **Cannot apply** | The resource it needs is missing. |

If the pack was made for another version of the data file: "The pack was made against another version of the data file. Changes are matched by resource name and content."

For a code conflict the row says "merges cleanly" or "{count} clashing parts". Choose **Keep mine**, **Keep theirs** or **Merge code**. **Save report...** writes the review to a file. Press **Apply pack**.

Result: "Applied mod pack '{name}' as one undo step." A pack applies as one edit, so Undo takes it back (unlike applying a patch to the open file).

## See also

- [Mods and the mod base](mods.md)
- [Patches](patches.md)
- [Copy to Other Data File](copy-to.md)
