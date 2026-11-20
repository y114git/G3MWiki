# Steam launching

Enable **Launch via Steam** in **Settings > Game** or in the menu beside the main action button, for a game with a Steam App ID. For a game without one the checkbox is disabled.

G3M applies the selected mods before asking Steam to start the game. In the normal launch mode, G3M restores the affected files after the monitored game session ends. Keep G3M running until restoration finishes.

The built-in Steam entries are DELTARUNE, DELTARUNEdemo, UNDERTALE, and Pizza Tower. A custom game can specify a Steam App ID in Game Manager.

Steam must be installed and able to launch that game. An App ID does not install the game, replace its configured folder, or prove that the selected patch matches the installed release.

## DELTARUNE direct launch

DELTARUNE's direct section launch cannot run through Steam. Choose normal game startup to use Steam, or disable Steam launch to open a section directly.

## Linux

Check that G3M's game folder is the installation Steam actually launches. Windows games may use a Proton prefix with a separate save directory. Set the game **data folder** to that prefix's user-data folder when mods require `${game_data_path}`.

## Different game folder

With mods selected, G3M can warn before launch: **Warning: Launching Mods Through Steam**. G3M applies mods to the configured game path, and Steam may start the game from another folder, which is common on Steam Deck. Make the configured path match Steam's install folder, or turn off **Launch via Steam** and launch from the configured path. This prompt is the **Steam may launch a different game folder** warning in [Manage Warnings](../features/warnings.md).
