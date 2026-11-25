# Tile sets

A tile set tab shows the picture of a tile set, the grid layout the game uses to cut it into tiles, the list of tile ids and the animated tiles. This page covers everything in that tab except auto-tile rules, which have [their own page](auto-tiles.md).

A tile set (a "background" in older GameMaker data files) is one picture cut into equal cells. A room tile layer stores a tile as the number of a cell, so the cell layout in the tile set decides which tile a number means. Tile index 0 is the empty tile.

## Open a tile set

Open a tile set from **Tile Sets** in the Explorer. The tab has these parts from top to bottom:

1. **Tile picture**, with a hint line and a one line summary of the layout.
2. The options row: **Show tile grid (borders and separation)**, **Show tile indices**, **View in columns** and **Original columns**.
3. The picture itself, with a zoom slider and the buttons **Export PNG**, **Replace PNG** and **Edit Image...**.
4. **Tile ids** (only when the tile set lists ids), then **Find rooms using this tile**.
5. Two folded sections: **Animated tiles** and **Auto-tile rule sets**.
6. The property list of the tile set below.

The layout summary reads, for example: `Tile size: 16 x 16. Columns: 8. Tiles: 64. Border 0, 0. Separation 0, 0.` It follows edits to the tile set properties.

## The picture

The picture area is 260 pixels high. Drag the bar under it to change the height. The zoom slider runs from 1x to 8x (`Ctrl` + wheel zooms too). The tab opens at the largest whole zoom that fits. The picture sits on a chequer pattern that shows transparent pixels.

| Control | What it does |
|---|---|
| **Export PNG** | Asks for a file name (default `texture.png`) and saves the tile set picture as a PNG. The caption beside the buttons says `Saved to {path}` or `Could not save: {reason}`. |
| **Replace PNG** | Asks for a picture (`Picture for {name}`), then replaces the tile set picture as one undoable step. The caption shows `Picture of {name} replaced.` A file that cannot be read gives `{name} is not a picture that can be read: {reason}` and changes nothing. |
| **Edit Image...** | Opens the [image editor](image-editor.md) on the tile set picture. |

A click on a tile in the picture outlines that tile and selects its row in **Tile ids**. Clicking outside every tile clears the outline.

### Replacing or editing a picture of another size

A new picture of the same size replaces the old one in place. A picture of another size finds a new place on the texture pages (see [Resize and fit](resize-and-fit.md)). Tiles are cut from a grid that starts at the top left of the picture, so only a picture that grows to the right or downward keeps every tile where it was. When the new picture is smaller, or its content moved, applying it from the image editor asks first with a warning about the tile grid. The warning can be overruled to apply the picture anyway.

## Grid and index overlays

- **Show tile grid (borders and separation)** outlines every cell in cyan. Use it to check the tile size, border and separation: a wrong value shows as cells that miss the artwork.
- **Show tile indices** draws the number of each tile in its corner.

Both draw at most the first 16384 tiles. Both are view options. They are not saved.

## View in columns

**View in columns** redraws the tiles in the number of columns you type (1 up to the tile count). It helps with a sheet whose tiles were saved in a scrambled order. **Original columns** returns to the stored column count.

This is a view only. The tile set, its picture and the tile numbers do not change. The overlays and the outline follow the new layout. The box shows this tip: "Draws the tiles in this many columns so a scrambled sheet is easier to read. Only the view changes: the tile set, its picture and the tile numbers stay as they are."

## Tile ids

**Tile ids** appears when the tile set lists ids. It has one row per tile in the form `index: ID`, where the index is the tile number and the ID is the value that rooms store. Select a row to outline its tile on the picture. Change the ids in the property list below the view.

## Find rooms using this tile

Pick a tile (click it or select its row), then press **Find rooms using this tile**. The Usages panel opens with a search for tile `{tile set} #{index}`. Without a picked tile the button is disabled and says `Pick a tile in the sheet or the list first.`

## Animated tiles

The **Animated tiles** section is always folded in the tab, but it holds controls only when the tile set has animation data. It has:

| Control | Meaning |
|---|---|
| List | One row per animated tile: `index: frame ids`. |
| **Frames** and **Set frames** | Type the frame tile numbers of the picked row as a comma or space separated list, then press **Set frames**. One undoable edit. A bad value shows its message under the controls. |
| **Speed (frames per second)** | Range 1 to 1000. Stored as the length of one frame in microseconds, `1000000 / fps` rounded. |

Frames after the last distinct one are padding the file needs. They are hidden in the box and added again when you set frames.

## Editing the tile layout

Tile width and height, borders, separation, columns and tile count are properties in the property list below the view. After a change the layout summary, the overlays and the room tile palette use the new values.

## Related room features

The tile palette of the room editor uses this layout. It shows the tile set picture, treats tile 0 as the empty cell and starts with tile 1 selected. Mirror (bit 0), flip (bit 1) and rotate (bit 2) flags are stored in the tile value, not in the tile set. Room tile editing is described in the world pages. See [Convert image to tile layer](image-to-tiles.md).

## Troubleshooting

| Message | Cause and fix |
|---|---|
| `Tile set '{name}' cuts its tiles from a grid that starts at the top left ...` | The edited picture is smaller or has moved content. Apply anyway only if you also change the tile size or ids. |
| `{name} is not a picture that can be read: {reason}` | The file is not a readable picture. Use PNG. |

## See also

- [Auto tiles](auto-tiles.md)
- [Image editor](image-editor.md)
- [Textures and texture pages](textures.md)
- [Resize and fit](resize-and-fit.md)
- [Image settings](image-settings.md)
