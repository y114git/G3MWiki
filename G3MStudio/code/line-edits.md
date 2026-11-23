# Line Edits

The code editor has commands that change whole lines, comment code, or change letter case. Each command is one change in the text, so one editor undo takes it back. This page describes what each one does. Keys are in [Editor Shortcuts](editor-shortcuts.md).

All commands on this page do nothing in a read-only tab. Where a command works on "the lines of the selection", a selection that ends at the very start of a line does not include that line. With no selection, the caret line is used.

## Comments

| Command | Result |
|---|---|
| **Toggle Line Comment** | Puts `// ` in front of every non-empty line, at the smallest indent of the block. If every non-empty line already starts with `//`, removes the marks (and one space after them). Empty lines stay empty. |
| **Toggle Block Comment** | Wraps the selection in `/*` and `*/`. With no selection, wraps the caret line without its line break. If the selection already starts with `/*` and ends with `*/`, removes them. |

## Indent

| Command | Result |
|---|---|
| **Indent Selection** (**Tab In** in the context menu) | Puts one tab character in front of every non-empty line. |
| **Outdent Selection** (**Tab Out**) | Removes one tab, or up to four spaces, from the front of every line. |

The **Tab width** option (2 to 8, default 4) sets how wide a tab looks and how wide an indent step is in the editor.

## Move, copy and delete lines

| Command | Result |
|---|---|
| **Duplicate Line** | Copies the lines below themselves and moves the selection to the copy. |
| **Move Line Up** / **Move Line Down** | Swaps the lines with the neighbour. Does nothing at the first or last line. |
| **Delete Line** | Removes the lines with their line break. On the last line, the break before it goes instead. |
| **Join Lines** | Joins the selected lines into one, with one space in place of each break and the indentation after it. With no selection, joins the caret line with the next. Does nothing for a single line. |
| **Sort Selected Lines** | Sorts the lines, ignoring case. Lines that are already in that order are reversed instead. Needs at least two lines. |
| **Trim Trailing Whitespace** | Removes spaces and tabs at the end of each selected line, or of every line when nothing is selected. Each line is its own small change, so the caret keeps its place. |

Line endings in the text (`\n` or `\r\n`) are kept: new line breaks use the style the text already uses.

## Change case

| Command | Result |
|---|---|
| **Convert to Upper Case** | `Name` becomes `NAME`. |
| **Convert to Lower Case** | `Name` becomes `name`. |
| **Convert to Title Case** | Each word starts with a capital, the rest is lower case. A word is letters, digits and underscores. |

With no selection, the word at the caret is changed. Nothing happens if the text would stay the same.

## Selection and brackets

| Command | Result |
|---|---|
| **Expand Selection** | Selects the next larger range around the selection: the word, the inside of the brackets around it, the brackets with their content, the line, then the whole text. |
| **Shrink Selection** | Goes back to the selection before the last expand. The history is dropped when you edit or select something yourself. |
| **Go to Matching Bracket** | Moves to the partner of the bracket at the caret. From an opening bracket the caret lands after the closing one, from a closing bracket before the opening one. |
| **Go to Line...** | Asks for "Line number, or line:column (1 to {max})". A bad entry shows "Enter a line from 1 to {max}." |

## See also

- [Code Editor](code-editor.md)
- [Editor Shortcuts](editor-shortcuts.md)
