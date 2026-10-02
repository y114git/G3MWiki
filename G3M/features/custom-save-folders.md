# Custom Save Folders

Custom Save Folders chooses a separate save-folder name for a game, profile, or selected mod. Install the plugin from **Settings > Catalog > Plugins**, enable it, and open its panel in the main window.

The plugin changes the game's save location for a modded session. It does not copy your current saves into the new folder. The game can start with empty slots until you create or copy saves there.

## Add a folder

1. Configure the game's installation and user-data folders in **Settings > Game**.
2. In the plugin's folder list, click **Add Custom Save Folder**.
3. Choose a game and a profile scope.
4. Enter a save-folder name, such as `Undertale_Testing`, and click **Create**.

The value is a folder name, not an absolute path. Names cannot contain path separators, reserved filename characters, or a final space or dot. **Global** makes an entry available to every profile; a named profile limits it to that profile.

Enable an entry to make it available as a fallback. Drag entries to set their fallback order. The game filter changes which entries you see; it does not change their game or profile scope.

**Open AppData** opens the user-data location on Windows. Locate the game's actual save directory before copying files yourself, especially for a Windows game running through Proton or Wine.

## Link a mod to a folder

1. Click **Add Mod Rule** in the rule list.
2. Choose the profile, game, installed mod, and folder.
3. Click **Create** and enable the rule.
4. Drag the rule above other rules if it should take precedence.

Only folders for the chosen game and either that profile or Global are available. A rule refers to the mod's ID, so another version with the same ID can use the same rule.

The selection follows this order:

1. The first enabled rule whose mod is selected and installed in the launched profile wins.
2. If no rule matches, the first enabled folder for that game and profile wins.
3. If neither exists, the plugin leaves the save location unchanged.

A matching rule can use a folder whose fallback switch is disabled. Use this to reserve a folder for one mod while excluding it from ordinary fallback selection.

For example, a Global folder named `Undertale_DefaultMods` can be the fallback, while a rule for `translation_test` selects `Undertale_TranslationTest`. Put a more specific rule above a general one when several selected mods have rules.

## Remove an entry

Deleting a folder entry or rule removes its plugin configuration. It does not delete save folders from disk.

A rule marked **Mod not found** or **Folder not found** is ignored. Reinstalling the same mod ID can make its existing rule usable again. Delete a rule you no longer need rather than assuming the warning removes it.

## Sessions and shortcuts

Normal Launch restores the game's original DATA after exit while leaving the separate save folder on disk. **Launch and keep changes** and **Patching only** retain the selected save-folder change along with the other committed changes.

The plugin's shortcut summary records the resolved folder name. Existing shortcuts keep that choice when you reorder rules or folders. Create another shortcut to capture a different choice, and include the plugin in the shortcut.

The game must use a supported DATA file and its save-folder setting must affect its actual saves. This plugin cannot redirect every game's independently implemented file access. Back up saves before testing a new game or combining save-management plugins.
