# Guide: create a project from a data file

This guide turns an open data file into a project, makes an edit and syncs it. You end with a project folder, a snapshot and a data file that holds your edit.

## Before you start

- Free disk space for a full copy of the game's resources. Large games need several hundred MB.
- A data file you can write to.

## Steps

1. Open the data file with **File > Open** (**Ctrl+O**) or drop it on the window.
2. Choose **Project > Enable Project**. The first time, the dialog **Projects folder** asks "Where should projects be saved?" Choose a folder and tick **Remember this choice**. Press OK.
3. Wait for the progress bar "Exporting {name}... {done} of {total} resources". You can keep working; edits wait and apply afterwards.
4. Check the result: the notification "Project created for {name}." and the status bar item **Synced**.
5. Make an edit, for example rename a sprite to `spr_player_idle` with **F2**. The status bar shows "Changes not synced to data file: 1". Autosave writes the change into the project folder after 2 seconds of quiet.
6. Press **Ctrl+S**. In the first Save question choose **Save and sync with data file**. Tick **Remember my choice and don't ask again** if you want this every time.
7. Check the result: the status bar shows **Synced** again, the notification says "Saved {name}", and a [snapshot](snapshots.md) is taken.

## What you have now

- A folder `<name>-<id>` in your projects folder with a `.g3mproj` file and one entry per resource.
- A backup of the previous data file in the G3MStudio data folder (see [Saving and backups](saving.md#backups)).
- Optionally a Git history: open **View > Panels > Git** and press **Initialize Repository** (see [Git panel](git.md)).

## If something goes wrong

| Message | Fix |
|---|---|
| "Could not create the project: {reason}" | Check free space and that the projects folder is writable. |
| "The project was created, but problems were reported: {count}. First: {first}" | A resource could not be exported and was left out. The data file is unaffected. |
| "Data file changed outside" | Another program changed the file. See [Outside changes](outside-changes.md). |

## See also

- [Projects and Project mode](overview.md)
- [Project menu](project-menu.md)
- [Project settings](project-settings.md)
