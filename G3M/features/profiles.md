# Profiles

A profile contains its own installed mods, selected game, selected mods, priority steps, Chapter Mode, direct section selection, and Full Install state. Switch profiles to keep a playthrough separate from mod testing.

Game paths, appearance, plugin settings, downloads, and saved Game Versions are shared application data. A profile is not a separate game installation or save directory.

## Switch or create

Use the main window's profile selector to switch. **Profile Manager** provides these actions:

| Action | Result |
| --- | --- |
| Create | Adds an empty profile |
| Duplicate | Copies the selected profile and its installed mods |
| Rename | Renames a non-default profile |
| Delete | Removes a non-default profile and its local mod files |
| Export | Writes the selected profile to ZIP |
| Import | Adds a profile from ZIP |
| Use | Activates the selected profile |

Drag profiles in the list to reorder the selector. Drop profile ZIPs into the manager to import them.

The **Default** profile cannot be renamed or deleted. Names used on disk are trimmed, limited to 80 characters, and adjusted to avoid unsupported characters and reserved Windows names.

## Test without changing a working setup

1. Select your working profile in Profile Manager.
2. Click **Duplicate** and name the copy `Testing`.
3. Use the copy and change its mods or steps.
4. Switch back to the original profile to return to its Library selection.

This protects the profile's mod set. It does not isolate game saves, shared plugin settings, or changes intentionally kept in the game folder.

## Export and backup

A profile ZIP contains its profile state, Library metadata, and mod files. It does not contain shared settings, plugins, logs, saves outside the profile, or Game Versions. Use [backup and restore](../advanced/backup-and-restore.md) for a complete personal backup.

Profiles are stored in `profiles/<name>/` under the [G3M data directory](../advanced/data-directory.md). The profile state is `<name>.json`; `mods_data.json` records Library metadata.
