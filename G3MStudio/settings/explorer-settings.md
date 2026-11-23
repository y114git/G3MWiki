# Explorer settings

These options set how the Explorer lists the resources of a data file when nothing else has been chosen. They sit at the end of the **Appearance** category, under the font and display options.

A data file that already has its own Explorer state (view, sort, filter) keeps it. The settings here apply to files without a saved state and to the defaults of a new view.

## Controls

| Label | Control | Default | Allowed values | What it does |
| --- | --- | --- | --- | --- |
| **Show the Explorer as folders instead of resource types** | Checkbox | Off | On, off | The Explorer shows your own folders as its main tree instead of grouping by resource type. A file can choose its own view. Saved as `explorerFolderView`. |
| **Show counts in the Explorer** | Checkbox | On | On, off | Shows how many items each group holds. Saved as `explorerShowCounts`. |
| **Open folders and collections when they appear** | Checkbox | On | On, off | Folders and collections start expanded. Saved as `explorerFoldersOpen`. |
| **Hide code that cannot be decompiled (and scripts using it)** | Checkbox | Off | On, off | Leaves out code that has no text of its own: child entries and code the decompiler fails on. The check runs in the background and the list is rebuilt as failures turn up. Saved as `explorerHideUndecompilable`. |
| **Hide broken resources and entries with errors** | Checkbox | Off | On, off | Leaves out resources that point at nothing, for example a sprite frame without its picture. Saved as `explorerHideBroken`. |
| **Open Strings, Variables and the other data lists when I search** | Checkbox | Off | On, off | While you type in the Explorer filter, the **Data** group and **Code** always open to show their matches. With this on, the other data lists (Strings, Variables, Functions and the rest) open too. Off, they keep the fold you gave them. Saved as `explorerOpenDataOnSearch`. |
| **Leave out the data lists (Strings, Variables, Functions and so on)** | Checkbox | Off | On, off | Removes the data lists from the Explorer. The code entries stay. Saved as `explorerHideData`. |
| **Anonymous functions in code** | List | **Gray out** | **Show**, **Gray out**, **Hide** | How the Explorer lists `gml_Script_anon_...` entries. They only lead to the entry they are part of. Saved as `explorerAnonymousFunctions` (`Show`, `Dim`, `Hide`). |
| **Struct constructors in code** | List | **Gray out** | **Show**, **Gray out**, **Hide** | How the Explorer lists the constructors of struct literals (`gml_Script____struct___N_...`). Saved as `explorerStructConstructors`. |
| **Other functions inside code entries** | List | **Gray out** | **Show**, **Gray out**, **Hide** | How the Explorer lists other named functions defined inside a code entry. Saved as `explorerInnerFunctions`. |
| **Default sort order of the Explorer** | List | **ID** | **ID**, **A to Z**, **Z to A** | Sort order for a data file without a saved Explorer state. Saved as `explorerSortOrder` (`ListOrder`, `NameAscending`, `NameDescending`). |
| **Size of the Explorer icons at 100% zoom** | Number | 22 px | 16 to 96, step 2 | Size of the picture in a row at 100 percent zoom. The zoom slider under the list scales it together with the text. Saved as `explorerThumbnailSize`. |
| **Sprite previews start at** | List | **First visible frame** | **First visible frame**, **First frame** | Which frame a still picture of a sprite shows: Explorer rows, hover cards, Go To, search results, pickers and the object preview. **First visible frame** skips frames that are fully transparent, so a sprite with an empty first frame can still be recognised; only the first 64 frames are checked, and a sprite with none visible shows frame 0. **First frame** always shows frame 0. The Play button still plays every frame, and the room editor still draws the frame the instance uses. Saved as `spritePreviewFirstVisible`. |

The last two options of **Appearance**, **Show previews and IDs on hover** and **Show null entries in the resource tree**, also act on the Explorer. They are listed in [Appearance settings](appearance.md).

## Entry visibility

The three entry rows share one list:

- **Show** lists the entry like any other.
- **Gray out** lists it in a dimmed row.
- **Hide** removes it from the list.

The same three choices appear in the Explorer filter, and both places write the same setting. See [Explorer filter](../interface/explorer-filter.md).

## Remembered zoom

The zoom slider under the Explorer list (also `Ctrl` and the wheel over the list) is saved as `explorerZoom`, 50 to 250 percent, default 100. The zoom of the sprite picture view is saved as `spriteZoom`. Both load at the next start. Neither has a control on the Settings page and **Restore defaults** does not reset them.

## Folded options bar

The bar under the Explorer list (count, thumbnails, zoom) can be folded to a strip. That state is saved as `explorerOptionsFolded` and has no control on the Settings page.

## Restore defaults

**Restore defaults** in **Appearance** resets every Explorer option on this page except the icon size: the folder view, the counts, **Open folders and collections when they appear**, the four hide and search checkboxes, the three entry visibility rows, the default sort order and **Sprite previews start at**.

## See also

- [Explorer](../interface/explorer.md)
- [Explorer filter](../interface/explorer-filter.md)
- [Appearance settings](appearance.md)
- [Default values](defaults-table.md)
