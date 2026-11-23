# Export packages

An export package is a ZIP that holds selected resources in the G3MLib export layout. You can send it to someone and import it into another data file. This page covers **Export as Package...** and **Import Resources from a Package...**.

## Export as Package...

Command: **Export as Package...** (`organize.exportPackage`). Reach it from the Explorer context menu, from **File > Transfer Resources...** (group "Out of this data file": "Save the selected resources as one package.") or from the command palette. It has no default shortcut.

What is packed:

- The selected Explorer entries. If nothing is selected but a virtual folder, collection or tag is, all its entries.
- The code that the entries own.
- Only types that can be exported as files. If none of the entries can be: "Nothing to pack: these entries cannot be exported as files."

Steps:

1. Select resources in the Explorer, or a folder, collection or tag.
2. Choose **Export as Package...** as above.
3. In the save dialog "Save {name} as a package", enter a file name. If you leave out the extension, `.zip` is added.
4. If `{file}` exists after `.zip` was added: the dialog "Replace the existing file?" asks "{file} exists already. Replace it with the package?"

Result: the ZIP is written under a temporary name and moved into place, so a failure leaves no half file. A notification reports the result.

## Import Resources from a Package...

Command: **Import Resources from a Package...** (`file.importPackage`), in **File > Transfer Resources...** (group "Into this data file": "Add the resources of an exported package.").

1. Save or revert unsaved edits first, otherwise: "Save or revert your unsaved changes before importing, so the import does not mix with them."
2. Choose **Import Resources from a Package...** and pick the ZIP ("Package to import (ZIP)").
3. Confirm: "Import {folders} from {folder}? The open data file changes in memory and this cannot be undone. Save to keep it, or Revert to Saved to drop it."
4. A progress bar with Cancel runs. The ZIP is unpacked and checked before any change, so a refused package does not clear the undo history.

Result: "Imported from {folder}. This cannot be undone: Save to keep it, or Revert to Saved." with a summary "{written} written, {skipped} skipped, {warnings} warnings, {failed} failed."

If you cancel, the open data file may hold part of the import: "Import cancelled. The open data file holds part of it: Revert to Saved to start over."

The import clears the undo history, so it cannot be undone. Silence the confirmation in **Settings > Prompts** ("Import resources into the open file").

## Related commands

These are reached the same way, through **File > Transfer Resources...**.

| Command | Purpose |
|---|---|
| **Import Resources from Another Data File...** | Copies resources in from another data file. |
| **Export Selected Resources to a Folder...** and **Export All Resources to a Folder...** | Writes the library layout to a folder instead of a ZIP. Prompt "Folder to export to". |
| **Import Pictures, Sounds and Scripts from Files...** | Adds sprites, sounds and scripts from files. |
| **Import Code from a Folder...** and **Import Code from Files...** | Replaces code of existing entries from files. |
| **Copy to Other Data File...** | Copies into a second data file directly (see [Copy to Other Data File](copy-to.md)). |

Exports can be cancelled ("Export cancelled. Nothing was kept."). After an export: "Exported to {folder}."

## See also

- [Copy to Other Data File](copy-to.md)
- [Mods and mod packs](mods.md)
- [Saving and backups](saving.md)
