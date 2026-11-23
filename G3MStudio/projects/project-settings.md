# Project settings

Project settings decide when G3MStudio writes your edits into the project folder (autosave) and into the data file (autosync). Open them with **Project > Project Settings...** or set the defaults for new projects in **Settings > Projects**.

## Options

The dialog **Project settings** has two rows. Each row has a trigger and a number of seconds.

| Row | Default trigger | Default seconds |
|---|---|---|
| **Autosave to the project** | **After a delay** | 2 |
| **Autosync to the data file** | **On save** | 30 |

Seconds accept 1 to 3600. The seconds value only applies to the trigger **After a delay**.

| Trigger | Meaning |
|---|---|
| **Off** | Never writes by itself. |
| **After a delay** | Writes when no edit has happened for the set seconds. Autosync also waits at least that long since the last sync started. |
| **When the window loses focus** | Writes when you switch to another window. |
| **On save** | Writes when you save (**Ctrl+S**). |

Autosave writes only the resources you changed. Autosync writes the whole data file and takes a [snapshot](snapshots.md). The two settings are independent.

Autosync waits while the data file is marked "Data file changed outside". It never opens a dialog.

## Saving in Project mode

**Save** (**Ctrl+S**) first writes the project. It also syncs the data file when autosync is **On save**. The first time you save, a dialog asks:

- **Save and sync with data file**: also rebuilds the data file and keeps a backup of the previous data file.
- **Save progress**: writes only the project folder files.

Tick **Remember my choice and don't ask again** to keep the answer. A remembered answer overrides the autosync setting. Reset it in **Settings > Prompts** under "Saving in a project (yes: also sync the data file, no: save progress only; a remembered answer overrides the autosync setting)".

## When a write fails

If the project folder cannot be written (a locked folder, a full disk), G3MStudio shows "Could not write the project: {reason}" and tries again after a growing pause, from 10 seconds up to 5 minutes. Your edits stay in memory and in the journal. A write that died half way is redone in full at the next open.

## See also

- [Project menu](project-menu.md)
- [Saving and backups](saving.md)
- [Projects and Project mode](overview.md)
