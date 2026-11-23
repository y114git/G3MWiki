# Run on Linux and macOS

G3MStudio runs native games directly and starts Windows games through Wine, Proton, umu, PortProton or a command you write. This page explains how it chooses, what each tool needs and what the Compatibility settings do.

## How the choice is made

With **Run Windows games with** set to **Automatic**, G3MStudio takes the first of these that exists:

1. A native runner next to the data file or one folder up (`<game>`, `<game>.x86_64`, `<game>.AppImage`, `<game>.sh`, `runner`, `Runner`, `Mac_Runner`, `game.x86_64`), or in a `MacOS` folder beside it.
2. On macOS, an `.app` bundle next to the data file or one folder up. Its program is the one `Info.plist` names (`CFBundleExecutable`), else the only file in `Contents/MacOS`. A binary `Info.plist` is not read.
3. A Windows `.exe` (`<game>.exe`, `runner.exe` and similar, matched in any letter case) started with the first tool found, in this order: Proton, umu, Wine, PortProton.

**Proton** is used by **Automatic** only for a Steam game, that is, when the data file lies in `steamapps/common/<folder>` and an `appmanifest_*.acf` of that `steamapps` names the folder as its `installdir`. The other tools work for any game.

If a Windows game is found and no tool exists: "This is a Windows game and no Wine, Proton or PortProton was found. Install one, or choose a tool in Settings > Run."

The run log starts with a line such as "Running {exe} with {tool} (prefix {prefix})". For a Steam game it adds "Steam keeps this game's Wine prefix and its saves in {folder}; a plain Wine prefix is a different place." (not on macOS). When Proton will create a new prefix: "The prefix folder {folder} does not exist yet. Proton creates it on this run."

## The Compatibility settings

In **Settings > Run**, the group **Compatibility (Windows games on Linux and macOS)**:

| Option | Meaning |
|---|---|
| **Run Windows games with** | **Automatic** (default), **Native only**, **Wine**, **Proton**, **umu**, **PortProton**, **Custom command**. |
| **Tool program or folder (empty detects it)** | The program for the chosen tool. For **Proton**, the `proton` file or the folder that holds it. **Browse...** picks it. |
| **Wine prefix folder (empty uses the Steam prefix of a Steam game, else the tool's default)** | The prefix to use. |
| **Custom command: {exe} {data} {dir} {prefix} {args}** | Shown for **Custom command**. |
| **Detected now** and **Detect** | Shows what **Automatic** would use now: "Auto would use: {tool}", "Auto would use: {tool}, prefix {prefix}" or "Nothing found: no Wine, Proton, umu or PortProton". |

**Native only** never uses a compatibility tool; a Windows game is not started.

## Tools

| Tool | Program looked up | Prefix used when none is set |
|---|---|---|
| **Wine** | `wine`, `wine64`. On macOS also `wine-stable`, `Wine*.app` in `/Applications`, CrossOver, and `gameportingtoolkit`. | An existing Steam prefix, else `WINEPREFIX`, else `~/.wine`. |
| **Proton** | The newest Proton: the Steam root's `compatibilitytools.d` and every Steam library's `steamapps/common/Proton*`. An Experimental build counts as the newest. | The Steam `compatdata/<appid>` of a Steam game, else `~/.local/share/G3MStudio/proton/<game>` (or under `XDG_DATA_HOME`). |
| **umu** | `umu-run` | The Steam prefix if it exists, else `~/Games/umu/umu-<appid>` (`umu-0` for a non-Steam game). The game id is `umu-<appid>`. |
| **PortProton** | `portproton`, the Flatpak `ru.linux_gaming.PortProton`, or `~/PortProton/data/scripts/start.sh`. | PortProton keeps its own prefixes, unless you set one. |

The Steam root is searched in `STEAM_COMPAT_CLIENT_INSTALL_PATH`, `~/.local/share/Steam`, `~/.steam/steam`, `~/.steam/root`, the Flatpak and the Snap Steam folders, and the libraries in `libraryfolders.vdf` count too. Tools are looked up on `PATH`, then in `/usr/local/bin`, `/opt/homebrew/bin`, `/usr/bin` and `~/.local/bin`, because a program started from a desktop icon has a short `PATH`.

A Steam prefix counts only when it already exists, so Wine never creates a half prefix in Steam's folder. G3MStudio sets `WINEDEBUG=-all` unless you set `WINEDEBUG` yourself (in your environment or in **Environment variables**).

The data file is given to the Windows game as its name when it lies in the working folder, otherwise as a `Z:` path with backslashes (Wine maps `Z:` to `/`).

### Custom command

Write the whole command. G3MStudio replaces these tokens:

| Token | Replaced by |
|---|---|
| `{exe}` | The Windows game program. |
| `{data}` | The data file (as the game sees it). |
| `{dir}` | The working folder. |
| `{prefix}` | The prefix folder. |
| `{args}` | The game arguments as separate arguments. |

Inside a larger word, each argument is quoted so a path with spaces stays whole. A command with neither `{args}` nor `{data}` gets the arguments at its end. The first word must be a full path (`~/` works) or a program on `PATH`.

## Saves

For a Steam game under Proton or Wine on Linux, saves live in `<compatdata>/pfx/drive_c/users/steamuser/AppData/Local/<game>`. A run from a plain Wine prefix writes to that prefix instead, so the two do not share saves.

## Troubleshooting

| Problem | Fix |
|---|---|
| "Nothing found: no Wine, Proton, umu or PortProton" | Install one, or set **Tool program or folder**. |
| The game starts but has no saves | The prefix differs from the one Steam uses. Set **Wine prefix folder** to the Steam `compatdata/<appid>/pfx`. |
| The tool cannot be found from a desktop icon | Set **Tool program or folder** to a full path. |

## See also

- [Run the game](run-game.md)
- [Run settings and profiles](run-settings.md)
- [Guide: run the game on Linux with Proton or Wine](guide-run-linux.md)
