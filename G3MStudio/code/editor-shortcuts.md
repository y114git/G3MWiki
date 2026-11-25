# Editor Shortcuts

This page lists every keyboard shortcut of the code editor with its default key, as defined in the command registry. Each command can be rebound or cleared in the keyboard shortcuts settings, and each one can be run from the command palette and from the **Code** menu.

On macOS the `Ctrl` of every default is `Cmd`. The code commands run on the code tab in front and are disabled while no code tab is in front.

## Code commands

Names are the menu labels. "Needs write access" means the command does nothing in a read-only tab.

| Command | Default key | Needs write access | What it does |
|---|---|---|---|
| Go to Definition | `F12` | no | Moves to the declaration of the name at the caret in this text, or opens the script or resource tab it names. `Ctrl+click` does the same. |
| Peek Definition | `Alt+F12` | no | Shows the definition in a popup without leaving the text. `Esc` closes it. |
| Parameter Hints | `Ctrl+Shift+Space` | no | Shows the signature of the call around the caret. |
| Quick Fix | `Ctrl+.` | yes | Opens the fixes for the problem at the caret. |
| Insert Snippet | `Ctrl+J` | yes | Opens the list of code templates. |
| Toggle Line Comment | `Ctrl+/` | yes | Adds or removes `//` on the selected lines. |
| Toggle Block Comment | `Shift+Alt+A` | yes | Wraps the selection or the caret line in `/* */`, or removes the marks. |
| Replace Numbers with Asset Names | none | yes | Replaces asset numbers with names in the selection or the whole entry. |
| Indent Selection | none | yes | Puts a tab in front of every non-empty line touched. |
| Outdent Selection | none | yes | Removes one tab or up to four spaces from each line touched. |
| Duplicate Line | `Shift+Alt+Down` | yes | Copies the lines below themselves. |
| Move Line Up | `Alt+Up` | yes | Swaps the lines with the line above. |
| Move Line Down | `Alt+Down` | yes | Swaps the lines with the line below. |
| Toggle Bookmark | `Ctrl+F2` | no | Adds or removes a bookmark on the caret line. |
| Next Bookmark | `Alt+F2` | no | Jumps to the next bookmark, wrapping round. |
| Previous Bookmark | `Alt+Shift+F2` | no | Jumps to the previous bookmark. |
| Next Problem | `F8` | no | Moves to the next problem of this entry, wrapping round. |
| Previous Problem | `Shift+F8` | no | Moves to the previous problem. |
| Go to Symbol in File | `Ctrl+Shift+O` | no | Opens the outline list. |
| Show Changes Against Original | none | no | Shows or hides the changes panel. |
| Help on Word Under Caret | `Ctrl+F1` | no | Shows the word in the function reference. |
| Open in Manual | none | no | Opens the GameMaker manual page of the word, or the manual index. |
| Go to Line... | `Ctrl+G` | no | Asks for `line` or `line:column`. |
| Go to Matching Bracket | `Ctrl+Shift+\` | no | Moves to the partner of the bracket at the caret. |
| Expand Selection | `Shift+Alt+Right` | no | Selects the next larger range. |
| Shrink Selection | `Shift+Alt+Left` | no | Goes back to the selection before the last expand. |
| Delete Line | `Ctrl+Shift+K` | yes | Removes the lines of the selection. |
| Join Lines | `Ctrl+Alt+J` | yes | Joins the lines into one. |
| Sort Selected Lines | none | yes | Sorts lines, or reverses them when already sorted. |
| Trim Trailing Whitespace | none | yes | Removes spaces and tabs at line ends (whole text when nothing is selected). |
| Convert to Upper Case | none | yes | Changes the selection or the word at the caret. |
| Convert to Lower Case | none | yes | As above. |
| Convert to Title Case | none | yes | As above. |

Details on what each line command does are in [Line Edits](line-edits.md).

## Search and navigation keys

| Command | Default key | Notes |
|---|---|---|
| Find | `Ctrl+F` | Opens the find box of the code tab in front. See [Find and Replace in Code](find-and-replace.md). |
| Find in Code... | `Ctrl+Shift+F` | Opens the Search panel for all code. |
| Replace in Code... | `Ctrl+Shift+H` | Opens the Search panel in replace mode. |
| Find Usages | `Shift+F12` | Finds the uses of the name or resource. See [Find Usages](find-usages.md). |
| Rename... | `F2` | In a code tab, renames the name under the caret ([Rename Symbol](rename-symbol.md)). Elsewhere it renames the selected resource. |
| Go to... | `Ctrl+T` | Opens the Go To window (**View** menu). See [Go To and Project Symbols](project-symbols.md). |
| Go to Resource... | `Ctrl+P` | Finds a resource by name (**View** menu). |
| Replace Numbers with Asset Names in All Code... | none | In the **Edit** menu. See [Replace Numbers with Asset Names](asset-numbers.md). |
| Save | `Ctrl+S` | In a code tab with pending changes, applies them. |
| Documentation | `F1` | Opens the documentation. |
| Run Game | `F5` | Runs the game. See [Run the game](../projects/run-game.md). |

## Keys inside the find box

| Key | Action |
|---|---|
| `F3` | Find next |
| `Shift+F3` | Find previous |
| `Alt+R` | Replace next |
| `Alt+A` | Replace all |

These are fixed and are not in the registry.

## Keys of completion and parameter walking

| Key | Action |
|---|---|
| `Ctrl+Space` | Opens the completion list, also with the automatic list switched off. |
| `Tab` | After inserting a function from the list: moves to the next parameter. |
| `Shift+Tab` | Moves to the previous parameter. |
| `Esc` | Ends the parameter walk, or closes a peek popup. |

Past the last parameter, `Tab` puts the caret after the closing bracket.

## Mouse

| Gesture | Action |
|---|---|
| `Ctrl+click` on a name | Go to Definition |
| `Ctrl+mouse wheel` | Changes the code font size |
| Click in the gutter | Toggles a bookmark |
| Right button | [Context menu](code-editor.md#context-menu) |

## Change a key

Open the keyboard shortcuts settings, find the command by its label, and press the new key. If two commands hold the same key, the one you chose yourself wins.

## See also

- [Code Editor](code-editor.md)
- [Line Edits](line-edits.md)
- [GML Intelligence](gml-intelligence.md)
