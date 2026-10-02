# Getting started

## Install G3M

Choose the release package for your operating system and processor from [GitHub releases](https://github.com/y114git/G3M/releases). x64 and ARM64 are separate downloads; select the architecture of the machine running G3M.

| Operating system | Minimum requirement | Architectures |
| --- | --- | --- |
| Windows | Windows 10 version 1809 | x64, ARM64 |
| Linux | glibc 2.34 | x64, ARM64 |
| macOS | macOS 13 | x64, ARM64 |

Extract the package into a folder you can write to, and open G3M. Keep the accompanying files together. Release packages contain the application runtime; Python is not required.

The requirements apply to G3M. A game's executable and a mod can have different platform requirements. An ARM64 G3M package does not convert an x64 game into an ARM64 game.

## Set the game folder

1. Choose your game in the Library.
2. Open **Settings > Game**.
3. Select the game's installation folder. This is the folder containing the executable and game resources, rather than its save folder.
4. Check the separate **data folder** setting if a mod changes saves or other per-user game files.
5. Launch without selected mods once to check the path.

G3M attempts to locate supported installed games automatically. Set the path manually if detection does not find your installation. See [game detection](games/game-detection.md) for executable overrides and platform-specific layouts.

## Install and play a mod

1. Find a mod in **Mods Browser**, open its details, and select a download. Alternatively, drop a mod archive or folder into the Library.
2. Complete the import dialog. Files without a G3M configuration require you to specify the game and intended file operations.
3. Open **Library**, locate the imported mod, and click **Use**.
4. Keep the normal **Launch** mode selected, and launch the game.
5. Let G3M finish restoring files after the game closes before launching another session.

For several selected mods, review [Priority & Steps](mods/modpacks.md). A dependency can require a separate step, and matching game names alone do not establish compatibility.

## Find the main controls

**Settings** controls game paths, appearance, download behavior, and the Catalog. The profile selector switches independent mod libraries. **Windows** opens auxiliary windows, including logs and the Support Packager. **Help > Run Onboarding** opens the guided tour.

G3M keeps settings and mods in a separate [data directory](advanced/data-directory.md). Replacing the application package does not replace that directory.
