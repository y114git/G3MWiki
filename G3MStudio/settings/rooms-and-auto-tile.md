# Rooms and auto-tile settings

The **Rooms** and **Auto-tile** categories set the room editor grid and the defaults of the auto-tile brush. Both sit under one heading block; **Auto-tile** is the second heading on the **Rooms** page and has its own entry in the category list.

## Rooms

| Label | Control | Default | Range | Saved as |
| --- | --- | --- | --- | --- |
| **Use one grid for every room** | Checkbox | Off | | `roomGridOverride`. Off: each room keeps its own grid. |
| **Grid width and height** | Two numbers | 16 and 16 | 1 to 1024 each | `roomGridWidth`, `roomGridHeight` |

## Auto-tile

| Label | Control | Default | Range | Saved as |
| --- | --- | --- | --- | --- |
| **Treat the layer edge as terrain** | Checkbox | Off | | `autoTile.outsideIsTerrain`. Cells beyond the layer edge count as terrain, so terrain painted at the edge continues past it. |
| **Choose the first rule set of a tile set when its layer is selected** | Checkbox | Off | | `autoTile.startOn` |
| **Layout of a new rule set (0 = 47 tiles, 1 = 16 tiles, 2 = 256 masks)** | Number | 0 (47 tiles) | 0 to 2 | `autoTile.defaultLayout` (`Blob47` by default) |

## Restore defaults

The **Rooms** reset covers the grid options. The **Auto-tile** heading has a reset of its own, which resets the auto-tile options.

## See also

- [Rooms](../world/rooms.md)
- [Room canvas](../world/room-canvas.md)
- [Auto tiles](../graphics/auto-tiles.md)
