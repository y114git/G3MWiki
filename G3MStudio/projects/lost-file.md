# Lost data file

When G3MStudio cannot open a data file it remembers (the last session, a recent entry), it opens the dialog **Data file not found** with ways to find the file again. This page explains the dialog, the search and the project rebuild.

## When it appears

G3MStudio checks the remembered file against the size and MD5 hash it saved. The dialog opens for one of these reasons:

| Case | Message |
|---|---|
| Missing | "G3MStudio cannot find {name} at {path}. It may have been moved, renamed or deleted. Choose the file again, and G3MStudio keeps its layout and sizes when the content is the same." |
| Unreadable | "{name} at {path} cannot be read: {reason}. Another program may be using it, or you may not have permission." |
| Changed | "{name} is not the same as when G3MStudio last saw it. It was changed outside G3MStudio, is another version of the game, or is another file. The layout and sizes G3MStudio kept belong to the earlier content." |

## The dialog

The list "Files that may be the one:" shows candidates, each marked **same content** (same size and hash) or **different content** (same name only). With none: "No file with the same content was found nearby."

| Control | Effect |
|---|---|
| **Keep the saved layout and sizes for this file** | For a file with different content, keeps the saved layout. |
| **Open selected** | Opens the chosen candidate. |
| **Choose file...** | Picks any file ("Choose the data file"). |
| **Try again** | Checks again (a network drive or a locked file). |
| **Remove from Recent** | Removes the entry from the recent list. |
| **Go to Home** | Shows the start page. |

When exactly one file has the same content and the setting allows it, G3MStudio opens it without asking: "Found {name} at {path}. It has the same content, so its layout and sizes are kept."

## The search

The search looks in the folder of the old path, the folder above it, up to 24 folders next to the old one and up to 24 folders of recent files, so a recent list full of network paths cannot stall the start. It matches by hash, so a renamed or moved file is found.

## Settings

In **Settings > Files**:

| Setting | Meaning |
|---|---|
| Look for a lost data file by its hash in nearby folders and the folders of recent files | Turns the search on or off. |
| Use the found file without asking when exactly one has the same hash | Skips the dialog for a single match. |
| When a data file differs from the one remembered | **Ask me**, **Open it and keep the saved layout**, **Open it with a fresh layout** or **Do not open it**. The last one says "{name} is not the file G3MStudio remembered, and your settings say not to open it. Settings > Files changes this." |
| Remove the old entry from Recent when a replacement file is chosen | Cleans the recent list. |
| When G3MStudio starts | **Reopen the last file, tabs and layout**, **Show Home** or **Ask me** ("Open {name}, where you stopped last time?"). |

## Rebuild a data file from its project

If a data file does not open and it has a project, the dialog **Data file cannot be opened** asks "{name} is missing or damaged, but it has a project. Build it again from the project?" Press **Rebuild from project**.

1. G3MStudio uses the newest [backup](saving.md#backups) as the file to build on. Without one it asks "Choose a copy of the game's data file to build on".
2. It builds a new file from the project and checks that it reads.
3. A file in the way is kept as `<name>.corrupt`.
4. The project record points at the new file and the file opens.

Failure: "Could not rebuild the data file: {reason}". Only a file that cannot be read as data triggers the offer, not a locked one.

## See also

- [Saving and backups](saving.md)
- [Projects and Project mode](overview.md)
- [Guide: recover after a crash](guide-recover-crash.md)
