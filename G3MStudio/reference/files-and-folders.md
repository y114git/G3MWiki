# Files and folders

This page lists every file and folder G3MStudio creates on disk. `<key>` is a 16 character name derived from the full path of a data file; it stays the same when the file is saved, so journals and backups survive saves. On Windows and macOS paths are compared without regard to case.

## Data folder

Location per system: see [Data folder](../getting-started/user-data.md). It is `%APPDATA%\G3MStudio` on Windows, `~/.config/G3MStudio` on Linux and `~/Library/Application Support/G3MStudio` on macOS. The variable `G3MSTUDIO_DATA` replaces it. If the folder cannot be created, the session uses `G3MStudio-<user>` in the system temporary folder.

| Path | Content | Cleanup |
| --- | --- | --- |
| `settings.json` | All settings, saved dock layouts, install id. | Kept. |
| `recent.json` | Recent files and pins. Up to 50 unpinned entries. | Kept. |
| `recent-scripts.json` | The last 10 scripts run (Tools > Run Recent Script). | Kept. **Clear Items** empties it. |
| `last-folder.txt` | The folder of the last file, save or folder dialog. The next dialog opens there. | Kept. |
| `backups/<key>/<file>.<n>` | Rotating copies of the data file made on save. Slot 1 is the newest. | **Settings** sets the count: default 3, range 1 to 100. |
| `journal/<key>/journal.jsonl` | Edit journal that lets unsaved edits be recovered after a crash ("Unsaved edits recovered: {count}"). | Per file. |
| `view/<key>.json`, `view/<key>.layers.json` | Open tabs, sizes and layer state per data file. | Kept. |
| `nicknames/<key>.json` | Nicknames of resources. | Kept. |
| `locks/<key>.json` | Resources marked **Read only** (edits refused by G3MStudio). | Kept. |
| `organize/<key>.json` | Explorer folders, collections and saved searches. | Kept. |
| `autotile/<key>.json` | Auto-tile rules. | Kept. |
| `runoptions/<key>.json` | Run options for that data file. | Kept. |
| `modbase/` | Cache of mod base files (`index.json`, `<hash>.gz`). See [Mod base](../settings/mod-base.md). | **Clear the Mod Base Cache** in the Mods menu, or the size limit in Settings. |
| `projects.json` | Registry of linked projects. | Kept. |
| `projects/<id>/` | Per project: `base.g3mproj` (the state at the last sync), `merge/`, `snapshots/<id>.json` and `objects/<hash>/` for [snapshots](../projects/snapshots.md). | With the project. |
| `plugins/<id>/` | One folder per plugin: `plugin.json`, `src/`, optional `lib/`, and `.origin` for a plugin installed from GitHub. | Kept. |
| `plugins.json` | Ids of the plugins you switched off. | Kept. |
| `plugin-cache/<id>-<hash>.dll` | Compiled plugins. Rebuilt when a source, a library or G3MStudio changes. | Kept. Safe to delete. |
| `plugin-settings/<id>.json` | Saved options of each plugin. | Kept. |
| `fonts/` | Fonts you added. | Kept. |
| `trash/<yyyyMMdd-HHmmss-fff>/` | Included files removed in the editor. | Batches older than 30 days are deleted at start. |
| `crashes/crash-<time>-<id>.txt` | Local crash log. Newest 10 kept. Written whether or not crash reports are on. Nothing is sent from it. | Pruned. |
| `crashes/hang-<time>-<id>.txt` | Written when the window does not answer for 10 seconds: what the interface thread was doing, whether it was busy or waiting, memory. A line is added when it answers again. Newest 10 kept. | Pruned. |
| `icons/g3mstudio.ico` | Icon for the data file types (Windows only). | Kept. |
| `*.<32 hex>.tmp` | Leftover of an interrupted settings write. | Deleted when older than 1 day. |

## Next to the data file

| File | When |
| --- | --- |
| `<data file name>.g3mrun.json` | Run options and profiles, when **Settings > Run > Where run options are stored** is set to **Next to the data file**. A file that is new or changed is ignored until you approve it. |
| `<name>.g3mnew` | During a save: the new file is written and checked here, then replaces the old one. A failed save deletes it. |
| `audiogroupN.dat` | Not created by G3MStudio; it opens audio group files that sit beside the data file. |

## Temporary folders

Work folders in the system temporary folder, deleted when the job ends: `g3mstudio-copy-*`, `g3mstudio-import-*`, `g3mstudio-pack-*`, `g3mstudio-mod-*`, `g3mstudio-base-*`, `g3mstudio-link-*`, `g3mstudio-audio-*`, `g3mstudio-utmt-*`, `g3mstudio-plugin-*` and `g3mstudio-update-*`. A crash can leave one behind. It is safe to delete.

## Next to the program

| Path | Content |
| --- | --- |
| `Scripts/` | `.csx` scripts for **Tools > Run Script**. |
| `projects/` | Default root for projects. |
| `.g3mstudio-update/` | Update work files (`new`, `old`, `result.txt`), temporary. |

## System registration

Created when you accept the file association or on each start for "Open with" and `g3mstudio://` links. `G3MSTUDIO_NO_REGISTRATION=1` skips it.

| System | Where |
| --- | --- |
| Windows | Current user registry classes (`G3MStudio.DataFile` and `OpenWithProgids` of `.win`, `.unx`, `.ios`, `.droid`, `.symbian`). No administrator needed. |
| Linux | `~/.local/share/applications/g3mstudio.desktop`, `~/.local/share/mime/packages/g3mstudio.xml`, icon under `~/.local/share/icons/hicolor/scalable/mimetypes` (XDG variables are honored). |
| macOS | Document types in the `Info.plist` of the app bundle. |

## Single instance

A named pipe (not a file) called `g3ms-<12 hex>`, reachable only by your user, passes files and links to the running window.

## See also

- [Data folder](../getting-started/user-data.md)
- [Saving](../projects/saving.md)
- [Glossary](glossary.md)
