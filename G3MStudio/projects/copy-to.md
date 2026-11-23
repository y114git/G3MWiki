# Copy to Other Data File

**Copy to Other Data File...** copies the selected resources, with the resources they use, into a second data file. The copy is one undoable edit of the target file.

## Before you start

- Select the resources to copy in the Explorer, or have one open in the tab in front. With nothing selected: "Select the resources to copy in the Explorer first."
- The target must be another file: "That is the file you copy from. Use Duplicate to copy inside one data file."
- The target is opened only for this copy. It does not become a tab.

Reach it from the Explorer context menu, from **File > Transfer Resources...** under "Out of this data file" ("Copy the selected resources into another data file."), or from the command palette. The command is `edit.copyToOtherFile`; it has no default shortcut.

## Steps

1. Choose **Copy to Other Data File...** as above.
2. In the picker "Pick the data file to copy into", choose the target.
3. The dialog **Copy to Other Data File** shows "Copy {count} from {source} to {target}" and a list of every item that would be copied, skipped or refused. While it plans it says "Checking what to copy...".
4. Adjust the options (below), then press **Copy**.
5. A progress bar "Copying to {target}" shows with a Cancel button.
6. Afterwards the dialog **Save the target file** asks "{target} was opened only for this copy. Save it now? If you say no, the copy is dropped."
7. If the copied resources sat in folders, collections or tags in the source, the dialog **Keep folders?** asks "Put the copied resources in the same folders, collections and tags in {target}?"

Result: a notification "{count} resources copied to {target}. One Undo takes them all out." Saving the target goes through the normal [save steps](saving.md), with backups. Answering no to the save question undoes the copy: "The copy to {target} was dropped."

## Options

| Option | Default | Effect |
|---|---|---|
| **Copy dependencies too** | On | Adds every resource the selection uses (a sprite used by an object, code called by an event). Dependencies show "needed by {names}". |
| **When the name exists in the target:** | **Rename (add _copy)** | The other choices are **Skip (use the target's item)** and **Replace the target's item**. |

For an item whose name already exists, the list has a drop-down with **Rename**, **Skip** and **Replace** (Replace only when the item in the target has the same type). Other rows show **Add**, **Skip** or **Refused**. A renamed row shows `old  >  new`.

The summary line says "To copy: {copy}. Skipped: {skip}. Refused: {refused}." If nothing can be copied: "Nothing to copy with these settings." **Copy** is off then.

## Refusals and warnings

| Case | Message or behaviour |
|---|---|
| The two files have different bytecode versions | Refused: "The bytecode versions differ (source {n}, target {n}); code and events would not run in the target." |
| One file is GameMaker Studio 1 and the other is not | Refused: "One file is GameMaker Studio 1 and the other is not (source ..., target ...); their data layouts differ." |
| The target is older than the source | Warning: "The target (...) is older than the source (...); properties the target version does not have are not copied." |
| The package is larger than 256 MB | A risky-action question. Going on may use a lot of memory, because the whole package is held in memory and in the journal. |

Problems found while copying (a resource that failed, a warning) are posted as notifications named after the resource.

Copy to Other Data File works through a temporary ZIP in the library's export layout. Page and audio positions and instance ids of the source are made safe for the target, and names are changed as chosen.

## Prompts you can silence

In **Settings > Prompts**: "Save the target file before copying to it" and "Keep folders, collections and tags when copying to another file".

## Troubleshooting

| Message | Fix |
|---|---|
| "The copy failed: {reason}" | The target is unchanged. Read the reason, fix it and try again. |
| "The copy was cancelled; nothing changed." | You cancelled. |
| Could not open the target | "Could not open '{name}': {reason}". Check that the file is a data file and not held by another program. |

## See also

- [Export packages](packages.md)
- [Saving and backups](saving.md)
- [Mods and mod packs](mods.md)
