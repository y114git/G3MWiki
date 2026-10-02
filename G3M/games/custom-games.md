# Custom games

A custom game connects a GameMaker installation to G3M's Library, editor, launch controls, and optional online mod browsing.

## Add an entry

Open **Game Manager** and choose its add action. Fill in the game information:

| Field | Value |
| --- | --- |
| Display name | The name shown in the game selector |
| Executable or app name | Exact executable filename, such as `ExampleGame.exe`, or macOS bundle name, such as `ExampleGame.app`. |
| Target DATA file name | Exact GameMaker resource filename, such as `data.win`, `game.unx`, or `game.ios`. |
| Steam App ID | Optional numeric application ID for Steam launching |
| GameBanana game ID | Optional numeric game ID or GameBanana game-page URL for Mods Browser results. |

Executable and data fields identify file names; configure the installation folder in **Settings > Game** after saving. The game's ID is used by mod configurations, so use that entry's ID when choosing a game in the editor.

Names and online IDs must be unique among game entries. Steam IDs contain digits only. Leave an online ID empty when the game has no matching entry on that service.

For example, an installation containing `ExampleGame.exe` and `data.win` uses those two names. If the game stores its resources in another location or requires a specialized launcher, check detection and launch behavior before distributing mods for it.

## Capabilities and limits

Custom entries have a game path, user-data path, executable override, mod selection, and priority steps. A GameBanana ID enables browsing for that game; a Steam ID enables Steam launch.

A custom game has one section. It does not provide chapter splitting, direct chapter launching, or Full Install. Adding an entry does not add support for a game engine other than GameMaker or make an unsupported DATA format readable.

Hide an entry when you want to keep it without displaying it in the main selector. The delete confirmation has **Remove from registry only** and **Remove and clean related data** choices. Cleanup also removes related profile selections and saved Game Versions. It does not uninstall the game. Export any Game Versions you want to retain before choosing cleanup.
