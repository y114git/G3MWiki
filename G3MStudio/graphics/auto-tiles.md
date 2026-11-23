# Auto tiles

Auto-tile rule sets let the room editor pick the right edge and corner tile while you paint terrain. This page covers the **Auto-tile rule sets** section of a tile set tab, the rule file Studio keeps, and the settings that affect it.

GameMaker data files store no auto-tile rules, so Studio keeps them in its own data folder. A rule set belongs to one tile set of one data file. It maps a neighbour pattern (a "slot") to a tile number of that tile set.

## Where the rules are stored

Rule sets are saved in the Studio data folder as `autotile/<file key>.json`, one file per data file. Every change is written at once through a temporary file and a rename. They are not part of the data file, so they are not in the undo history and not written when you save the data file. A damaged rule file is read as empty and replaced at the next change. If a change cannot be written, it is dropped and the screen keeps showing what is on disk. Renaming a tile set moves its rule sets to the new name.

## Layouts

A rule set uses one of three layouts, chosen when you create it:

| Layout | Slots | Standard sheet columns | What a slot means |
|---|---|---|---|
| 47 | 47 | 8 | The 47 distinct shapes of the eight neighbours. A corner counts only when both edges beside it are terrain. |
| 16 | 16 | 4 | The four edges only (north, east, south, west). |
| 256 | 256 | 16 | Every combination of the eight neighbours. |

The neighbour bits are N 1, NE 2, E 4, SE 8, S 16, SW 32, W 64, NW 128. A slot button shows its pattern as three rows of marks: `#` terrain, `.` no terrain, `+` the cell itself. A 16 slot shows only the edges.

## Edit rule sets

Open the **Auto-tile rule sets** section (folded by default) in a tile set tab.

| Control | Meaning |
|---|---|
| Rule set list | Rule sets of this tile set. |
| **New** and the layout box (`47`, `16`, `256`) | Starts a rule set with every slot empty, named **Terrain** (then `Terrain 2`, `Terrain 3`). The layout box defaults to the Settings value. |
| **Delete** | Removes the chosen rule set. |
| **Name** | Renames the rule set when the box loses focus. A name in use gets a number added. |
| **Terrain** | A number from 1 to 255. A cell of any rule set with the same terrain counts as a neighbour, so several rule sets on one layer do not fight. A cell of another terrain does not count. |
| **Click a tile in the picture to assign it to the slot** | When on, a click on a tile in the tile picture assigns it to the selected slot. Clicking a slot button turns it on. |
| Slot buttons | The pattern and the assigned tile number (`-` when empty). The selected slot has a border. The tooltip is `Slot {slot}`. |
| **First tile**, **Columns**, **Auto-fill from standard layout** | See below. |
| **Import JSON...** and **Export JSON...** | See below. |

### Assign tiles by hand

1. Pick or create a rule set.
2. Click a slot button.
3. Click the tile for it in the tile picture above. Studio assigns it and moves to the next slot.
4. Repeat. Assigning tile 0 clears a slot. Tile 0 is the empty tile and cannot be used in a rule.

### Auto-fill

If your tile set is drawn like the standard sheet, set **First tile** (1 to 65535) and **Columns** (1 to 256, defaults to the layout's standard width), then press **Auto-fill from standard layout**. Slot order is ascending. The first slot gets the first tile, the next slots follow to the right and wrap after **Columns** tiles, one row of the tile set down each time. Slots whose tile is outside the tile set stay empty. Auto-fill replaces all slots of the rule set.

### Import and export

**Export JSON...** saves the chosen rule set as an indented JSON file. **Import JSON...** reads a rule set from a file into the current tile set. The imported set takes this tile set's name as owner, and a name already in use gets a number. Slots outside the layout are dropped, and terrain is clamped to 1 to 255. A file larger than 4 MB, or one that is not a rule set, gives `The file is not an auto-tile rule set.` A rule set holds at most 1024 slots.

## Use rules in a room

In a room tile layer, **Toggle Auto-Tile Brush** (`room.autoTile`) paints terrain with the rules, and **Re-Tile Selection** (`room.retileSelection`) recomputes the tiles inside a selection. Neither has a default shortcut. Painting changes the painted cells and their eight neighbours, and a neighbour of the same terrain is retiled with the rule set that owns its tile. A cell whose slot has no tile keeps its value. The mirror, flip and rotate bits of a cell never change. See the room pages for the brush controls.

## Settings

Settings, **Auto-tile** section:

| Setting | Default | Effect |
|---|---|---|
| **Treat the layer edge as terrain** | off | Cells beyond the layer edge count as terrain, so terrain painted at the edge continues past it. |
| **Layout of a new rule set (0 = 47 tiles, 1 = 16 tiles, 2 = 256 masks)** | 0 | The layout box value for **New**. |
| **Choose the first rule set of a tile set when its layer is selected** | off | The brush starts with the first rule set of the layer's tile set. |

## See also

- [Tile sets](tile-sets.md)
- [Image editor](image-editor.md)
- [Settings pages](../settings/README.md)
