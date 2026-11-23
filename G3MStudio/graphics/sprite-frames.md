# Sprite frames and import

A sprite is a list of frames. This page covers the frame strip, playback, the frame buttons and keys, importing a sheet, strip or GIF, replacing a frame, exporting a GIF and importing or exporting collision masks.

Pictures that you import are placed on the texture pages for you when you save the data file. To see or move the page item of a frame, open it under **Texture Page Items**.

## The frame strip

The strip appears when the sprite has more than one frame. Each frame is cut out of its texture page and shown in order (up to 56 pixels on its longest side). The frame in view has an outline. Selected frames are tinted. Pictures arrive as the pages are decoded, so a large sprite opens without freezing the window.

| Action | Result |
|---|---|
| Click a frame | Selects it and shows it on the canvas. |
| `Ctrl` + click | Adds or removes the frame in the selection. |
| `Shift` + click | Selects every frame from the first click to this one. |
| Drag a frame | Moves the selected frames to the drop mark (the line between frames). They keep their order. One undoable step. |

Keys (the strip needs the focus; click it first). `Ctrl` is `Cmd` on macOS.

| Key | Action |
|---|---|
| Left, Right | Previous or next frame. |
| `Space` | Play or pause. |
| `Delete` | Delete the selected frames. |
| `Ctrl+A` | Select every frame. |
| `Ctrl+D` | Duplicate the selected frames. |

A text next to the playback controls shows `Frame {n} / {count}`.

## Playback

Nothing plays on its own. The controls above the strip:

| Control | Meaning |
|---|---|
| **Play** / **Pause** | Starts or stops playback in the editor. |
| **Loop** | On by default. Off: playback stops on the last frame. |
| **Speed** | The sprite's playback speed, 0.1 to 1000, with the unit beside it: `frames per second` or `frames per game frame`. Changing it edits the sprite (one undo step). |
| **Onion skin** | Draws the neighbouring frames faintly behind the current one on the canvas. The image editor option sets how many (see [Image settings](image-settings.md)). |
| **Lock origin** | See [Sprite editor](sprite-editor.md). |

The editor timer runs at the speed of the sprite. A speed in frames per game frame counts as 30 game frames per second. The interval is kept between 0.25 and 240 frames per second.

## Frame buttons

All frame edits are single undoable steps.

| Button | What it does |
|---|---|
| **Add** | Adds an empty frame (the sprite's size, fully transparent) after the last selected frame and selects it. |
| **Duplicate** | Puts a copy of every selected frame after it. |
| **Delete** | Deletes every selected frame. |
| **Import...** | Opens a file picker for a sheet, strip or GIF and then the Import dialog. |
| **Edit Image...** | Opens the [image editor](image-editor.md) on the selected frame. |
| **Resize Image/Canvas...** | Opens the [Resize dialog](resize-and-fit.md). |

Deleting the last frame is a warned refusal: `Delete the last frame of {name}?` The warning says a sprite without frames draws nothing and the game can fail where it uses the sprite. You may overrule it. Undo puts the frame back.

## Import a sheet, strip or GIF

Use **Import...** (frame strip) or **Import Sheet or Strip...** in the **Sprite** menu to add frames to the open sprite. Use **New Sprite from Sheet or Strip...** (File menu import entries, and the menu of the Sprites group in the Explorer) to make a new sprite instead.

1. Pick a file (`Choose a sheet, strip or GIF`). PNG, BMP, GIF, JPEG and WebP are read. Other formats give `The picture format is not supported. Use PNG, BMP, GIF, JPEG or WebP.` A picture over 8192 pixels on a side is refused.
2. In **Import sheet or strip**, choose **Cut by frame count** or **Cut by frame size** and set the fields. A summary shows the result: `{count} frames of {width} by {height} pixels.`
3. Choose **Replace the current frames and take the new size** if the frames differ in size from the sprite or if you want to drop the old frames.
4. Press **OK**.

| Field | Meaning |
|---|---|
| **Columns**, **Rows** | Frames across and down (by count). 1 to 256. |
| **Frame width**, **Frame height** | Size of one frame (by size). 1 to 8192. |
| **Offset X**, **Offset Y** | Distance of the first frame from the top left. 0 to 8192. |
| **Gap X**, **Gap Y** | Space between frames. 0 to 8192. |
| **Frames (0 = all)** | Stop after this many frames. 0 to 1024. |

Frames are cut row by row from the top left. A cell that does not lie fully inside the sheet is left out. A file named `name_strip8.png` starts as 8 frames in one row, cut by count. Without a strip name the dialog uses the choice you made last (by count at first). An animated GIF is not cut: it brings its own frames, each drawn over the previous one as a viewer does. At most 1024 frames are read from a GIF. A GIF larger than 8192 pixels or with more frames is refused.

Without **Replace**, the frames must have the size of the sprite: `The sprite is {width} by {height}. Tick Replace to take the new size.` With **Replace**, the sprite takes the frame size, its box margins become the whole picture, and the collision masks are built again. The result is one undoable step. A new sprite opens in its own tab. Messages: `Imported {count} frames into sprite {sprite}.` or `{name} is not a picture that can be read: {reason}`.

## Replace a frame

**Replace Sprite Frame...** in the Sprite menu replaces the picture of the selected frame. It asks for a picture (`Picture for the frame`). The result is one undoable step and the message `Frame {frame} of {sprite} replaced.` (see the note at the end). A file that cannot be read is refused before it reaches the history. For pictures of another size, edit the frame in the [image editor](image-editor.md), which fits the texture pages.

## Export as GIF

**Export as GIF...** saves every frame as an animated GIF (`Save the sprite as a GIF`). Transparent pixels are kept. The frame delay comes from the playback speed in hundredths of a second, never below 2. The GIF loops forever. The message is `Sprite {sprite} was saved as an animated GIF to {path}.`

## Collision mask pictures

**Export Mask...** saves the mask of the selected frame as a black and white PNG, white for solid (`Save the mask as`). A sprite with one shared mask uses the last mask for any frame. **Import Mask...** reads a picture back (`Choose a mask picture`). The picture must have exactly the size of the mask: `The mask picture must be {w} x {h}, not {w} x {h}.` A pixel is solid when it is opaque and lighter than mid grey. A sprite with no mask says `This sprite has no collision mask.` Messages: `Mask {index} of {sprite} was imported.` and `Mask {index} of {sprite} was saved to {path}.` The index counts from 0.

## Preview player

The object editor shows a small player for a sprite with the buttons **Play** or **Pause**, **Stop**, **Loop** and a speed list (`0.5x`, `1x`, `2x`). It draws each frame at its place in the sprite's bounding box. With one frame it is disabled. The state is not saved.

Before you press **Play** it shows the still picture of the sprite, which is the first frame with a visible pixel by default. **Stop** returns to that picture. The same still picture is used for Explorer rows, hover cards, Go To, search results and pickers. The room editor, the sprite editor canvas and the exports always use the real frames. Change the rule in **Settings** > **Appearance** > **Sprite previews start at** (**First visible frame** or **First frame**). See [Explorer settings](../settings/explorer-settings.md).

## Limits

| Limit | Value |
|---|---|
| Picture side | 8192 pixels |
| Frames from a sheet or GIF | 1024 |
| One encoded picture | 64 MiB of PNG |
| Collision box edges | 0 to 65535 |

## Troubleshooting

| Message | Cause and fix |
|---|---|
| `The frames differ in size.` | A set of frames with different sizes cannot be imported together. |
| `There are no frames to import.` | The cut produced no frames. Check the offset, size and gaps. |
| `The picture is larger than 8192 pixels on a side.` | Reduce the picture first. |

## See also

- [Guide: import a sprite strip](guide-import-sprite-strip.md)
- [Sprite editor](sprite-editor.md)
- [Image editor](image-editor.md)
- [Textures and texture pages](textures.md)
- [Image import and export formats](image-formats.md)
