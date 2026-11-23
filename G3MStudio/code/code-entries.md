# Code Entries

A code entry is one named piece of compiled code in a data file. This page explains the entry names GameMaker uses, what each GameMaker item maps to, the child entries the compiler makes for functions inside other entries, and the assembly view.

## Entry names

The Explorer lists code entries under **Code**. Each name starts with a prefix that tells what the code belongs to:

| Name pattern | Belongs to |
|---|---|
| `gml_Script_name` | A script function (GameMaker 2.3 and later). A script resource opens its code. |
| `gml_GlobalScript_name` | The global script of a script resource. Its functions run once when the game starts. |
| `gml_Object_obj_name_Event_n` | One event of an object. The event word is `Create`, `Destroy`, `Alarm`, `Step`, `Collision`, `Keyboard`, `KeyPress`, `KeyRelease`, `Mouse`, `Other`, `Draw`, `Gesture`, `PreCreate`, `CleanUp` or `Async`, and `n` is the sub-event number. |
| `gml_Room_room_name_Create` | The creation code of a room. |
| `gml_RoomCC_room_name_id_Create` | The creation code of one instance in a room (`id` is the instance id). |
| `gml_Timeline_name_step` | The code of one moment of a timeline. |

Open any entry with a double-click in the Explorer, `Ctrl+P`, or from the editor of the item that owns it (an object's event list, a room's **Creation Code** command, and so on). See [Code Editor](code-editor.md).

Behavior that depends on the kind of entry:

- The code check treats entries other than `gml_Script_` and `gml_GlobalScript_` as instance code. A variable read in an object event can be set by another event, so the "read before it is set" hint is limited to scripts (see [Code Checks and Lint Rules](lint-rules.md)).
- Completion after a dot in `gml_Object_..._Event_n` code lists the variables of that object, because the object that owns the entry is known from the name.
- An entry takes the nickname of its owner in the Explorer, search results, usages and problems.
- Locking an object, script or room in G3MStudio locks its entries too, and those code tabs are read-only.

### Import code from files

**Import Code from a Folder...** and **Import Code from Files...** (in the File menu's transfer commands, group "Into this data file") read `.gml` and `.asm` files. The file name without its extension is the name of the code entry the file replaces. A folder gives every matching file in it and its sub folders.

1. Choose the folder ("Folder with GML and assembly files") or the files ("Pick GML or assembly files").
2. The preview asks "Replace the code of {count} entries from files?" and lists the names.
3. A file that names no entry: you are asked whether to add an empty entry for each one and fill it from the file ("Yes adds an empty entry for each one and fills it from the file. No skips these files."). The new name must use letters, digits and underscores and not start with a digit.
4. A file that cannot be imported (over 8 MB, binary, unreadable, or the same entry name twice as `.gml` and `.asm`) is listed. Yes imports the others, No imports nothing.
5. Result: "Code replaced in {count} entries. Undo takes it back in one step." Nothing is written when one entry fails to compile or assemble.

## Child entries

Since GameMaker 2.3, a function declared inside another entry gets its own code entry. These entries have no GML text of their own. Only the parent decompiles, so the tab for a child shows the assembly of the function under a banner.

| Kind | Name pattern |
|---|---|
| Anonymous function | `gml_Script_anon_...` |
| Struct constructor | `gml_Script____struct___N_...` (made for struct literals) |
| Other inner function | Any other entry that has a parent entry (a method or constructor declared inside a script or event). |

The Explorer shows each kind as shown, grayed out or hidden. The defaults are grayed out. Change them in **Settings > Appearance** with **Anonymous functions in code**, **Struct constructors in code** and **Other functions inside code entries**. A script resource stands for its code. When the script is a global script, the `gml_Script_name` entry the compiler makes for its function is listed as a child of the `gml_GlobalScript_name` entry, next to the other child entries.

### The banner of a child entry

The tab shows one of these texts, with the parent's name:

- "This is a function inside {parent}. Only the parent decompiles to GML, so its assembly is shown here. Edit the function in the parent."
- "This is an anonymous function inside {parent}. Only the parent decompiles to GML, so its assembly is shown here. Edit the function in the parent."

Buttons next to the text:

| Button | What it does |
|---|---|
| **Go to Parent** | Opens the parent entry in its own tab. Edits and saves belong to the parent. |
| **Details** / **Hide Details** | Shows or hides a line "Name: {name}" with the name of the entry. |

A child entry has no **Switch to GML** button, because there is no GML to switch to. Saving the assembly of a child assembles the text instead of compiling GML.

## Assembly view

The assembly view shows the VM assembly of the entry, one instruction per line, with variable names and local names resolved. Use it to see exactly what the game runs when the decompiler gives up or when you need an exact instruction.

Reach it in one of three ways:

- **Switch to assembly** in the button bar of the tab (it becomes **Switch to GML**).
- **Tools > Disassembly of Code Entry...**, which switches the tab in front, or the selected code entry, between GML and assembly.
- Automatically, when decompiling fails. The banner then reads "Decompiler failed: {reason}. Showing assembly." with **Retry** and, for entries inside another entry, **Go to Parent**.

Switching views saves pending changes first. Saving in the assembly view assembles the text back into the entry. The live check and lint do not run in the assembly view, and an error is shown in the message area. The change view compares against the first text of the view you are in (see [Compare With Original](code-diff.md)).

## See also

- [Code Editor](code-editor.md)
- [GML Language Support](gml-language.md)
- [Debug Data](debug-data.md) and [Hex View and Raw Chunks](hex-view.md)
- [Explorer settings](../settings/)
