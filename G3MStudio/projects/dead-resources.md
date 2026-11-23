# Find Dead Resources

**Organize > Find Dead Resources...** lists resources that nothing in the data file refers to and deletes the safe ones in one undoable step. Use it to shrink a project before sharing it.

## Open the window

Choose **Organize > Find Dead Resources...** The window **Dead Resources** scans the open file ("Scanning...") and says: "Resources that nothing in the file refers to. Uses that only exist as numbers computed at run time cannot be seen, so check the list before you delete."

The summary reads "{safe} safe to delete (about {size}), {maybe} maybe used." With nothing found: "No dead resources found."

## The two lists

| List | Meaning |
|---|---|
| **Safe to delete (uncheck to keep)** | Nothing uses the resource and nothing hints at it. Grouped by type: "{type} ({count}, {size})". Items are checked; uncheck the ones to keep. |
| **Maybe used (never deleted here)** | Nothing uses the resource directly, but a hint says it may still be needed. |

Reasons a resource is only "maybe used":

| Reason shown | Meaning |
|---|---|
| its name is a string in code | Code may look the resource up by name. |
| a string in code starts its name | Code may build the name (`"spr_" + name`). A text of one or two characters counts only when code joins more text to it. |
| global init script | The script runs at start. |
| may be the default font | The first font. |
| the game's code (before GameMaker 2023.8) refers to assets by number | Older bytecode turns asset names into plain numbers that cannot be told apart from other numbers. Every dead resource of such a file is "maybe used". |
| has a nickname, has a tag, is a favourite | You marked it. |

## Buttons

| Button | Effect |
|---|---|
| **Scan again** | Scans again. |
| **Delete all safe** | Deletes the checked safe resources. |

Before deleting, the dialog **Delete {count} dead resources** asks "Delete {count} resources that nothing uses?" with the counts per type and "One Undo brings them back."

The scan runs again before the delete. A resource that is no longer certainly unused is skipped ("no longer certainly unused"), as is one of a type that cannot be deleted here ("cannot be deleted here"). Result: "Deleted {count} dead resources." and, for skipped ones, "Skipped {count}:" with a list. With nothing safe: "Nothing was deleted."

## Limits

- Uses that only exist as numbers computed at run time cannot be seen.
- Only asset types are scanned.

## See also

- [Edit history and undo](history.md)
- [Export packages](packages.md)
- [Projects and Project mode](overview.md)
