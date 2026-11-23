# Guide: draw on a sprite

This guide takes you from an open sprite to an edited, saved frame. It uses the image editor and the default settings. The result is a changed picture in the data file that you can undo.

1. Open a sprite from **Sprites** in the Explorer. Click the frame to edit in the frame strip.
2. Press **Edit Image...**. The image editor opens in a new tab on that frame.
3. Choose the **Pencil** (`D`) and pick a colour in the **Colours** panel. Draw on the canvas. Use `Ctrl` + wheel to zoom.
4. To change the canvas size, drag an edge or corner of the picture. Hold `Shift` to keep the aspect.
5. Press **Apply**. The tab no longer says **Not applied yet** and the sprite tab shows the change.
6. Press `Ctrl+S` (**Save**) to write the data file.

To undo after Apply, use **Undo** in the main window (`Ctrl+Z`).

## Expected result

The frame in the strip shows the new picture. If the new size differs, the message `Applied the picture of {name}.` appears and the texture pages hold the new picture.

## Other tasks

- Add a frame: **Add frame** in the Frames column of the editor, or **Add** in the sprite tab.
- Make a copy: arrow beside **Apply**, **New sprite from this frame...**.
- Import a sheet: [Sprite frames and import](sprite-frames.md).
- Set the origin and collision mask: [Sprite editor](sprite-editor.md).

## See also

- [Image editor](image-editor.md)
- [Image tools](image-tools.md)
