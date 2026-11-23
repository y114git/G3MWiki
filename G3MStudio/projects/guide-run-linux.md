# Guide: run the game on Linux with Proton or Wine

This guide starts a Windows GameMaker game from G3MStudio on Linux. You end with the game running from **F5** and its output in the Output panel.

## Before you start

- A Windows game folder with the game `.exe` next to the data file (or one folder up).
- Wine, Proton (through Steam), umu or PortProton installed.
- For a Steam game, the game installed in a Steam library so the data file lies under `steamapps/common/`.

## Steps

1. Open the data file in G3MStudio.
2. Open **Settings > Run**. In the Compatibility group press **Detect**. The line "Detected now" says what **Automatic** would use, for example "Auto would use: Proton 9.0, prefix ...".
3. If it says "Nothing found: no Wine, Proton, umu or PortProton", install a tool, or choose it in **Run Windows games with** and set **Tool program or folder** (for Proton, the folder that holds the `proton` file).
4. Optional: set **Wine prefix folder**. Leave it empty to use the Steam prefix of a Steam game. Without one, Proton creates `~/.local/share/G3MStudio/proton/<game>` and Wine uses `~/.wine`.
5. If the game needs folders outside its folder (music, language files), add them under **Files and folders to link**, for example `../mus`.
6. Press **F5** (**Run > Run Game**).
7. Watch the Output panel. The first lines say "Running {exe} with {tool} (prefix {prefix})".
8. Stop the game with **Shift+F5** or close it normally.

Result: the game opens on a working copy of your data file. Your data file is not changed.

## Tips

- Edits you make while the game runs apply at the next start. Press **Ctrl+F5** to restart, or turn on **Restart the running game after an edit**.
- For a Steam game, saves live in the Steam prefix. A different prefix has different saves.

## If something goes wrong

| Message | Fix |
|---|---|
| "This is a Windows game and no Wine, Proton or PortProton was found. Install one, or choose a tool in Settings > Run." | Install a tool or set one. |
| "The game was not found next to {file}. Put the game executable in the same folder." | Check the `.exe` name and place. |
| "Could not start the game: {reason}" | Check the tool path and that it is executable. |
| "{name} ended with exit code {code}. Last output: ..." | Read the Output panel; click an underlined `gml_...` line to open the code. |

## See also

- [Run on Linux and macOS](run-linux-macos.md)
- [Run settings and profiles](run-settings.md)
- [Run the game](run-game.md)
