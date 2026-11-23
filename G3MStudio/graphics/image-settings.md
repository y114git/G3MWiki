# Image settings

Several Settings controls change how the image editor, sprites, tile sets and the picture comparison behave. This page lists the **Image editor**, **Auto-tile** and **Data file comparison** controls with defaults and ranges. The settings window itself is described in the settings pages.

Open **Settings** (command `settings.open`) and choose the section. Changes apply to editors opened afterwards. Colour boxes take `#RRGGBB` or `#RRGGBBAA`. An empty or invalid text keeps the theme colour.

## Image editor section

| Label | Default | Range | Effect |
|---|---|---|---|
| **Default brush size** | 1 | 1 to 256 | Starting brush size. |
| **Round brush by default** | off | | Starting brush shape. |
| **Smooth drawing by default** | off | | Starting state of **Smooth**. |
| **Default tolerance** | 0 | 0 to 255 | Tolerance of fill, replace, remove and wand. |
| **Contiguous fill by default** | on | | Starting state of **Contiguous**. |
| **Text size and font** | 16, interface font | 4 to 256 | Size and font family of the Text tool. Empty font uses the interface font. |
| **Apply image edits immediately** | off | | Off: edits wait in the tab until **Apply**. On: every edit of a sprite goes straight into the workspace history. |
| **Save layers to disk after every edit** | on | | On: layer stacks are written shortly after each edit. Off: when the file closes. |
| **Show the grid** | on | | Starting state of **Grid**. |
| **Grid cell size and colour (#RRGGBB, empty for the theme)** | 16, theme | 0 to 1024 | Pixels between the heavier grid lines. 0 turns them off. |
| **Onion skin frames and opacity (percent)** | 1 and 35 | 1 to 8, 5 to 100 | Frames before and after, and how faint. |
| **Checkerboard colours (light, dark)** | theme | | The chequer behind transparent pixels. |
| **Canvas background colour** | theme | | The colour around the picture. |
| **Handle size (pixels)** | 3 | 1 to 12 | Size of the control point markers of polygons, curves and the lasso. |

Not shown in Settings but saved with it (set by using the Resize dialogs): the choices of the Resize dialog (mode, method, keep aspect, unit, anchor, fill, box scaling, percents), the all-frames key (`Alt`), the Advanced page options (grow on, new page on, largest page size auto), and whether Import Sheet cuts by size. See [Resize and fit](resize-and-fit.md).

## Auto-tile section

| Label | Default | Effect |
|---|---|---|
| **Treat the layer edge as terrain** | off | Terrain painted at the edge continues past it. |
| **Layout of a new rule set (0 = 47 tiles, 1 = 16 tiles, 2 = 256 masks)** | 0 | Default layout of **New**. |
| **Choose the first rule set of a tile set when its layer is selected** | off | The brush starts with the first rule set. |

See [Auto tiles](auto-tiles.md).

## Data file comparison section

These affect how pictures and code show in Compare Data Files.

| Label | Default | Effect |
|---|---|---|
| **Ignore white space in code differences** | off | Whitespace-only changes are not differences. |
| **Show code differences as unified text** | off | One merged text instead of side by side. |
| **Context lines in unified text** | 3 | 0 to 50. |
| **Picture mode** | **Overlay** | The comparison of two pictures: **Overlay**, **Swipe** or **Difference**. The Picture tab then shows `{count} pixels differ.` or `There is no picture to compare.` and has a **Frame** box. |

## Shortcuts

The image tool keys are commands and can be rebound in the shortcuts editor. See [Image tools](image-tools.md) for the list.

## See also

- [Image editor](image-editor.md)
- [Image tools](image-tools.md)
- [Settings pages](../settings/README.md)
