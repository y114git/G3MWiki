# History, journal and crash recovery

G3MStudio keeps every edit in an undo history and writes each one to a journal on disk. This page covers Undo and Redo, the History panel, the journal and what happens after a crash.

## Undo and Redo

| Command | Shortcut |
|---|---|
| **Edit > Undo** | **Ctrl+Z** |
| **Edit > Redo** | **Ctrl+Y** |

Each workspace (open data file) has one undo stack. A new edit after an undo removes the redo steps. Undoing back to the point of the last save marks the file as unchanged again.

Edits cannot run while a save is reading the model; they wait and apply afterwards.

### Limits

- The oldest steps are dropped when the pictures and audio kept for undo pass the **Undo memory limit** (**Settings**, default 128 MB, range 16 MB to 1,048,576 MB). A file whose saved state was dropped stays marked as changed.
- Some actions cannot be undone: imports from folders, scripts, applying a patch to the open file. They clear the undo history and count as unsaved changes. G3MStudio warns first: "You have unsaved changes. This action cannot be undone, so the undo history is cleared..." Save before you run them. Apply Patch and Import show their own warnings (see [Patches](patches.md)).

## The History panel

**View > Panels > History** shows the undo stack as a list, newest first.

- Each row shows the time (`HH:mm:ss`) and a description such as "Edit code", "New sprite: spr_player", "Rename obj_a to obj_b" or "Delete 3 resources".
- Rows that are undone are dimmed.
- The last row, **Opened state**, is the state when the file was opened.
- Click a row to undo or redo to that point. Clicking does nothing while a save runs.

Descriptions come from the edit kind. Resource nicknames show in place of names.

## The journal

The journal is a file in the G3MStudio data folder, `journal/<key>/journal.jsonl`. It starts with a header line that names the data file state (size and MD5) the edits start from, then one line per edit, undo or redo.

- Each line is handed to the operating system when written, so a crash of the program loses nothing. The slow disk sync is done at most once every 500 ms of activity, and always on save and close.
- Large values (over 64 KB, such as a picture or audio) go to a side folder `journal.jsonl.blobs/` named by hash, so a large edit is stored once.
- It never touches your data file.
- A journal that starts from another file state (the file was saved, patched or replaced since) is set aside as `journal.jsonl.stale`, then `.stale2` up to `.stale9`.
- When a bulk change (import, script, patch) ran, or a journal write failed, the unsaved edits are not safe in the journal until the next save. The close prompt then offers **Save** or **Discard** only.

A second G3MStudio window cannot open a data file whose journal the first one holds: "'{name}' is in use by another G3MStudio window. Finish there first."

## After a crash

Open the same data file again. If the journal starts from the file as it is on disk, its edits are replayed and a notification says "Unsaved edits recovered: {count}". The recovered edits are undoable and unsaved. In Project mode the project folder is also rewritten in full when a crash left it behind or half written.

Steps are in [Guide: recover after a crash](guide-recover-crash.md).

## See also

- [Saving and backups](saving.md)
- [Snapshots](snapshots.md)
- [Guide: recover after a crash](guide-recover-crash.md)
