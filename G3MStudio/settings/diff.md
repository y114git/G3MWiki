# Data file comparison settings

The **Data file comparison** category sets the starting view of the comparison of two data files. The values are read when the comparison window opens. The added, removed and changed colors are on [Colors and themes](colors-and-themes.md).

## Controls

| Label | Control | Default | Allowed values | Saved as |
| --- | --- | --- | --- | --- |
| **Ignore white space in code differences** | Checkbox | Off | On, off | `diff.ignoreWhitespace`. Lines that differ only in white space count as equal. |
| **Show code differences as unified text** | Checkbox | Off | On, off | `diff.unified`. Off: side by side. |
| **Context lines in unified text** | Number | 3 | 0 to 50 | `diff.contextLines`. Unchanged lines kept around a change. |
| **Picture mode** | List | **Overlay** | **Overlay**, **Swipe**, **Difference** | `diff.pictureMode` (`overlay`, `swipe`, `difference`). The view when a picture item opens. |

An unknown picture mode in the file falls back to `overlay`.

## Restore defaults

Resets the whole `diff` object.

## See also

- [Code diff](../code/code-diff.md)
- [Mods](../projects/mods.md)
- [Default values](defaults-table.md)
