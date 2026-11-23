# Saving and backups

Saving writes your edits into the data file in a way that leaves either the old or the new valid file on disk, whatever happens. This page covers the Save commands, the steps of a save, backups, and what the messages mean.

## Save commands

| Command | Shortcut | What it does |
|---|---|---|
| **File > Save** | **Ctrl+S** | Writes the data file. In [Project mode](overview.md) it writes the project and, depending on the settings, the data file. |
| **File > Save As...** | **Ctrl+Shift+S** | Writes the data file to a new path, then opens the new file. The old file keeps what it had. |
| **File > Save a Copy...** | none | Writes the data file to a new path and keeps the current file open. |
| **File > Revert to Saved...** | none | Drops unsaved edits and loads the file again. |

Save As and Save a Copy offer the file types `*.win`, `*.unx`, `*.ios`, `*.droid` and `audiogroup*.dat`. In Project mode Save As does not move the file; use **Project > Change Data File Location...** (see [Project menu](project-menu.md)).

Save first applies the text typed in code editors. A code entry that does not compile stops the save.

### The first Save question

Outside Project mode the first Save asks: "Save writes all your edits into the data file. G3MStudio first writes and checks a new file, keeps backups of the previous one, then replaces the data file." with **Save** and **Cancel**. Tick **Don't ask again** to save at once from then on. In Project mode the question offers **Save and sync with data file** and **Save progress** (see [Project settings](project-settings.md)). Reset remembered answers in **Settings > Prompts**.

While a save runs a progress overlay with a Cancel button shows. Cancelling leaves the file on disk untouched; the edits stay in memory and in the journal. Edits you make during the save wait and apply afterwards ("The file is being saved. The change applies when that ends.").

## Steps of a save

1. Check free space. The drive needs room for a file the size of the current data file plus 16 MB, otherwise the save stops with "Not enough free space to save: {n} MB needed, {n} MB free." Paths without a drive letter (shares) skip this check.
2. Write a new file next to the target with the extra extension `.g3mnew`, and flush it to disk. A file or link left under that name is removed first.
3. Read the new file back and compare its chunks and resource counts with the model. If they differ, the new file is deleted and the original stays: "Could not save '{name}'. The written file did not match the model ({detail}). The original is untouched."
4. Copy the current data file into the backup folder as a temporary `.part` file.
5. Replace the data file with the new file.
6. Rotate the backups (only after the replace worked, so a failed save never pushes a good backup out).

The target is never opened for writing, so a crash at any step leaves the old or the new valid file. Afterwards the status bar item shows the saved state, the journal starts over from the saved file, and a notification says "Saved {name}".

If the saved file cannot be read back for its checksum, the save stands and a warning follows: "Saved {name}, but the file could not be checked afterwards: {reason}".

## Backups

Each save keeps the previous data file as a numbered backup in the G3MStudio data folder: `backups/<key>/<file name>.<slot>`. Slot 1 is the newest. The key is stable for a data file path.

- **Settings > Files > Backups of the data file (1 to 100)**: the number of backups. Default 3.
- Each backup is a full copy, so 100 backups of a 500 MB file take 50 GB.
- A backup that cannot be made (full or read-only backup drive) costs the backup, never the save.
- The newest backup is the base for **Rebuild from project** when a data file is lost (see [Recovery](#recovery)).

## The file changed on disk

G3MStudio remembers the size and write time of the file when it opened or last wrote it. If another program changed it, Save opens a dialog: "{name} was changed by another program after G3MStudio opened or last saved it. Overwrite it with your version, or save your version as a copy?"

| Button | Result |
|---|---|
| **Overwrite** | Replaces the file with your version. |
| **Save As...** | Opens the file picker and saves your version as a copy. The original save is not done. |
| **Cancel** | Nothing is saved. |

A save nobody asked for (autosync) never opens this dialog. It is refused and, in Project mode, the project shows **Data file changed outside** (see [Outside changes](outside-changes.md)). A refused save says "Did not save '{name}': it was changed outside G3MStudio. Your edits are kept."

## Failures

| Message | Cause and fix |
|---|---|
| "Could not save '{name}': the file is in use by {holder}. Your edits are kept." | A program holds the file. G3MStudio retries 6 times, 500 ms apart, then gives up. Close the program (a scanner, a sync client, the game) and save again. |
| "Could not save '{name}': the file is in use by another program. Your edits are kept." | The same, with an unknown holder. |
| "Could not save '{name}': the file is read only. Clear the read-only flag and save again. Your edits are kept." | Clear the read-only attribute. A read-only flag is not retried. |
| "The drive is full, so the last change could not be saved. Free some space and try again." | A write into the G3MStudio data folder hit a full drive. Free space. |
| "The G3MStudio data folder could not be written, so crash recovery and remembered settings may be out of date. Your edits stay in memory: {reason}" | The data folder is read only or full. Edits stay safe in memory; save the data file to keep them. |
| "Not enough memory for {name}..." | Close other programs and save again. Your edits stay in memory and in the journal. |
| "Could not save '{name}': {reason}" | Any other write error. |

## Closing with unsaved changes

When you close the window, switch files or restart, the close prompt **Unsaved changes** asks:

- Direct mode: "{name} has unsaved changes: {count}. Save them, or keep them in the journal for later?" with **Save**, **Do it later**, **Cancel**.
- Project mode: "{name} has changes that are not in the data file: {count}. Sync them, or keep them in the journal for later?" with **Sync changes**, **Do it later**, **Cancel**.

**Do it later** leaves the edits in the journal; they come back the next time you open the file ("Unsaved edits recovered: {count}"). Tick **Don't ask again (keep changes in the journal when closing)** with **Do it later** to stop the prompt. **Settings > Files > Ask before closing with unsaved changes** and the item "Ask when closing with unsaved changes" in **Settings > Prompts** control it.

In Project mode, changes are first written to the project folder without asking. If that fails, the journal still holds them.

Edits that cannot be kept in the journal (after a script, a patch or an import ran, or when the journal could not be written) get a different prompt: "{name} has unsaved changes: {count}. They cannot be kept in the journal (a script, patch or import ran, or the journal could not be written). Save them, or discard them?" with **Save**, **Discard**, **Cancel**. It is asked even when the setting is off.

If **Save** fails, the window stays open.

## Recovery

See [History, journal and crash recovery](history.md) for the journal and [Guide: recover after a crash](guide-recover-crash.md) for the steps. The data file and the backups are never opened for writing during a save.

## See also

- [History, journal and crash recovery](history.md)
- [Project settings](project-settings.md)
- [Outside changes](outside-changes.md)
- [Lost data file](lost-file.md)
