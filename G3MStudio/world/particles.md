# Particle systems

A particle system is a set of emitters that spawn small moving pictures. This page covers the particle system tab: the live preview and the emitter list.

## Preview

The preview simulates the system 60 times a second while the tab is shown. When no particle is left for a while it starts again, so systems made of bursts keep playing. It is an approximation of the game's own result. A drag bar under it sets its height. **Restart** restarts the simulation.

Drag an emitter's region in the preview to move it. The change is one undo step. It moves the region centre relative to the system origin.

## Emitters

Under the preview each emitter is a collapsible row with a one-line summary: name, `on` or `off`, emit mode and count (`x{count}`), shape and region size. Opening a row shows the property list of that emitter. Controls on the row:

| Control | What it does |
| --- | --- |
| **Edit Image...** | Opens the emitter's sprite in the image editor, when it has one. |
| **Show** | Check box that enables or disables the emitter in the file. |
| **Solo** | Preview only this emitter. Editor state, not stored. |
| **Lock** | Disables the property list of that emitter so it cannot be changed by accident. Editor state, not stored. |
| **Duplicate** | Copies the emitter. |
| **Remove** | Deletes the emitter. |

Under the list: **Add emitter** adds an emitter with default settings, and **Copy GML** copies GML code that rebuilds the system to the clipboard. Every change is one undo step. Without a project the list is text only.

The other fields of the system are in the [property list](property-grid.md); the emitter list is left out of it. A particle system placed in a room is an element of an asset layer. See [Room instances and elements](room-instances.md#asset-layer-elements).

## Make a particle system

1. Choose **File** > **New** > **New Particle System...**, type a name and press **Create**. The system starts with no emitters.
2. Press **Add emitter**. A row appears and the preview starts.
3. Open the row. In its property list set the sprite (press **Set...** in the sprite field), the emit mode and count, and the shape and size of the region.
4. Drag the emitter's region in the preview to place it. Press **Restart** to see a burst again.
5. Add a second emitter and tick **Solo** on one of them to judge it alone. **Solo** and **Lock** are not saved. Clear **Show** to switch an emitter off in the file.
6. Place the system in a room as an element of an asset layer, or press **Copy GML** and paste the code into a script to build the system while the game runs.

## See also

- [Sequences](sequences.md)
- [Resource management](resources.md)
