# Resize and fit

The Resize Image/Canvas dialog changes the size of a picture, and the fit rules decide where a picture of a new size goes on the texture pages. This page covers the dialog, the canvas handles, the Advanced options and the messages about pages.

A texture page is a large picture that holds many small pictures (sprite frames, tiles, glyphs). When a picture grows, it may no longer fit where it was, so Studio plans a new place for it. The dialog says what will happen before you apply.

## Open the dialog

- In the image editor: **Resize image...** (Transform menu) or **Resize canvas...** (Canvas menu). With a sprite open and no selection these open the full dialog. With a selection, or for a picture that is not a whole sprite, they ask for plain numbers instead.
- In a sprite tab: **Resize Image/Canvas...** (command `edit.resizeSprite`).

The title is **Resize Image/Canvas**.

## Options

| Control | Meaning |
|---|---|
| **Mode** | **Resize image** stretches the picture and the canvas together. **Resize canvas** changes only the canvas and keeps the pixels. |
| **Unit** | **Pixels** or **Percent**. Pixel sizes start at the current size. The unit and percents are remembered. |
| **Width**, **Height** | 1 to 4096. |
| **Keep the aspect ratio** | Changing one side changes the other. On by default. |
| **Method** | **Nearest** keeps hard pixel edges (default). **Bilinear** blends neighbours. Resize image only. |
| **New area** | **Transparent** (default), **Black** or **White**. The colour of the area a larger canvas adds. Resize canvas only. |
| **Anchor** | A 3 by 3 grid. Where the picture sits on the new canvas. A smaller canvas cuts the other sides. Resize canvas only. |
| **Scale collision box and origin** | On by default. The collision box, its margins and the sprite origin scale with the picture. Off: they keep their pixel values and a mask is read again only where needed. Resize image only. Resize canvas always moves them with the pixels. |
| **Apply to** | For sprites with several frames: **Selected frame (frame {n})** or **All {n} frames**. The all-frames choice shows `All {n} frames will become {w} x {h} (now {sizes})`. The scope is never remembered. |
| **Texture group** | Moves the sprite to another texture group. Shown when the file has groups. |
| **Texture page** | A line that says where the new pictures go (see below). |
| **Advanced** | See below. |

The OK button reads **Resize frame {n}** or **Resize {n} frames**. An option that does not apply to the chosen mode is disabled and its tooltip says why, for example `The anchor applies only to Resize canvas. Resize image stretches the whole picture.`

The choices are remembered for the next dialog and the canvas handles. They are stored with the Image editor settings (see [Image settings](image-settings.md)).

For a font sheet, glyph rectangles follow a canvas change. Scaling, rotating or skewing the whole sheet is refused.

## Canvas handles

Drag an edge or corner of the picture. `Shift` keeps the aspect, `Ctrl` stretches the picture, the all-frames key applies it to every frame (default `Alt`; it can be Alt, Control, Shift or Meta), `Esc` cancels. The result is one undoable step using the dialog's remembered method and box choice. The hint shows `Canvas {w} x {h}` or `Stretch {w} x {h}`, then `, frame {n}` or `, all {n} frames (now {sizes})`, and `, at the limit of 4096 pixels`. A sprite with one frame shows no scope.

## Where pictures go on the texture pages

The line under **Texture page** is one of:

| Message | Meaning |
|---|---|
| `Fits in its place on {page}.` | Same place, no move. |
| `Fits in free space on {page}.` | Moves into empty space. |
| `Fits on {page} after {count} other pictures on it move.` | Other pictures are repacked. |
| `{page} grows from {from} to {to}.` | The page gets bigger. |
| `A new texture page of {size} is needed.` | A new page is added. |
| `No picture has to move.` | Nothing changes on the pages. |

### Advanced

| Option | Default | Meaning |
|---|---|---|
| **Page to try first** | **Auto** | Which page the picture tries first. Not remembered. |
| **Allow growing the page** | on | A page may grow. |
| **Allow a new page** | on | A new page may be made. |
| **Largest page size** | **Auto ({size})** | Choices: Auto, 256, 512, 1024, 2048, 4096, 8192 (square). Auto is the largest page of the file, at least 2048, at most 8192. |

The last three are saved in the Image editor settings. The same options apply when a picture of another size is applied from the image editor.

Page sides are 1 to 8192 pixels. A picture outside this gives `A picture of {w}x{h} cannot be placed: a texture page side is 1 to 8192 pixels.`

## Resize a texture page

**Resize Texture Page...** (Tools menu, and a button on the page tab) sets the size of a whole page. The dialog **Resize texture page** has width and height (1 to 8192), a button to double both, the **Texture page** fit line and **Advanced**. The message after it is `Resized {page} to {width} x {height}.`

## See also

- [Image editor](image-editor.md)
- [Sprite editor](sprite-editor.md)
- [Textures and texture pages](textures.md)
- [Image settings](image-settings.md)
