# Image formats and limits

This page lists which picture files G3MStudio reads and writes, and every size and count limit in the graphics area.

## Reading pictures

Every picture import (replace frame, replace PNG, import sheet, mask import, **Edit Image...** on a loose file, paste) reads only plain raster formats. The format is found from the file signature, not the extension, so a file that claims to be another type is refused.

| Format | Notes |
|---|---|
| PNG | The usual choice. Keeps transparency. |
| BMP | |
| GIF | A sprite import reads every frame of an animated GIF. A single picture uses the first frame. |
| JPEG | No transparency. |
| WebP | |

Any other file gives `The picture format is not supported. Use PNG, BMP, GIF, JPEG or WebP.` A picture over 8192 pixels on a side gives `The picture is larger than 8192 pixels on a side.` A file that cannot be read gives `{name} is not a picture that can be read: {reason}`.

## Writing pictures

| What | Format |
|---|---|
| Texture page, page item, tile set, font sheet, sprite frame | PNG |
| Collision mask | PNG, black and white (white is solid) |
| Sprite | Animated GIF, looping forever, transparent pixels kept |
| Included picture (loose file) | Keeps its type: png, jpg, jpeg, bmp or gif. JPEG has no transparency and a GIF keeps its first frame. |
| Copy in the image editor | Also a bitmap on the system clipboard |

Inside the data file, pictures are stored on texture pages in the file's own format. Studio decodes pages when needed. The frames of one sprite import can be at most 64 MiB as PNG in total.

## Limits

| Limit | Value |
|---|---|
| Picture side read from a file | 8192 pixels |
| Resize dialog and canvas handles | 1 to 4096 pixels |
| Texture page side | 1 to 8192 pixels |
| Sprite zoom in the sprite editor | 1x to 32x (up to 8192 screen pixels on a side) |
| Image editor zoom | Up to 256x. Down to where the longest side is about 16 screen pixels, never below 1/256 |
| Frames cut from a sheet or GIF | 1024 |
| Layers per frame | 64 |
| Layer stack picture side | 16384 pixels |
| Brush size | 1 to 256 |
| Font file | 16 MiB |
| Font glyph character code | U+0000 to U+FFFF |
| Auto-tile import file | 4 MB, 1024 slots |
| Texture groups, pages | No fixed limit in Studio |

## See also

- [Sprite frames and import](sprite-frames.md)
- [Resize and fit](resize-and-fit.md)
- [Textures and texture pages](textures.md)
