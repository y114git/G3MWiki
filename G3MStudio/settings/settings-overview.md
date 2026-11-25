# Settings page

The Settings page is where every option of G3MStudio is changed. It opens as a page over the work area, not as a separate window, and every control saves its value the moment you change it.

## Open and close

| Action | How |
| --- | --- |
| Open Settings | **File** > **Settings**, or `Ctrl+,` (`Cmd+,` on macOS). The command is also in the command palette as **Settings**. |
| Open from the start page | The **Settings** button on the start page. |
| Open one category | **Go To** (`Ctrl+T`, with a data file open) lists every category by name. Picking one opens Settings on that category. |
| Close | The **Close** button at the top right of the page. |

The command id is `settings.open`. Its shortcut can be changed in [Keyboard shortcuts](shortcuts-editor.md).

## Layout

The page has three parts:

1. A list of categories on the left, titled **Settings categories** for screen readers.
2. The title **Settings**, the **Restore defaults** button, the **Close** button and the search box **Search settings** at the top. These stay in place while the settings scroll.
3. The settings of the chosen category in the middle, up to 640 pixels wide.

The categories, in order:

| Category | Page |
| --- | --- |
| **Appearance** | [Appearance](appearance.md) and [Explorer settings](explorer-settings.md) |
| **Colors** | [Colors and themes](colors-and-themes.md) |
| **Code editor** | [Code editor settings](editor.md) |
| **Image editor** | [Image editor settings](image.md) |
| **Sound** | [Sound settings](sound.md) |
| **Data file comparison** | [Data file comparison settings](diff.md) |
| **Rooms** | [Rooms and auto-tile settings](rooms-and-auto-tile.md) |
| **Auto-tile** | [Rooms and auto-tile settings](rooms-and-auto-tile.md) |
| **Decompiler** | [Decompiler settings](decompiler.md) |
| **Files** | [Files and startup settings](files.md) |
| **Projects** | [Projects settings](projects.md) |
| **Run** | [Run settings](run.md) |
| **Advanced** | [Advanced settings](advanced.md) |
| **Privacy** | [Privacy and crash reports](privacy.md) |
| **Prompts** | [Prompts](prompts.md) |
| **Mod base** | [Mod base](mod-base.md) |
| **Keyboard shortcuts** | [Keyboard shortcuts](shortcuts-editor.md) |

Without a search, only the chosen category shows. The page remembers the chosen category until you close it.

## Search settings

Type in **Search settings** to look through every category at once. The **Clear search** button (a cross inside the box) appears while the box has text.

- A setting shows when its label contains the text. The match ignores upper and lower case.
- When the text is part of a category heading, the whole category shows.
- Categories with no match are dimmed in the list on the left (40 percent opacity), so you can see where the hits are.
- When nothing matches, the page says **Nothing found.** and **Clear the search box to see all settings.**
- Press `Enter` in the box to move the focus to the first visible setting.
- The text you typed stays in the box when the page is rebuilt, for example after a language change.

What the search matches:

| Control | Matched text |
| --- | --- |
| A checkbox | Its label. |
| A row with a box, list or number | The label above the control. |
| A color | The names of all colors, as one block (type "Keyword" to find the code colors). |
| A size | The block title **Shape and size**. |
| A decompiler setting | Its label. |
| A shortcut | The command name, the command id or any of its keys (see [Keyboard shortcuts](shortcuts-editor.md)). |

There is no separate tag list. The label is the only searchable text.

## Restore defaults

**Restore defaults** resets the settings of the category that is selected, not the whole page. Its tip reads "Resets the settings of the selected section." The page rebuilds afterwards.

What each reset covers is listed on the page of its category. Some values are not touched by any reset, for example **Language**, **Previous and next tab buttons** and the Explorer icon size. To start again from nothing, see [Settings file](settings-file.md).

## Related pages

[Plugins](plugins.md), [Update settings and channels](updates.md) and [Keyboard shortcuts and mouse gestures](shortcuts-editor.md) cover the parts of the page that sit outside a category.

## Language

Changing **Language** rebuilds the page and the main window in the new language. The page also follows a language chosen from the command palette.

## Live reload

While the main window is open, G3MStudio watches `settings.json`. An edit made by another program is applied at once. See [Settings file](settings-file.md).

## See also

- [Default values](defaults-table.md)
- [Settings file](settings-file.md)
- [Command palette and Go to](../interface/command-palette-and-go-to.md)
- [Main window](../interface/main-window.md)
