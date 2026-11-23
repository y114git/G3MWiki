# Snapshots

A snapshot records the state of a project at one sync. This page explains when snapshots are taken, where they live and what works today.

## When a snapshot is taken

G3MStudio takes one snapshot after every successful sync of the data file (**Sync to Data File**, **Save** in sync mode, an autosync) and after a merge of outside changes. The label is `sync` or `merge`.

## What is stored

- The snapshot list and the files live in the G3MStudio data folder under `projects/<id>/` (`snapshots/*.json` and `objects/`), not in the project folder. Deleting the project folder does not remove them; **Disable Project...** with **Delete project** does.
- Each resource is stored once by its content hash. A resource that did not change since an earlier snapshot is not stored again.
- At most 500 snapshots are kept per project. Older ones are dropped, with the stored objects only they used. The limit is fixed.
- A snapshot list entry that cannot be read (cut short by a crash) is skipped.

## What you can do with them

The program core can compare a snapshot with the project and put one resource back into the project folder (the copy is made beside the target first, so a failure leaves the current resource as it was). The current version of the app has no menu, panel or command for browsing or restoring snapshots. Use [Git](git.md) or the data file [backups](saving.md#backups) to go back to an earlier state.

## See also

- [Projects and Project mode](overview.md)
- [Edit history and undo](history.md)
- [Git panel](git.md)
