# Image menus

The image editor options bar has four drop-down menus, and the main menu bar has an **Image** menu with the same commands in groups. This page lists every command with its parameters.

Dialogs that ask for numbers have **OK** and **Cancel**. Commands work on the selection when there is one, otherwise on the whole picture. A locked or hidden layer refuses them.

## Selection

| Command | Meaning |
|---|---|
| **Select all** | Selects the whole picture. |
| **Select none** | Clears the selection. |
| **Invert selection** | Selects what is not selected. |
| **Expand or shrink selection...** | **Pixels**, default 1, range -64 to 64. A negative value shrinks. |
| **Copy**, **Cut**, **Paste** | See [Image editor](image-editor.md). |
| **Clear** | Makes the selected pixels transparent. |
| **Fill selection** | Fills the selection with the primary colour. |
| **Stroke selection...** | Outlines the selection with the primary colour. **Pixels** 1 to 64, default 1. |

## Transform

| Command | Parameters |
|---|---|
| **Flip horizontally**, **Flip vertically** | None. |
| **Rotate 90 degrees clockwise**, **Rotate 90 degrees anticlockwise** | None. |
| **Rotate by angle...** | **Angle** -360 to 360, default 45. |
| **Resize image...** | The [Resize dialog](resize-and-fit.md). The plain form has **Width** and **Height**, 1 to 4096. |
| **Skew...** | **Angle** -80 to 80, default 20. A skew that would make the picture wider than 4096 pixels is refused with `The skew would make the picture wider than 4096 pixels.` |

## Adjust

| Command | Parameters |
|---|---|
| **Hue, saturation, brightness, contrast...** | **Hue** -180 to 180, **Saturation**, **Brightness**, **Contrast** each -100 to 100, all default 0. |
| **Invert colours** | None. |
| **Posterize...** | **Levels** 2 to 256, default 4. |
| **Greyscale...** | **Amount** 0 to 100 percent, default 100. |
| **Dither...** | **Levels** 2 to 16, default 4. |
| **Outline with primary colour** | Draws an outline around the opaque pixels in the primary colour. |
| **Blur...** | **Radius** 1 to 8, default 1. |

## Canvas

| Command | Meaning |
|---|---|
| **Resize canvas...** | The [Resize dialog](resize-and-fit.md). The plain form has **Width**, **Height**, **Anchor X (0 to 2)** and **Anchor Y (0 to 2)**. |
| **Crop to selection** | Makes the canvas the selection bounds. |
| **Trim to content** | Cuts away transparent borders. |

## Which menu is marked

The menu whose command ran last shows that item checked. The **Selection** menu is also marked while a selection tool is in use. Choosing a tool clears the mark.

## Main menu

The **Image** menu appears while an image tab is in front. It groups the commands in submenus **Tools**, **Selection**, **Transform**, **Adjust**, **Canvas**, **Brush and colour**, **View** and **Frames**, **Layers**. Its entries are the commands in [Image tools](image-tools.md), plus **Discard Image Edits** and **Go to Original Resource**.

## See also

- [Image editor](image-editor.md)
- [Image tools](image-tools.md)
- [Resize and fit](resize-and-fit.md)
