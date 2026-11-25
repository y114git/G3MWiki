# Dialogs

| Dialog | When to use it | Guide |
| --- | --- | --- |
| Import and Manual Mod Installation | Add a package or configure loose files | [Importing](../mods/importing.md), [Manual Mod Installation](../mods/importing.md#files-without-configuration) |
| Mod Editor | Edit metadata, operations, placeholders, and relationships | [Editor](../mods/mod-editor.md) |
| Mod Versions | Save, import, switch, or delete mod checkpoints | [Mod Versions](../mods/mod-versions.md) |
| Profile Manager | Create, copy, use, reorder, and share libraries | [Profiles](../features/profiles.md) |
| Game Manager | Manage visible games and custom entries | [Game Manager](../games/game-manager.md) |
| Game Versions | Save and restore game installations | [Game Versions](../features/game-versions.md) |
| Priority & Steps | Arrange the selected mods | [Priority and steps](../mods/modpacks.md) |
| Diagnostics | Inspect conflicts and test application | [Diagnostics](../features/diagnostics.md) |
| Downloads | Manage downloads and imports | [Downloads](../features/downloads.md) |
| Modding Tools | Create, apply, merge, convert, and compare DATA inputs | [Modding Tools](../features/modding-tools.md) |
| Support Packager | Save selected diagnostic information locally | [Support packages](../features/support-packages.md) |
| Blocklist | Hide matching mod entries | [Blocklist](../features/blocklist.md) |
| Update Mods | Check GameBanana-linked mods and install chosen updates | [Updates](../features/update-system.md) |
| Community | Read GameBanana feeds | [Community](../features/community.md) |
| Manage Warnings | Choose which warnings interrupt patching | [Manage Warnings](../features/warnings.md) |
| Log Viewer | Read the G3M, Patching, and Conflicts logs | [Title bar](title-bar.md) |
| Create Shortcut | Save a launcher | [Shortcuts](../features/shortcuts.md) |
| Theme | Import, export, and apply theme packages (**Im/Export Theme**) | [Theme packages](../customization/theme-packages.md) |
| Conflicts report | Read the patching conflicts from a report (**Open Report**) | [Diagnostics](../features/diagnostics.md) |
| Changelog and About | Read release notes and the installed version | [About and changelog](about-and-changelog.md) |
| Getting Started | Restart the guided tour | [Title bar](title-bar.md) |
| Announcements | Read notices and answer polls from G3M's online service | [Announcements](../features/announcements.md) |

Open dialogs update their language, theme, font, and scale when those settings change. Entered text, selections, and operation progress are preserved.

File, save, and folder dialogs open in the folder of your last pick. G3M keeps that folder in `last_dialog_dir.txt` in the user data folder.

Dialogs with destructive actions show confirmations. Read the named target: deleting a download, a mod version, a Library mod, and a game snapshot removes different files.

## Prompts you may see

| Prompt | Meaning |
| --- | --- |
| **GameBanana Remote Install** | Asks to link this G3M to your GameBanana Remote Install after you pair on the site. See [One-click install links](../advanced/one-click-install.md#gamebanana-remote-install). |
| **Update available** | Offers an application update. See [Updates](../features/update-system.md). |
| **Game not added** | A mod needs a game that is not set up. See [Custom games](../games/custom-games.md). |
| **Patching Warning** | A patching, dependency, or order problem. A general warning has **Continue** and **Cancel**. **Open Report** appears when a conflicts report exists, and **Do not show again** appears for warnings that allow it. See [Manage Warnings](../features/warnings.md). |
| **Startup Error** | G3M could not start. The message gives the cause. Send the log with a report. |
| **Import Previous Data?** | Legacy DELTAHUB data was found. **Import Data** copies it, **Start Fresh** does not. |
| **G3M data location unavailable** | The configured data folder cannot be read. Choose **Try again**, **Choose another folder**, or **Use default location**. |
| **API Rate Limit** | GameBanana's 250 requests per hour limit was reached. Wait and try again. |
