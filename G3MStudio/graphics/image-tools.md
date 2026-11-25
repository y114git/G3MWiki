# Image tools

The image editor has 17 tools in the **Toolbox** panel, plus brush, colour and selection helpers. This page lists every tool with its key and options, the colour panels, the modifier keys and every image editor shortcut.

The tool in use is a toggle in the **Toolbox** panel. Its hint shows under the canvas. Only the options that the tool reads appear on the options bar.

## Tools

| Tool | Default key | What it does |
|---|---|---|
| **Pencil** | `D`, also `B` and `.` | Click to paint. Drag to draw a stroke. `Shift` draws a straight line from where the last stroke ended. `Ctrl` picks a colour. The right button paints the second colour. |
| **Eraser** | `E` | Click or drag to erase to transparent. |
| **Replace colour** | `V` | Click a pixel: every pixel of that colour becomes the primary colour. Uses **Tolerance**. |
| **Remove colour** | `H` | Click a pixel: every pixel of that colour becomes transparent. Uses **Tolerance**. |
| **Fill** | `F` | Click to fill an area with the primary colour. Uses **Tolerance** and **Contiguous**. |
| **Line** | `L` | Drag to draw a line. |
| **Rectangle** | `R` | Drag. `Alt` draws from the centre, `Shift` keeps it square. |
| **Ellipse** | `C` | Drag. `Alt` draws from the centre, `Shift` keeps it a circle. |
| **Polygon** | `P` | Click the corners. `Enter` or a double click closes it. `Escape` cancels. |
| **Curve** | `A` | Click three or four control points. `Enter` or a double click draws it (a third click draws it at once when it has three points). |
| **Text** | `T` | Type the text in the options bar (200 characters at most), then click to place it in the primary colour. |
| **Colour picker** | `O`, also `I` | Click to pick the primary colour, right click for the secondary. It reads the flat result of all layers. |
| **Rectangle select** | `S` | Drag a rectangle. `Escape` clears the selection. |
| **Lasso select** | `Q` | Drag around the pixels to select. |
| **Magic wand** | `W` | Click to select similar pixels. Uses **Tolerance** and **Contiguous**. |
| **Move selection** | `M` | Drag to move the selection, or the whole picture without one. Arrow keys nudge by 1, `Shift` by 8. |
| **Hand** | none | Drag to move the view. The picture does not change. |

Filled variants have their own keys: **Rectangle (Filled)** `Shift+R`, **Ellipse (Filled)** `Shift+C`, **Polygon (Filled)** `Shift+P`.

With a selection tool, `Shift` adds the new shape to the selection and `Alt` cuts it out of the selection. All pixel tools work inside the selection only when there is one. A locked or hidden layer refuses the tools that change pixels.

### Options bar

| Option | Used by | Range and default |
|---|---|---|
| **Size** (box and slider) | Pencil, Eraser, Line, Rectangle, Ellipse, Polygon, Curve | 1 to 256. The slider goes to 64. Default from Settings, 1. |
| **Smooth** toggle | Same tools | Anti-aliased drawing. Default from Settings, off. |
| Style list: **Outline**, **Filled**, **Both** | Rectangle, Ellipse, Polygon | Outline uses the primary colour, fill the secondary. |
| **Outline colour**, **Fill colour** swatches | Same shapes | Show the two colours the style uses. |
| **Tolerance** | Fill, Replace colour, Remove colour, Magic wand | 0 to 255, default 0. |
| **Contiguous** | Fill, Magic wand | On by default. Off fills or selects all matching pixels in the picture. |
| **Text** and size | Text | Size 4 to 256, default 16. Font set in Settings. |

### Brushes, colours and mirrors

- **Brushes** panel (folded by default): ten presets, square 1, 2, 3, 5, 8 and round 3, 5, 8, 12, 16. A click sets shape and size.
- Brush shape is **Square** or **Round** (a Settings default).
- **Colours** panel: the **Primary colour** and **Secondary colour** swatches (for shapes **Outline colour** and **Fill colour**), **Swap**, a hex box (`#RRGGBB` or `#RRGGBBAA`), **Recent colours** (up to 16) and a palette of 32 colours. A click on a palette or recent chip sets the primary colour, a right click the secondary.
- **Mirror left-right** and **Mirror top-bottom** draw each stroke and shape mirrored around the picture centre.

## Pointer and view keys

| Input | Effect |
|---|---|
| Middle button, or `Space` with the left button | Pan the view with any tool. |
| `Ctrl` + wheel | Zoom around the pointer. |
| Wheel | Scroll. `Shift` + wheel scrolls sideways. A horizontal wheel or a touchpad swipe pans sideways. |
| `Ctrl` + click | Pick a colour with any tool except the picker. |
| Arrow keys | Nudge the selection (1 pixel, `Shift` 8). Without a selection they scroll the view (32 pixels, `Shift` 128). |
| `Enter` | Finish a polygon or curve. |
| `Escape` | Cancel a polygon, curve or stroke in progress. A pencil stroke is kept up to that point. Also ends a canvas handle drag. |

## All image shortcuts

These commands run only while an image editor tab is in front. Every one can be rebound in Settings. Keys marked `-` have none by default. `Ctrl` is `Cmd` on macOS.

| Command | Key |
|---|---|
| Bigger Brush | `+` (also keypad `+`) |
| Smaller Brush | `-` (also keypad `-`) |
| Swap Primary and Secondary Colour | `X` |
| Smooth Drawing | - |
| Grid | `G` |
| Mirror Left-Right, Mirror Top-Bottom, Tile Preview, Onion Skin | - |
| Zoom In, Zoom Out | - |
| Fit | `Alt+F` |
| Actual Size (1:1) | `Alt+C` |
| Previous (frame) | `1` |
| Next (frame) | `2` |
| Duplicate Frame | `Ctrl+D` |
| Delete Frame, Play | - |
| Select All | `Ctrl+Shift+A` (also `Ctrl+A` in the editor) |
| Select None | `Escape` |
| Invert Selection | `Ctrl+I` |
| Copy, Cut, Paste | `Ctrl+C`, `Ctrl+X`, `Ctrl+V` |
| Clear | `Delete` |
| New Layer | `Ctrl+Shift+N` |
| Undo, Redo | `Ctrl+Z`, `Ctrl+Y` (redo also `Ctrl+Shift+Z`) |
| Apply | `Ctrl+S` (the Save key, while edits wait) |
| Rename layer | `F2` (renames the layer here, not the sprite) |
| All other commands (transforms, adjustments, layer commands, **Discard Image Edits**, **Go to Original Resource**) | - |

The tool keys are in the table above. Keys listed as 1 and 2 are the number row keys. The editor shows the current key in the tooltip of each button. A key does nothing while a text box or number box has the focus.

## See also

- [Image editor](image-editor.md)
- [Image menus](image-menus.md)
- [Image settings](image-settings.md)
