# Game Versions

**Game Versions** saves snapshots of the selected game's installation files. Snapshots remain available until deleted; they are separate from temporary launch backups and Mod Versions.

Open the Game Versions control near the game selector. The dialog's game selector chooses which game's saved versions are shown.

## Create

Use the add control and choose **Create Game Version**. Enter a name and select the profile option:

- **Without profile** saves the files currently in the configured game folder.
- A named profile builds a snapshot with that profile's selected mods applied to a temporary copy. The real installation is not the patching output for this operation.

For a clean baseline, choose Without profile while the game folder contains the clean installation. A folder that already contains kept mod changes produces a snapshot of those changes.

The game executable and configured executable override are protected from snapshot replacement. Saves outside the installation folder are not included. Keep those separately if you need a complete backup of a playthrough.

## Apply

Choose **Apply** on a saved version and confirm the destination. Files from the snapshot overwrite corresponding installation files.

**Settings > Library > Game Versions > Full file replacement on apply** also removes unprotected files absent from the snapshot. With that option off, unrelated files can remain in the folder. Save the current state before applying a version if you want to retain it.

Applying a snapshot changes the game folder, not the active profile's installed mods or selected-mod state. Do not apply one while a game session or launch restoration is running.

## Import, export, and delete

**Import Game Version** reads an exported version ZIP with `game_version_data.json`. An arbitrary game ZIP without that manifest is not a Game Versions export. The game recorded in the manifest must match the selected game.

Export copies the saved archive for external storage. Delete removes the saved record and archive after confirmation; it does not remove the game installation.

Use **Cancel** on a working record to request cancellation. A missing-archive status means the indexed file is unavailable. An error recorded while creating a patched snapshot deserves inspection before you use that snapshot as a baseline.

Archives and their index live in `game_versions/` under the [data directory](../advanced/data-directory.md).
