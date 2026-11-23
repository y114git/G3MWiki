# Image editor

The image editor is a paint tab for the pictures inside a data file: sprite frames, tile set pictures, font sheets, texture pages and more. This page covers what it can open, the window layout, Apply and Discard, frames, layers, view options and undo. The tools are on [Image tools](image-tools.md) and the Selection, Transform, Adjust and Canvas menus are on [Image menus](image-menus.md).

## What it opens

Open it with **Edit Image...** (command `edit.editImage`) from a sprite tab, the Resource menu, the context menu of a resource, or the **Edit Image...** button of a font, tile set, texture page item, embedded image or texture page view. With nothing that has a picture in front, it says `This tab has no picture to edit.`

| Resource | What you edit | Size rule |
|---|---|---|
| Sprite | One frame at a time, with a frame column. | Any size. A frame of another size finds its place on the texture pages. |
| Tile set, texture page item, embedded image | The one picture of the item. | Any size, with the same fit on the pages. A tile set warns when the tile grid would move. |
| Font | The font sheet. | Glyph rectangles follow a canvas change on the top or left side. Scaling, rotating or skewing the whole sheet is refused. |
| Font glyph (**Edit glyph image**) | One glyph rectangle of the sheet. | Fixed size. The status line says `Glyph of {w}x{h} on the font sheet; the size is fixed and Apply writes the sheet`. |
| Texture page | The whole page. | Fixed size: `Texture page of {w}x{h}; the size cannot change`. |
| Collision mask (**Edit mask image...**) | A black and white picture, white is solid. | Fixed size: `Collision mask of {w}x{h}, 1 bit: white is solid, the size is fixed`. |
| Included picture (png, jpg, jpeg, bmp, gif) | A loose file next to the data file. | `Loose file: Apply writes it to the game folder at once (JPEG has no transparency, a GIF keeps its first frame)`. |

## Window layout

- Top: the options bar. It holds the buttons **Go to the original resource** (when the picture belongs to a resource with its own tab), the menus **Selection**, **Transform**, **Adjust** and **Canvas**, undo and redo, **Apply** with its arrow, **Discard**, zoom buttons, the options of the current tool, and the toggles **Smooth**, **Mirror left-right**, **Mirror top-bottom**, **Grid**, **Tile preview** and **Onion skin**. A narrow window folds what does not fit into a "..." drop-down. The bar has at most two rows, one row in a window under 360 pixels high.
- Centre: the canvas.
- Left column: **Frames** (shown when the window is 560 pixels wide or more).
- Right column: the panels **Brushes**, **Colours**, **Toolbox** and **Layers**. Each folds under its header. Both columns have a splitter. At under 320 pixels wide the right column moves into the "..." drop-down.
- Bottom: the pointer position `(x, y)`, the status line `{w} x {h}   Frame {n}/{count}   {zoom}   Selection {w}x{h}`, **Not applied yet** while edits wait, the **All frames** toggle, and the hint of the tool in use.

## Apply, Discard and when edits reach the data file

By default the editor holds your edits in the tab with their own undo and redo. The data file, the sprite tab, the Explorer and autosave see them only when you press **Apply**, all together as one undoable step. **Not applied yet** shows meanwhile and the tab is marked as modified.

| Control | Meaning |
|---|---|
| **Apply** | Writes the edits. Disabled with `No changes to apply yet. Draw something first.` when nothing is pending. For a picture target the shortcut is `Ctrl+S` (the Save key). |
| **Discard** (`img.discard`, no default key) | Drops the edits since the editor opened or since the last Apply. If other edits came in after yours it says `Other edits came in after yours, so Discard cannot take back only yours. Use Undo.` |
| Arrow beside Apply (**More actions**) | **New sprite from this frame...** and, for several frames, **New sprite from all frames...**. Both ask for a name and make a new sprite. The original stays as it is. Message: `Saved the picture as the new sprite {name}.` |
| Closing the tab | With pending edits asks: `{name} has edits that are not applied. Apply them, or discard them?` with **Apply**, **Discard** and **Cancel**. |
| Save or exit of the program | Applies waiting edits first. If one is refused the save stops with `The edits to {name} were not applied: {reason}`. |

If the sprite changed elsewhere after you started editing it, Apply asks: `{name} changed elsewhere after you started editing it. Applying replaces those changes with your edits. Apply anyway, or discard your edits?` Choose **Apply anyway** or **Discard my edits**. Cancel keeps the edits (`Your edits are kept and not applied. The sprite changed elsewhere.`). For a texture page, font or tile set the question is `The picture of {name} changed elsewhere after you opened it. Applying replaces that change with your picture. Apply anyway?` with the title **Picture changed**.

If the picture changed under you while you drew, the edit is not written and the picture is read again: `The picture changed elsewhere while you drew, so this edit was not applied and the picture was read again.`

The setting **Apply image edits immediately** (Settings, Image editor) sends each edit of a sprite straight into the workspace history instead. Then **Apply** only stops counting the edits as pending. See [Image settings](image-settings.md).

## Frames

For a sprite the **Frames** column has the buttons **Previous**, **Next**, **Add frame**, **Duplicate frame**, **Delete frame**, **Play** and one thumbnail per frame. A click on a thumbnail shows that frame. Playback runs at 120 ms per frame. Add frame puts an empty frame after the current one and shows it. The last frame cannot be deleted here.

Thumbnails are up to 56 pixels. A frame with a different size from its neighbours keeps its own size.

The **All frames** toggle (visible for sprites with several frames) applies a canvas handle resize to every frame. See [Resize and fit](resize-and-fit.md).

## Layers

The **Layers** panel lists the layers of the current frame, top layer first, for sprite frames. Every row has an eye (show or hide), a lock and the name. A click selects, a double click renames. Pictures that are not sprite frames have one layer and no Layers panel actions.

| Button | Meaning |
|---|---|
| **+** | New layer above the current one, named `Layer {n}`. |
| **Copy** | Duplicates the layer as `{name} copy`. |
| **Delete** | Deletes the layer. The last layer cannot be deleted. |
| **^**, **v** | Move the layer up or down. |
| **Merge down** | Merges the layer into the one below. |
| **Rename** | Types a new name. `Enter` or leaving the box saves, `Escape` keeps the old name. |
| **Opacity** | 0 to 100 percent. |
| **Blend** | **Normal**, **Multiply**, **Screen**, **Overlay**, **Add**. |

A locked layer refuses pixel tools: `This layer is locked. Unlock it to change its pixels.` A hidden layer refuses them too: `This layer is hidden. Show it to draw on it.` Deleting or merging into or out of a locked layer is refused with the first message.

The data file stores only the flat result of a frame. Layer stacks are kept in Studio's data folder (`view/<file key>.layers.json`) and are used only while the flattened stack still looks like the frame. When something else changes the frame (replace frame, an undo of another edit, a frame list change), the stack is dropped and the frame becomes one layer. The limits are 64 layers per frame and 16384 pixels on a side. With **Save layers to disk after every edit** on (default), the stacks are written shortly after each edit. Otherwise they are written when the file closes. A damaged stack file is kept under another name and a warning says `The saved layer stacks were damaged. The file was kept as {file}.`

A canvas size change flattens the layers in the command, so the editor shows the flat result afterwards.

## View

| Item | Behaviour |
|---|---|
| Zoom | The buttons **-**, **+**, **Fit** and **1:1**. Steps are powers of 2 and 1.5 times them, from 1/256 up to 256x. Fit is a whole zoom up to 64x for a small picture. `Ctrl` with the wheel zooms around the pointer. The wheel scrolls, with `Shift` sideways. A horizontal wheel or a touchpad swipe pans sideways. |
| Pan | The **Hand** tool, `Space` with the left button, or the middle button. The arrow keys scroll when there is no selection (32 pixels, with `Shift` 128). |
| **Grid** | Fine lines from 6x zoom. The heavier lines every **Grid cell size** pixels (default 16) show while a cell is at least 8 screen pixels. |
| **Tile preview** | Draws the picture repeated around itself, to check that it tiles. |
| **Onion skin** | Draws the frames before and after, faint, under the picture. How many frames and how faint is set in Settings (default 1 frame, 35 percent). |
| **Mirror left-right**, **Mirror top-bottom** | Draws every stroke and shape mirrored. Both on mirrors four ways. |
| **Smooth** | Anti-aliased brush, line, curve and shapes. |

Handles outside the picture change the canvas size: drag an edge or corner. `Shift` keeps the aspect, `Ctrl` stretches the picture, the all-frames key (default `Alt`) applies it to all frames, `Esc` cancels. The hint above the canvas names these keys. A fixed size picture (texture page, mask, glyph) has no handles.

## Undo and redo

The **Undo** and **Redo** buttons and the keys `Ctrl+Z`, `Ctrl+Y` and `Ctrl+Shift+Z` act on the picture in the editor, not on the data file. A disabled button says `Nothing to undo yet.` or `Nothing to redo.` With **Apply image edits immediately** or for texture pages, undo goes through the workspace history. A selection is kept with each edit.

## Clipboard

**Copy** and **Cut** take the selected pixels (the whole picture without a selection). They also go to the system clipboard as a bitmap, so other programs can paste them. **Paste** takes a bitmap from the system clipboard, or the last copy from this program if there is none, and places it at the top left of the picture as a movable selection and switches to the Move tool.

## Troubleshooting

| Message | Cause and fix |
|---|---|
| `This sprite has no frames to edit.` | The sprite has no frames. Add one in the sprite tab. |
| `The edits to {name} were not applied: {reason}` | Apply was refused, for example by a locked sprite. The edits stay in the tab. |
| `Picture changed` | Another editor changed the picture. Choose which version wins. |

## See also

- [Image tools](image-tools.md)
- [Image menus](image-menus.md)
- [Resize and fit](resize-and-fit.md)
- [Image settings](image-settings.md)
- [Sprite editor](sprite-editor.md)
- [Guide: draw on a sprite](guide-edit-sprite-image.md)
