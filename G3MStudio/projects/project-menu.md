# The Project menu

The **Project** menu opens, creates, syncs and ends [projects](overview.md). This page lists every entry, what it does and when it is available.

## Entries

| Entry | Available | What it does |
|---|---|---|
| **Open Project...** | Always | Opens a project from its `.g3mproj` file, its folder, or a UTMT project (see [Import a UTMT project](utmt-import.md)). |
| **Close Project** | Project on | Leaves Project mode for the open file and keeps the project folder (archived). |
| **New Project...** | Project off | Enables a project for the open file and always asks for the projects folder. |
| **Enable Project** | Project off | Enables a project for the open file. Asks for the projects folder only the first time, or when the remembered folder is gone. |
| **Sync to Data File** | Project on | Writes the project, then writes the data file from memory and takes a [snapshot](snapshots.md). |
| **Resolve Outside Changes...** | Project on | Decides what to do with a data file that another program changed, and takes in edits made in the project folder (see [Outside changes](outside-changes.md)). |
| **Project Settings...** | Project on | Opens the [autosave and autosync settings](project-settings.md). |
| **Change Data File Location...** | Project on | Moves, copies or recreates the data file somewhere else. |
| **Disable Project...** | Project on | Ends Project mode and asks whether to keep or delete the project. |
| **Recent Projects** | Always | Lists up to 10 known projects whose folder still exists. Shows "No recent projects." when there are none. |

None of these entries has a default shortcut. You can bind keys in **Settings > Keyboard shortcuts**, and every entry is in the command palette.

An entry that is off shows a tip when you hover it: "Open a data file first.", "This file already has a project." or "This file has no project yet. Use New Project first."

## The status bar item

The project item in the status bar shows the state and acts as a button:

| Text | Meaning | Click runs |
|---|---|---|
| **Enable project** | The open file has no project. | Enable Project |
| **Synced** | The data file matches the project. | Sync to Data File |
| **Changes not synced to data file: N** | N edits are not in the data file yet. | Sync to Data File |
| **Syncing...** | A sync is running. | Sync to Data File |
| **Exporting project...** | The first export or a project write is running. | Sync to Data File |
| **Data file changed outside** | Another program changed the data file. | Resolve Outside Changes... |

## Enable a project

1. Open a data file.
2. Choose **Project > Enable Project** (or **New Project...**).
3. In **Projects folder**, "Where should projects be saved?", type or browse for a folder. Tick **Remember this choice** to skip the question next time. The default folder is `projects` next to the program.
4. Wait for the export. A progress bar shows "Exporting {name}... {done} of {total} resources" with a cancel button.

Result: a notification "Project created for {name}." appears and the status bar shows **Synced**. If G3MLib reports problems, a warning says "The project was created, but problems were reported: {count}. First: {first}". A resource that could not be exported is left out.

While the export runs, staging folders are moved with retries and a copy fallback when a virus scanner holds them (G3MLib 1.1.7).

Cancelling the export shows "Project export cancelled. Nothing was kept." and removes the folder. Edits you make during the export wait and run afterwards.

Errors: "Could not create the project: {reason}" and "This data file already has a project."

## Sync the data file

1. Make your edits. With autosave on they reach the project folder by themselves.
2. Press **Ctrl+S** (autosync **On save**), or choose **Project > Sync to Data File**, or click the status bar item **Changes not synced to data file: N**.
3. Wait for **Synced**. A snapshot is taken and the data file is replaced after the usual check, with backups kept.

If the status bar says **Data file changed outside**, sync is refused. Use **Project > Resolve Outside Changes...** first (see [Outside changes](outside-changes.md)).

## Resolve outside changes

1. Click **Data file changed outside** in the status bar, or choose **Project > Resolve Outside Changes...**.
2. Pick **Bring changes in** to merge, **Replace data file with project** to throw the outside change away, or **Keep both (archive the project)** to leave Project mode.
3. After **Bring changes in**, answer the **Resolve conflicts** list if one opens, then press **Merge**.

## Open a project

**Open Project...** asks "Choose a project file (.g3mproj or a UTMT project.json)". Then:

- A project the program knows opens its data file at the last synced path.
- An unknown project asks "Choose the data file of this project". The file must be the one the project was last synced with, otherwise: "'{name}' is not the data file this project was last synced with."
- If the chosen path is neither a project folder nor a project file: "'{path}' is not a project folder or project file."
- Other failures: "Could not open the project: {reason}".

If the data file is already open without a project, the project attaches to it. Otherwise the data file opens and finds its project.

## Close, disable and delete

- **Close Project** archives the project. Open Project, or the offer when the file opens, brings it back. The data file and the folder are not touched.
- **Disable Project...** asks: "Keep the project (archived, it can be enabled again) or delete it to free disk space? The data file is not touched." with **Keep project** and **Delete project**. Delete removes the project folder, its snapshots and its record. If a program still holds the folder, the delete fails with the project still listed, and you can retry.

## Change the data file location

**Change Data File Location...** (title **Data file location**) asks for a **New location** and one of three modes:

| Mode | Effect |
|---|---|
| **Move** | Writes pending changes, copies the file, checks the size, then deletes the original. |
| **Copy** | Writes pending changes and copies the file. The original stays. |
| **Recreate from project** | Builds a new data file from the project and leaves the original alone. |

The target must not exist ("'{path}' already exists."). Afterwards the open file, the project record, the journal, run options and recent list point at the new path. A notification says "Data file is now at {path}". Failure: "Could not change the location: {reason}".

In Project mode, **Save As...** does not move the file; a notification says "Move the data file of a project through Project > Data File Location."

## See also

- [Projects and Project mode](overview.md)
- [Project settings](project-settings.md)
- [Saving and backups](saving.md)
- [Outside changes, merge and conflicts](outside-changes.md)
