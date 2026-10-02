# Settings

Settings are grouped into **General**, **Appearance**, **Game**, **Mods Browser**, **Library**, and **Catalog**. Most changes apply when a control changes or when you finish editing a field. There is no separate save-all step.

Click a section heading to collapse or expand it. **Show Reset Buttons** reveals reset controls beside individual settings and section headings. A section reset affects that section's settings; it is not an uninstall or a library deletion.

## General

| Control | Effect |
| --- | --- |
| Language | Select the interface language. Translated views and plugin translations use the selection. |
| Beta Updates | Include beta application updates in update checks. |
| Fullscreen | Toggle the main window's fullscreen state. |
| UI Scale | Scale text and controls from 50% to 200%, in 10% increments. |
| Show Reset Buttons | Show individual and section reset controls. |
| G3M data location | Choose the root folder for G3M's settings, profiles, mods, downloads, themes, and plugins. |

Changing the data location offers copying existing data or using the selected location without copying. Copying refuses conflicting destination entries rather than replacing them. The application restarts to use the chosen location. The original data remains available. See [Data directory](../advanced/data-directory.md) before moving a large library or using an external drive.

## Appearance

### Themes

**Im/Export Theme** opens the theme management dialog. **Import** applies a package; **Export** saves the current appearance as a ZIP. The saved-theme list has controls to apply, save, and delete a theme.

**Do not save theme in list after import** applies an imported file without adding a saved copy to the theme list. It does not mean "do not apply."

Download optional packages from [Catalog](../features/catalog.md). Installed catalog themes also appear in the Appearance list.

### Graphics, audio, styling, and colors

The background, logo, and font controls select a custom file. When a custom file is active, the corresponding control offers removal. The audio controls select or remove background music and a startup sound.

**Border Radius** controls corner rounding from 0 to 999 pixels. It does not change border thickness. The seven color controls change the background, elements, border, hover, selection, main text, and secondary text. Each accepts a color picker selection or a valid hexadecimal color.

| Checkbox | Effect |
| --- | --- |
| Disable animations | Disable interface animations. |
| Disable background | Hide the background image or video while retaining its setting. |
| Disable startup sound | Suppress the startup sound without removing its file. |
| Stop music when unfocused | Stop background music while G3M is unfocused or minimized. Returning to G3M restarts playback from the beginning. |
| Disable Discord Rich Presence | Stop G3M from publishing activity to Discord. |

See [Customization](../customization/README.md) for supported files and theme packaging.

## Game

Select the game whose paths you want to edit. **Game Manager** controls built-in visibility, ordering, and custom games.

| Control | Effect |
| --- | --- |
| Game path | Installation folder or game location used for launching and `${game_path}`. |
| Game data folder | User-data location used by `${game_data_path}`. This is not the installed `data.win` file. |
| Launch via Steam | Use Steam for a game with a configured Steam app ID. |
| Don't hide window on launch | Keep G3M visible while the game runs. |
| Use PortProton instead of Wine | Use PortProton for Windows executables on Linux. |
| Properties merge | Combine supported structured property changes when merging patches in the same step. |
| Code merge | Attempt three-way merging of GML changes when patches edit the same code. |
| Manage Warnings | Select which patching and merging warnings interrupt an operation. |
| Clear G3MTool Cache | Remove reusable data-file analysis to free space or discard unwanted cached analysis. |
| Custom EXE | Override the game's executable. Saved per game. |
| Custom G3MTool | Override the bundled G3MTool for tool operations. |
| Custom XDELTA | Pass a chosen XDelta executable to G3MTool for XDelta operations. |
| Custom Wine | Prefer this Wine executable when launching a Windows game on Linux. |
| Custom PortProton | Override the PortProton command used when that launch option is enabled. |

Platform-specific launch controls are relevant only on platforms that support them. Changing a tool path does not install that tool. Selecting a data folder does not select or convert a game data file.

Warnings can be suppressed individually or as a group. Suppression lets an operation continue without those prompts; it does not repair failed patches or make conflicting edits compatible. See [Manage Warnings](../features/warnings.md) for each choice.

## Mods Browser

| Checkbox | Effect |
| --- | --- |
| Hide Mods Browser tab | Hide the online browsing view. Installed mods remain in the library. |
| Do not install downloaded files automatically | Keep a finished download ready for an explicit install or use action. |
| Delete downloaded file after use | Remove the downloaded source after it has been used. The installed mod is separate. |
| Save local imports in Downloads | Retain local import sources in Downloads as well. |

See [Downloads](../features/downloads.md) for the difference between a source archive and an installed package.

## Library

| Control | Effect |
| --- | --- |
| Hide Library tab | Hide the library view. |
| Hide filters in library | Hide the library's filtering controls. |
| Full file replacement on apply | When applying a Game Version, remove files absent from its archive. Protected game-original and custom-executable paths are retained. |
| Hide "Update Mods" button | Hide the mod-update button even when updates are available. |
| Automatically update GameBanana mods | Check and update eligible GameBanana-linked library mods without opening the update dialog. |
| Update current versions directly | Skip creation of a Mod Version backup during automatic updates. |
| Automatic update scope | Choose the selected game, every game in the selected profile, or all profiles. |

A Game Version is an installation snapshot, not a Mod Version. **Full file replacement on apply** can remove extra files from the installation. Read [Game Versions](../features/game-versions.md) before enabling it.

Automatic mod updates apply only to eligible linked mods. A local mod without a recognized online source is not updated by this option.

## Catalog

The **Plugins** and **Themes** subtabs have installed-only and type-specific tag filters. See [Catalog](../features/catalog.md) for installation, applying themes, and plugin settings.
