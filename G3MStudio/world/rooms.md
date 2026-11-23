# Rooms

A room is one screen or level of a GameMaker game. This page shows how G3MStudio presents a room and lists the menu commands that work on the room tab in front.

## Opening a room

Double click a room in the Explorer. The room opens in a tab with a toolbar, a ruler on the top and left edges, the picture of the room, and a status line under it. The layers, the selected instance fields, the creation code buttons and the tile palette appear in the Inspector panel while the room tab is in front. The room properties (name, size, speed, flags, views, physics and so on) are in the property list of the same tab. See [Generic property list](property-grid.md).

The picture is drawn from the data file: instances show the sprite of their object, tiles and background layers show their pictures. A room the preview cannot read keeps its property list, but the picture stays black. The first time a room is shown it is fitted to the canvas. The zoom and position are remembered with the tab.

Hovering the picture writes `x`, `y` and the snapped position into the status line under it, followed by the description of the element under the pointer. Hovering an instance shows a tip with its object name and instance id; hovering a cell of a tile layer shows `Tile {index} at {x}, {y}`.

## Older rooms

A room from GameMaker Studio 1 has no layers. It has no layers panel, but its instances are listed in the element list beside the picture, and instances can still be selected, moved and edited.

## Room menu

The **Room** menu works on the room tab in front. When no room is in front the commands show the notice `Open a room first.`

| Command | Shortcut | What it does |
| --- | --- | --- |
| Play This Room | Ctrl+Shift+F5 | Runs the game starting in this room. See [Run the game](../projects/run-game.md). |
| Room Creation Code... | | Opens the creation code of the room in the code editor. |
| Instance Creation Order... | | Opens the [creation order window](room-instances.md#creation-order). |
| Export Tiles as CSV... | | Saves the tiles of the selected tile layer. See [Tiles](room-tiles.md#csv-export-and-import). |
| Import Tiles from CSV... | | Reads tiles into the selected tile layer. |
| Convert Image to Tile Layer... | | Makes a tile layer from a picture. See [Tiles](room-tiles.md#convert-image-to-tile-layer). |
| Save Room as PNG... | | Saves the room picture at 100%. |
| Toggle Auto-Tile Brush | | Turns the auto-tile brush on or off. |
| Re-Tile Selection | | Applies the auto-tile rules to the selected tiles. |
| Add Layer | | Adds a layer of the kind chosen in the layers panel. |
| Duplicate Layer | | Copies the selected layer. |
| Show or Hide Grid | | Same as the Grid button (key `G`). |
| Snap to Grid On or Off | | Same as the Snap button (key `Shift+G`). |
| Fit Room to Window | | Same as the Fit button (key `Ctrl+0`). |
| Zoom to 100% | | Same as the 100% button. |

Only Play This Room has a default shortcut. The others can be given one in the shortcuts editor.

### Save Room as PNG

The file is written at 100% zoom. If the room is too large for a PNG, a risky-action prompt asks whether to continue with a reduced picture. The picture is drawn into memory first, so a failure leaves no empty file. Messages: `Room {room} saved to {path}.` and `Could not save the picture: {reason}`.

### Room creation code

If the room has no creation code entry, G3MStudio shows the prompt `Create creation code for {name}?` with the reason `This has no creation code yet.` Confirming creates an empty entry and opens it. The same prompt appears for an instance, named `inst_` and the instance id in hex.

## Room order

The room order decides which room the game starts in and what `room_goto_next` reaches. Open it with **Tools** > **Room Order...**. See [Room order](room-order.md).

## See also

- [Room editor canvas](room-canvas.md)
- [Layers](room-layers.md)
- [Instances](room-instances.md)
- [Tiles](room-tiles.md)
- [Room order](room-order.md)
- [Resource management](resources.md)
