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

The current and chosen folders cannot contain one another. Copying does not overwrite conflicting destination entries and does not delete the source. Restart after a successful change. Resetting the setting returns to the platform's default through the same selection flow.

If the selected drive is unavailable at startup, the main application offers retrying, choosing another folder, or using the default. A shortcut launch reports the unavailable location rather than silently using a different profile library.

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
| `logs/` | Application, patching, conflict, and shortcut logs |
| `cache/G3MTool/` | Reusable analysis cache |
| `patching_backups/` | Temporary launch backups and retained recovery data |
| `settings/session.lock` | Information needed to recover an interrupted modded session |

Each profile's mod folders sit directly under its profile folder. Custom appearance assets use `custom_background.*`, `custom_logo.*`, `custom_font.*`, `custom_startup_sound.*`, and `custom_background_music.*` at the data root.

Do not remove launch backups or session state while a game session or recovery is pending. See [backup and restore](backup-and-restore.md) before cleaning personal data.
