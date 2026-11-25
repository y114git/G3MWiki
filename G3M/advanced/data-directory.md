# Data directory

G3M stores application data separately from game installations.

| Platform | Default location |
| --- | --- |
| Windows | `%LOCALAPPDATA%\G3M`, with `%APPDATA%\G3M` as fallback |
| macOS | `~/Library/Application Support/G3M` |
| Linux | `~/.local/share/G3M` |

Throughout this wiki, `<data-root>` means this folder or the custom location selected in **Settings > General > G3M data location**.

## Change the location

Choose a folder and decide whether to copy existing data or use the selected folder without copying. G3M uses the chosen folder directly; it does not append another `G3M` folder.

The current and chosen folders cannot contain one another. Copying stops with an error that lists any destination entry with the same name, and nothing is overwritten. Copying does not delete the source. G3M records a custom location in `data-root.json` inside the default folder (`version` 1 and the chosen `path`); the default folder always stays, and resetting the setting deletes that file. Restart after a successful change. Resetting the setting returns to the platform's default through the same selection flow.

If the selected drive is unavailable or not writable at startup, the dialog **G3M data location unavailable** offers **Try again**, **Choose another folder**, and **Use default location**; Cancel stops G3M from starting. A shortcut launch reports the unavailable location rather than silently using a different profile library.

## Folders and files

| Path under `<data-root>` | Contents |
| --- | --- |
| `settings/settings.json` | Shared application settings |
| `settings/custom_games.json` | Custom games, visibility, and ordering |
| `settings/blocklist.json` | Hidden-result rules |
| `profiles/<name>/<name>.json` | Profile choices and launch selection |
| `profiles/<name>/mods_data.json` | Profile Library metadata |
| `profiles/<name>/<mod folder>/` | Installed mod files, with `mod_config.json` |
| `downloads/` | Downloaded files and `downloads_history.json` |
| `game_versions/` | Saved game archives and `game_versions_data.json` |
| `plugins/` | Installed plugin folders and `plugins_data.json` |
| `themes/` | Saved theme packages |
| `lang/` | External language files and fonts |
| `logs/` | `g3m.log`, `shortcut.log`, and the archive folder `g3m/`; see [Logs](logging.md) |
| `cache/G3MTool/` | Reusable analysis cache |
| `settings/operation-session/` | Journal and backups of the modded session in progress; shortcut launches use `settings/operation-shortcut-session/` |
| `g3m.instance.lock` | Lock that keeps a single G3M instance running |
| `last_dialog_dir.txt` | Folder of the last file, save, or folder pick. File and folder dialogs open there. |

Each profile's mod folders sit directly under its profile folder. A profile folder name keeps letters, digits, `_`, `-`, and spaces; other characters become `_`, and the name is cut at 80 characters. On Windows, a reserved name such as `CON` gets a trailing `_`. Custom appearance assets sit in `settings/` as `custom_background.*`, `custom_logo.*`, `custom_font.*`, `custom_startup_sound.*`, and `custom_background_music.*`.

A first start with an existing `DELTAHUB` data folder (same location rules, folder name `DELTAHUB`) asks whether to import it (**Import Data**) or start empty (**Start Fresh**).

Do not remove launch backups or session state while a game session or recovery is pending. See [backup and restore](backup-and-restore.md) before cleaning personal data.
