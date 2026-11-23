# Folders, collections and tags

G3MStudio lets you group Explorer entries in folders, collections, tags and favourites. This page explains each one, how to create and fill them, and how drag and drop and the breadcrumb work. None of this is written to the data file: it is kept in G3MStudio's own view state for that file, so the game never sees it.

## Quick Access

The **Quick Access** block at the top of the tree holds:

| Heading | Holds |
|---|---|
| **Recent** | The last 20 entries you opened, newest first. |
| **Favourites** | Entries you marked with **Add to or Remove from Favourites**. |
| **Saved Searches** | Filter texts you saved. See [Explorer filter and sorting](explorer-filter.md). |
| **Tags** | One row per tag, with the entries that carry it. |
| **Collections** | Your collections. |
| **Folders** | Your folders (in the type view). |

An entry that no longer exists is skipped in these lists. Deleting a resource also removes it from folders, collections, tags and favourites, and undo brings the memberships back.

## Folders

A folder is a named container you create in the Explorer. Folders can hold other folders. A folder does not change the data file or the order of the resources.

**Create a folder**

1. Select entries (optional) and run **Organize > New Folder...** (or right-click the **Folders** heading, or a folder, for **New Subfolder...**).
2. Enter a **Folder name** in the **New folder** window and press **Create**.
3. The selected entries move into the new folder.

A name must not be empty ("The name is empty.") and must not repeat a name in the same parent ("A folder with this name already exists here.").

**Fill a folder**

| Way | Result |
|---|---|
| **Organize > Move to Folder...** | Asks "Move {count} entries to:" with the choices **(top level)**, every folder by path, and **New folder...**. |
| **Organize > Add to Another Folder...** | Adds the entry to a second folder and keeps the first. |
| `Ctrl+X`, then `Ctrl+V` on a folder | Cut and paste. The notification reads "Cut {count} entries. Paste them into a folder with Ctrl+V." Paste works on a folder, an entry inside a folder, or the **Folders** heading (top level). |
| Drag entries onto a folder | Moves them. Hold `Ctrl` while dropping to add them to the folder and keep their other folders. |
| **Organize > Remove from Folder, Collection or Tag** | Takes the entry out of the folder, collection or tag it is listed under. |

One entry can sit in several folders. A folder holds a reference, not a copy: it is still one resource and a change shows in every folder. When an action makes an entry sit in a second folder the prompt "Resource in several folders" explains this and lets you stop.

**Rename, colour and delete a folder.** Right-click the folder row: **Rename...**, **Set Colour...**, **Delete Folder (keep the entries)**. Colours: No colour, Red, Orange, Yellow, Green, Teal, Blue, Purple, Pink. The colour shows as a dot before the row.

## Folder view and the type view

The **Types** or **Folders** button in the Explorer title row (tooltip "Switch between the type view and the folder view") and **Organize > Switch Folder View** swap the layout.

- In the **type view** the **Folders** heading sits in **Quick Access**. Under the type headings, only entries that no folder holds are listed.
- In the **folder view** the folders are the main tree, then the types list what no folder holds.
- A file remembers its own choice. A new file starts with the setting **Show the Explorer as folders instead of resource types**.
- Folders start open unless **Open folders and collections when they appear** is off.

## Collections

A collection is a flat, named set of entries. Use it for a working list, such as everything in one level.

1. Select entries and run **Organize > Add to Collection...**.
2. Pick a collection or **New collection...**. A new one asks for a **Collection name** in the **New collection** window.
3. The notification reads "Added {count} entries to {name}."

Drag entries onto a collection row to add them. Right-click a collection for **Open Folder**, **Rename...**, **Set Colour...** and **Delete Collection**. Deleting asks "Delete the collection '{name}'?" and explains: "The entries stay in the data file; only the collection is removed."

## Tags

A tag is a label on an entry. Select entries or have a tab in front, and run **Edit > Add Tag...** (command **Add Tag...**). Enter the **Tag name** in the **Add tag** window and press **Add**. An empty name shows "Type a tag name."

- Tags are listed under **Quick Access > Tags**, each with its entries.
- Drag entries onto a tag row to tag them.
- **Set Colour...** on a tag, or with an entry selected (it then asks which tag), colours it.
- The **Tag** list in the filter popup narrows the whole Explorer to one tag. See [Explorer filter and sorting](explorer-filter.md).
- **Remove from Folder, Collection or Tag** removes a tag from the entry when it is listed under that tag.

## Favourites

Select an entry, or have its tab in front, and run **Add to or Remove from Favourites**. The entry joins or leaves **Quick Access > Favourites**.

## Narrowing the list to one container

Double-click a folder, collection or Recent row, press `Ctrl+Enter` on it, or choose **Open Folder** from the menu (**Organize > Open Folder** or its context menu). The Explorer then lists only that container's entries and shows a breadcrumb above the list:

```text
All > Folder > Subfolder
```

Click a part to go there. **All** returns to the whole tree. `Backspace` or **Organize > Up One Folder** goes one level up. A saved search can be opened the same way.

## Selecting by container

| Command | Selects |
|---|---|
| **Organize > Select All of This Type** | Every entry of the type of the selected entry. |
| **Organize > Select Everything in the Group** | Every entry in the selected folder, collection, tag or type, or in the folder that holds the selected entry. |

## Bulk commands

With several entries selected, or on a folder, collection or tag row, the Organize menu and the right-click menu act on all of them. The commands:

- **Rename with Pattern...**: see below.
- **Move to Texture Group...** (sprites, fonts and tile sets) and **Move to Audio Group...** (sounds): choose a group and the entries move to it. With no fitting entries selected the messages are "No sprites, fonts or tile sets are selected." and "No sounds are selected."
- **Find Dead Resources...**, **Export as Package...** and the export and copy commands from the [Transfer window](../projects/README.md).

### Rename with Pattern

**Organize > Rename with Pattern...** opens **Rename {count} entries** with a mode, the text to use and a preview of the names before and after.

| Mode | Effect |
|---|---|
| **Add at the start** | Puts the text in front of every name. |
| **Add at the end** | Puts the text after every name. |
| **Replace text** | Replaces **Find** with **Text**. |
| **Regular expression** | Replaces by pattern. |

The summary reads "{count} will be renamed, {problems} cannot be, {same} stay the same." A name that cannot be used is listed with its reason: "The pattern is not valid.", "The new name is empty.", "The new name has spaces or control characters.", "Two entries get this name." or "The name is already used." Press **Rename** to apply. With nothing to change the message is "No name can be changed." The result is one undo step.

## See also

- [Explorer](explorer.md)
- [Explorer context menus](explorer-context-menus.md)
- [Explorer filter and sorting](explorer-filter.md)
