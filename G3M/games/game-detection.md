# Game detection and paths

G3M needs the installation folder to find the executable and GameMaker resources. The **data folder** setting identifies the game's per-user files, such as saves and configuration. These folders serve different purposes.

## Installation folder

Set the path in **Settings > Game**. For built-in games, G3M checks known executable names and supported layouts. Automatic detection searches supported installations; it does not find every manually moved or renamed copy.

On macOS, use the game's application bundle and check that its resource files are present. GameMaker resources commonly use `.win`, `.unx`, `.ios`, or `.droid`; a file extension alone does not prove compatibility with a patch.

A custom game uses the executable and data file names recorded in [Game Manager](game-manager.md).

## User-data folder

Set the separate game **data folder** when automatic detection does not locate it. G3M checks the platform's user-data directories and, for applicable Steam installations on Linux, Proton's user-data location.

Mods using `${game_data_path}` depend on this setting. Selecting the installation folder as the user-data folder can direct their changes to the wrong location.

## Custom executable

**Custom EXE** overrides the executable used for the selected game. It does not change the game's resource folder or the contents of a mod. Use it for an installation with a different executable name or a required local launcher.

For Linux Windows-game installations, configure Wine or PortProton as appropriate. A working G3M installation cannot run a Windows executable without the required runtime.

## Path checks

If launch reports a missing executable or DATA file, open the chosen folder and compare its contents with the configured game. Check the selected game, executable override, and available chapter files before changing patch settings.
