# Guide: recover after a crash

This guide gets your work back after G3MStudio or the PC stopped without a save. You end with your unsaved edits restored, or the data file rebuilt from a backup or its project.

## Case 1: edits were not saved

G3MStudio writes every edit to a journal as you work, so a crash loses nothing that was in the journal.

1. Start G3MStudio again.
2. Open the same data file, at the same path. If startup asks "Open {name}, where you stopped last time?", press **Open it**.
3. Check the notification "Unsaved edits recovered: {count}". The edits are back, unsaved and undoable.
4. Press **Ctrl+S** to save.

If the file on disk changed since (you saved it by another program, patched it, or replaced it), the old journal does not fit and is set aside as `journal.jsonl.stale`. The edits are not replayed then.

If a script, a patch or an import ran before the crash, those changes were never in the journal.

## Case 2: the data file does not open

1. If the file is missing or moved, use the dialog **Data file not found** (see [Lost data file](lost-file.md)).
2. If it is damaged and it has a project, press **Rebuild from project** in the dialog **Data file cannot be opened**. G3MStudio builds it on the newest backup (or a copy you choose) and keeps the damaged file as `<name>.corrupt`.
3. If it has no project, restore a backup by hand: the backups are in the G3MStudio data folder under `backups/<key>/<file name>.1`, `.2`, `.3` (slot 1 is the newest). Copy one next to the game under the data file name. Open **Help > Open Data Folder** to find the folder.

A save never opens the data file for writing, so a crash during a save leaves the old or the new valid file. A leftover `<name>.g3mnew` next to it is an unfinished save and can be deleted.

## Case 3: the project folder is behind

When a crash left a project half written, the next open writes the whole project again. Nothing to do.

## Case 4: the file is open in another window

"'{name}' is in use by another G3MStudio window. Finish there first." Close the other window, or end a stuck G3MStudio process, then open the file again.

## Prevent the next one

- Keep **Backups of the data file** at 3 or more (**Settings > Files**).
- Use [Project mode](overview.md) with Git for a history you can restore.

## See also

- [History, journal and crash recovery](history.md)
- [Saving and backups](saving.md)
- [Lost data file](lost-file.md)
