# Convert image to tile layer

The Convert image to tile layer window builds a room tile layer from a picture by matching each cell against the tiles of a tile set. This page covers the window, the matching rules and the result.

Open it from a room tab with **Convert Image to Tile Layer...** (command `room.imageToTiles`, no default key, in the Room menu).

## Steps

1. Press **Choose image...** and pick a picture (PNG, BMP, GIF, JPEG or WebP, up to 8192 pixels a side). Until then the window says `No picture chosen.`
2. Pick the **Tile set**. Only tile sets with a picture are listed. Without any: `The data file has no tile set with a picture.`
3. Set the **Layer name**.
4. Read the summary: `{matched} cells match a tile. Cells with pixels that match no tile stay empty: {unmatched}.` If the picture is larger than the room: `The picture is larger than the room and is cut at the room edge.`
5. Press **OK**. The message is `Layer {layer} made from the picture: {matched} tiles placed, {unmatched} cells unmatched.`

An unreadable file shows `{name} is not a picture that can be read: {reason}`.

## How it matches

The layer has one cell per tile size, from the top left of the room, covering the room. Each cell is compared pixel by pixel with every tile of the tile set.

- A cell with no visible pixel stays empty.
- A cell that matches no tile stays empty and is counted as unmatched.
- Tile 0 is never matched, because a cell value of 0 means empty.
- Flipped and rotated tiles are not tried.
- Only an exact 1:1 match counts. Scaled or recoloured art will not match.

The new tile layer goes in front of the other layers, like **Add Layer** in the room editor. The whole conversion is one undoable step.

## See also

- [Tile sets](tile-sets.md)
- [Auto tiles](auto-tiles.md)
- [Image formats and limits](image-formats.md)
