# Run the game

The **Run** menu starts the game on your current edits, shows its output and stops it. This page covers the commands, how the game is found and started, the Output panel and what happens when the game ends.

## Commands

| Command | Shortcut | What it does |
|---|---|---|
| **Run Game** | **F5** | Starts the game on the open data file. |
| **Restart Game** | **Ctrl+F5** | Stops the running game and starts it again on the latest edits. Starts it when none runs. |
| **Run with Runtime...** | none | Starts the game once with a GameMaker runtime you pick. |
| **Stop Game** | **Shift+F5** | Asks the game to close and kills it after a grace time. |
| **Play This Room** (Room menu) | **Ctrl+Shift+F5** | Starts the game at the room of the room tab in front. |

Run Game, Restart Game and Run with Runtime need an open file. Stop Game says "No game is running." when nothing runs.

From outside: the Explorer verb "Run game with G3MStudio" and the command-line flags `--run` and `--run-with-runtime` (first argument that is not a flag is the data file) open the file and run it.

## What a run does

1. Reads the options: an explicit runner, a run options file of the data file, then [Settings > Run](run-settings.md).
2. Finds the program (see below). On Linux and macOS a Windows game goes through [Wine or Proton](run-linux-macos.md).
3. Makes the files from **Files and folders to link** available next to the game (see [Run settings](run-settings.md#files-and-folders-to-link)).
4. Applies code you typed in editors. A code entry that does not compile stops the run.
5. Writes a **working copy** of the data file next to it, named `g3mstudio_run.tmp`, and starts the game on it. Your data file is not changed. The notification says "The game runs from a temporary copy of your data file. Changes the game makes are not kept, and your data file is not changed." (once per session; turn it off with **Warn that a run is a temporary copy**.)
6. Shows "Starting {name}..." then "{name} is running." The status bar shows "Running: {name}".

A quick restart skips the copy when nothing changed since the copy on disk was made. The copy is deleted when the game ends, except during a restart.

If the option **Save the data file and run on it instead of a working copy** is on, the run saves your data file first (a backup is kept as usual) and starts the game on it: "The run saves your data file first (a backup is kept as usual) and starts the game on it." A room start never runs in place, because it changes the room order.

The game starts with the arguments `-game <data file>` unless your extra arguments contain `{data}`, which stands for the data file. With **Write the game log (-debugoutput)** it also gets `-debugoutput <data file>.gamelog.txt`. The working folder is the data file folder unless set.

## How the game is found

When **Runner** in Settings is empty, G3MStudio looks next to the data file, in the folder above it (Steam keeps `game.unx` in `assets/`) and, on macOS, in a `MacOS` folder beside it. The names it tries, in this order:

- Game name (from the data file's General Info file name, else the data file name) with `.exe`, no extension, `.x86_64`, `.AppImage`, and on Linux and macOS `.sh`.
- `runner`, `Runner`, `runner.exe`, `Runner.exe`, `Mac_Runner`, `game.x86_64`.

On Linux and macOS `.exe` names count only for the [Windows compatibility path](run-linux-macos.md). Errors: "The game was not found next to {file}. Put the game executable in the same folder." and "The program {path} was not found. Set it in Settings > Run."

On Windows, when none of those names exists, G3MStudio looks at the `.exe` files in the data file folder (not the folder above). Helpers are skipped: names that start with `unins`, `vc_redist`, `vcredist`, `crash`, `unitycrash`, `dxsetup`, `dotnet`, `oalinst`, `setup`, `updater`, `installer`, `redist`, `steamerrorreporter` or `notification_helper`, and names that contain `crashhandler` or `crashpad`. Names may hold any characters, such as `$while_1.exe`, spaces or non-Latin letters, and `.EXE` counts too.

An `.exe` is taken at once when it is the only one left, or when exactly one matches the data file name, the game name or the folder name. The match uses letters and digits only, ignoring case, so `$while_1.exe` matches `while_1.win`. When several remain and none or more than one matches, the dialog "G3MStudio could not tell which program starts the game. Pick one:" lists them, matches first, and you pick the program. **Settings > Run > Runner** always overrides all of this.

Example: a folder holds `DELTARUNE.exe`, `data.win` and `vc_redist.x64.exe`. The redistributable is skipped, so `DELTARUNE.exe` is the only candidate and runs without a question.

A program whose name is not the game name and does not contain `runner` runs anyway, with a warning once: "{path} is not a known game runner. It runs anyway; its output and exit code are shown here."

## Run with Runtime...

Lists the GameMaker runtimes on this PC and starts the game with the one you pick, for this run only. The list "Run the game once with this runtime:" has the columns **Version** and **Path**. The first row, **Game**, is the game's own program, the one a plain run uses.

Runtimes are searched in `GameMakerStudio2`, `GameMakerStudio2-LTS` and `GameMakerStudio2-Beta` under `Cache\runtimes` in the Windows common application data folder (`runtime-VERSION` folders with `windows\x64\Runner.exe` or `windows\Runner.exe`), plus the folders in **Settings > Run > Runtime folders**. GameMaker Studio 1.4 is listed as version 1.4 when `Runner.exe` is in `GameMaker Studio` under Program Files (x86). Newest first. With none: "No GameMaker runtime found. Add its folder in Settings."

## Play This Room

**Play This Room** moves the room of the front room tab to the first place of the room order in the working copy and starts the game there. The warning reads "Starting at room {room}: the game opens there directly, so anything an earlier room sets up (globals, saved state, persistent objects) is missing and the game may fail or look wrong. A room that is not in the room order cannot be started this way." Your room order in the data file is not changed.

## The Output panel

**View > Panels > Output** shows the game output and G3MStudio notifications in time order.

| Control | Purpose |
|---|---|
| **Filter** | Shows only rows that contain the text. |
| **Times** | Shows a `HH:mm:ss.fff` time before each row (on by default). |
| **Copy** | Copies the visible rows. |
| **Clear** | Clears the output and the notification history. |
| **Restart** | Same as Restart Game. On when the game runs or nothing runs. |
| **Stop** | Same as Stop Game. On only while the game runs. |

Error-stream lines start with `! `. A line that names a code entry and line, such as `gml_Object_obj_player_Step_0 (line 12)`, is underlined; click it to open the code at that line. The Output keeps the last 5000 game lines. An empty panel says "Nothing has been reported yet."

## When the game ends

| How | Result |
|---|---|
| Exit code 0 | "{name} exited with code 0." |
| You stopped it | "Game stopped." |
| Other exit code | Error "{name} ended with exit code {code}. Last output: {lines}" (last 5 lines). The status bar shows "Game crashed (code {code})". A Problems entry "Game ended with exit code {code}: {line}" opens the failing line when the output names one. |

### Games started through Steam

A game with `steam_api*.dll` or `Steamworks*.dll` in its folder often ends at once with code 0: it asks Steam to start it again, and Steam launches the same program. Steam may then show a confirmation for the custom arguments (`-game ...`). Accept it. If the game ends within 20 seconds with code 0 and another process of the same program runs now or starts within 15 seconds, G3MStudio treats the run as still going. It keeps the working copy and the linked files until that process ends, and says "The game was started again by its launcher (Steam). Steam may ask you to confirm the arguments, so accept it. G3MStudio keeps the run copy until the game ends." With the Steam libraries in the folder the Output also says "This game started through Steam. G3MStudio keeps waiting...". **Stop Game** ends the wait but never touches the game Steam started. Turn the wait off with **Settings > Run > When the game hands over to another process (Steam, launchers)**: the choices are **Wait for it** (default) and **Do not wait**. With **Do not wait** the run ends when the first process exits and the working copy is deleted at once, so the game that Steam starts may find its data file gone. A working copy left behind by a crashed run is removed when the project opens, unless the game still runs.

**Stop Game** asks the game to close (the window on Windows, `SIGTERM` to the whole process tree on Linux and macOS) and kills it when it is still there after **Seconds to wait for the game to close before it is killed** (default 5). Only the process G3MStudio started is touched. Closing the window kills the game at once.

**Restart the running game after an edit** restarts the game 1.5 seconds after your edits pause. Default off.

## Troubleshooting

| Message | Fix |
|---|---|
| "The game is already running." | Stop it first, or use Restart Game. |
| "Could not start the game: {reason}" | The program could not start. Check the runner path and permissions. |
| "This is a Windows game and no Wine, Proton or PortProton was found. Install one, or choose a tool in Settings > Run." | See [Run on Linux and macOS](run-linux-macos.md). |
| The game has no music or text | Folders it needs may sit beside the game folder. See [Files and folders to link](run-settings.md#files-and-folders-to-link). |

## See also

- [Run settings and profiles](run-settings.md)
- [Run on Linux and macOS](run-linux-macos.md)
- [Guide: run the game on Linux with Proton or Wine](guide-run-linux.md)
