# Data folder

G3MStudio keeps settings, recent files, edit journals, backups and logs in one data folder per user, separate from the program folder. This page says where it is, how to open it and what stays next to the program.

## Where it is

| System | Default location |
| --- | --- |
| Windows | `%APPDATA%\G3MStudio` |
| Linux | `~/.config/G3MStudio` |
| macOS | `~/Library/Application Support/G3MStudio` |

Set the environment variable `G3MSTUDIO_DATA` to use another folder, for example to keep everything on a USB stick together with the program. There is no other portable switch.

Open the folder with **Help > Open Data Folder**. If the system has no file manager for it, a note shows the path instead. **Help > Open Logs Folder** opens the `crashes` folder inside it. Besides crash logs it holds `hang-<time>-<id>.txt`, written when the window does not answer for 10 seconds. G3MStudio keeps the newest 10 of them. Attach the newest file when you report a freeze.

If the folder cannot be created (read-only profile, full drive), G3MStudio still starts and uses `G3MStudio-<user>` in the system temporary folder. Settings made in that session are lost at the next temporary cleanup.

## What the program folder holds

The program folder is the folder of `G3MStudio.exe` or `G3MStudio` (the `Contents/MacOS` folder inside the app on macOS). Beside it G3MStudio uses:

| Item | Use |
| --- | --- |
| `Scripts` | `.csx` files, subfolders included. When the folder holds any, **Tools > Run Script...** asks whether to pick a file or choose one from this folder. |
| `projects` | Default root for new projects. |
| `.g3mstudio-update` | Work files of an update, removed at the next start after every update, good or failed (see [Updating](updating.md)). |

## Recent lists

`recent.json` holds the recent data files (see [Start page](start-page.md)). `recent-scripts.json` holds the last 10 scripts you ran, newest first. **Tools > Run Recent Script** lists them, and **Clear Items** empties the list. The list belongs to you, not to a data file.

## Last folder

File, save and folder dialogs open in the folder of the last pick. The folder is kept in `last-folder.txt` in the data folder, also after a restart. If that folder is gone, the dialogs start in their default place.

## Back up or reset

- Copy the data folder to keep settings, recent files and per-file view state.
- Close G3MStudio and delete `settings.json` to reset settings.
- Rotating backups of saved data files are in `backups` (see [Files and folders](../reference/files-and-folders.md)).

## See also

- [Files and folders](../reference/files-and-folders.md)
- [Command line](command-line.md)
