# Outside changes, merge and conflicts

When another program changes the data file of a project (a patch tool, a game update), G3MStudio compares three states and lets you bring the changes in, replace the file or keep both apart. This page covers the dialogs and the rules behind them.

## When it appears

G3MStudio marks a project as changed outside when the data file on disk has another MD5 hash than the one recorded at the last sync. This is checked when the file opens, and during an autosync. The status bar then shows **Data file changed outside**, and a dialog opens when the file is opened. You can also open it with **Project > Resolve Outside Changes...** or by clicking the status bar item.

**Resolve Outside Changes...** also takes in edits made in the project folder, such as a resource folder you pasted in or a Git pull. See [Changes in the project folder](#changes-in-the-project-folder). When neither the data file nor the folder has changed since the last sync, a notification says "Nothing changed in the project folder or the data file since the last sync." and nothing else happens.

While the state is set, **Sync to Data File** is refused with "The data file changed outside. Choose how to bring in or replace its changes first."

## Changes in the project folder

When the data file is as the last sync left it, **Resolve Outside Changes...** asks no question. It looks at the project folder instead:

- A resource whose folder (or single file) no longer matches what was recorded at the last write is taken from the folder, even if you edited the same resource in the app.
- A resource folder that the project file does not list yet, copied into a type folder, is taken in too.
- Edits not synced yet are written first, then the data file is built from the folder.
- A resource listed in the project whose folder is gone stays in the data file. The warning reads "These resources have no folder in the project any more and stay in the data file: {names}." It names up to five.

A change of line endings or a byte order mark in a `.json`, `.gml` or `.asm` file does not count as a change.

When it is done, the data file opens again and a notification says "Merged the outside changes into {name}." If the data file could not be written, you get "The merged data file was not written: {reason}".

## The question

When the data file itself changed, the dialog **Data file changed outside** says "{name} was changed by another program since the project was last synced. Bring its changes into the project, replace the data file with the project, or keep both apart?"

| Button | Result |
|---|---|
| **Bring changes in** | Three-way merge of the outside changes into the project, then the merged data file is written. |
| **Replace data file with project** | Writes the data file from the project, whatever the file on disk holds. |
| **Keep both (archive the project)** | Leaves Project mode. The project is archived and the data file is not touched. |
| **Decide later** | Closes the dialog. The state stays. |

## How the merge works

Three states are compared resource by resource, by content hash:

- Base: the project at the last sync.
- Ours: the project now.
- Theirs: the data file that changed outside.

| Situation | Result |
|---|---|
| Same in ours and theirs | Nothing to do. |
| Changed only in theirs | The project takes theirs. |
| Removed only in the project | The data file loses the resource. |
| Changed in both, differently | Conflict. |
| A project made before merges existed (no base) | Every difference is a conflict. |

The strings, variables and functions lists follow the resources and the code, so they are never listed as conflicts.

## Resolve conflicts

When there are conflicts, the dialog **Resolve conflicts** says "These resources changed in the project and in the data file. Choose the version to keep for each." For each resource choose **Keep project version** or **Use data file version**, then press **Merge**. Resources you do not change keep the project version.

After the merge:

1. The merged file is built and checked, backups rotate as in a normal save (see [Saving and backups](saving.md)).
2. The merged state becomes the new base and a snapshot is taken with the label `merge`.
3. The data file opens again. A notification says "Merged the outside changes into {name}."

Failures are reported, not ignored: "Could not merge the outside changes: {reason}", and when the merged data file could not be written, "The merged data file was not written: {reason}".

## Replace data file with project

Every project resource is imported over the file on disk, and resources the project dropped since the last sync are removed. Resources that exist only in the data file stay in it, because G3MLib cannot tell them apart from the game's own resources.

## Memory use

Each phase of a merge releases its memory before the next one starts, so the peak is the largest phase and not the sum. A very large file still needs room for the data file and the project files at the same time.

## Troubleshooting

| Message | Cause and fix |
|---|---|
| "Could not merge the outside changes: ..." or "The merged data file was not written: ..." | A resource could not be read or written. The project and data file are unchanged. Try again, or choose **Replace data file with project**. |
| The state returns after Keep both | Keep both archives the project. Enable a project again to start a new one. |

## See also

- [Project menu](project-menu.md)
- [Snapshots](snapshots.md)
- [Saving and backups](saving.md)
- [Disk-changed window](saving.md#the-file-changed-on-disk)
