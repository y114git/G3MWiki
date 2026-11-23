# Settings file

All settings live in one JSON file named `settings.json` in the G3MStudio user data folder. This page covers where it is, its format, how to edit or reset it by hand, and what happens when it is damaged or cannot be written.

## Location

| System | Folder |
| --- | --- |
| Windows | `%APPDATA%\G3MStudio` |
| Linux | `~/.config/G3MStudio` |
| macOS | the application data folder of the user (see [User data](../getting-started/user-data.md)) |

The environment variable `G3MSTUDIO_DATA` replaces the folder. **Settings** > **Advanced** > **Open settings file** opens the file, and **Help** > **Open Data Folder** opens the folder.

If the folder cannot be created (read-only profile, full drive), the session runs on a temporary folder named `G3MStudio-<user>` and loses its settings at the next clean-up. A warning says "The G3MStudio data folder could not be written, so crash recovery and remembered settings may be out of date. Your edits stay in memory: {reason}".

## Format

- JSON, indented, keys in camelCase, enum values as text (`"Restore"`, `"Dim"`).
- The file is written whole after every change, ending with a line break.
- Only the keys listed on the settings pages are read. A key you add that the build does not know is dropped when the file is next written.
- Values that are out of range are clamped on load, for example `codeFontSize` 5 becomes 8.
- Most keys are listed with their defaults in [Default values](defaults-table.md). Keys that hold maps and lists are described below.

```json
{
  "language": "en",
  "uiScale": 125,
  "density": "Compact",
  "colors": { "Accent": "#039D5B" },
  "sizes": { "Control.Height": 32 },
  "shortcuts": { "settings.open": "Ctrl+Alt+P" },
  "shortcutAliases": { "view.zoomIn": ["Ctrl+Add", "Mouse4"] },
  "prompts": { "deleteResource": "Yes" },
  "overrides": { "Radius.Window": "10" }
}
```

### Keys that hold data, not options

| Key | Holds |
| --- | --- |
| `window` | Position, size and maximized state of the main window (x, y, width, height, maximized). |
| `lastSession` | The data file open when G3MStudio last closed, with size and MD5. |
| `savedLayouts` | Your saved dock layouts, name to layout (**Window** > **Layout** > **Saved Layouts**). |
| `paneSizes` | Sizes you dragged, such as the height of a property list. They load again when the tab opens. **Window** > **Layout** > **Reset Layout** puts the panels back to the preset. |
| `explorerZoom`, `spriteZoom` | The last zoom of the Explorer list (50 to 250) and of the sprite picture view. |
| `dialogSizes` | Sizes of dialogs you resized, by window type (width 200 to 10000, height 100 to 10000). |
| `favouriteFunctions` | Functions starred in the Function Reference and the completion list. |
| `renameStrings` | The rename dialog's **Also rename matching strings** box (default true). |
| `fileRunProfiles`, `runProfiles`, `trustedRunFiles` | Run profiles, the profile each data file used last and the hashes of approved run files. |
| `onboardingDone` | The welcome window was answered or skipped. |
| `installId` | Random install ID (see [Privacy](privacy.md)). |
| `reduceMotion` | Old switch kept to migrate saved settings. Use `animations`. |

## Edit by hand

1. Open **Settings** > **Advanced** > **Open settings file**, or open the file in any editor. G3MStudio may stay open.
2. Change a value and save.
3. The main window watches the file and applies the change at once. A file that does not parse is ignored until it does.

A change you make in Settings after a hand edit starts from the edited file, so the edit is not lost. If your hand edit does not parse when G3MStudio next writes the file, the text is first moved aside as `settings.json.invalid`.

## Reset

| What | How |
| --- | --- |
| One category | **Restore defaults** on that category. |
| All prompts | **Reset all** in **Prompts**, or the command **Ask Again for All Prompts**. |
| Everything | Close G3MStudio and delete `settings.json`. The next start uses the defaults and shows the welcome window again. |

## Damaged or unreadable file

| Situation | Result |
| --- | --- |
| The file is not valid JSON | It is renamed to `settings.json.invalid` and the defaults load. If it cannot be renamed, the next change overwrites it. |
| One value has the wrong type or an unknown enum value (for example from a newer build) | That one setting falls back to its default. Up to 16 such settings are dropped. The rest of the file loads. |
| The file is held by a virus scan or another instance | Reading is tried three times, 50 ms apart. If it stays locked, nothing overwrites the file until it can be read, and changes stay in memory. |
| Two instances write | Each change re-reads the file first, so another instance's change is the base of yours. |

## Full drive and failed writes

The file is written to a temporary file next to it and renamed over it. A crash or a failed write leaves the old file or the new one, never half of each. If the drive is full, a leftover temporary file may stay and is removed on a later start (temporary files older than one day are swept). The change stays in memory for this session. The message "The drive is full, so the last change could not be saved. Free some space and try again." appears for the write errors that reach the interface.

## See also

- [User data](../getting-started/user-data.md)
- [Files and folders](../reference/files-and-folders.md)
- [Default values](defaults-table.md)
- [Advanced settings](advanced.md)
