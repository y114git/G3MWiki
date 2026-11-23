# Room instances and elements

An instance is one copy of an object placed in a room. This page covers the instance fields, the element list, asset-layer elements and the creation order window.

## Instance fields

When one or more instances are selected on the canvas, the Inspector panel shows the title `Instance {id} ({count} selected)` and these fields. With nothing selected it says `No instance selected.` and the fields are hidden.

| Field | Format | Notes |
| --- | --- | --- |
| **X**, **Y** | whole number, step 1 | |
| **Scale X**, **Scale Y** | up to 3 decimals, step 0.1 | |
| **Rotation** | up to 3 decimals, step 0.1 | |
| **Frame** | whole number, step 1 | Image index. |
| **Speed** | up to 3 decimals, step 0.1 | Image speed. |
| **Object** | list of objects | Changes the object of the instance. |

Typing a value sets it on every selected instance as one undo step. With several selected, the boxes show the first instance, and a box you do not touch changes nothing. Whole-number fields are rounded; a value outside the whole-number range is ignored.

The **Edit Creation Code** button appears when exactly one instance is selected. It opens the instance's creation code. If there is none, G3MStudio asks `Create creation code for {name}?` (reason: `This has no creation code yet.`) and creates an empty entry when you agree.

Next to the object box, **Go to Object** and **Go to Sprite** appear when exactly one instance is selected. Each opens that resource in a tab. **Go to Sprite** is hidden when the object has no sprite. The same two commands are in the right-click menu of the canvas, the Edit menu and the Command palette: `Ctrl+F12` goes to the object, `Alt+Shift+F12` to the sprite. With nothing selected, **Go to Sprite** leads to the tile set or sprite of the selected layer. The tooltip of each button shows the current shortcut.

Instance ids are shown as `inst_` and the id in 8 hex digits. A new instance takes the next free id after the largest id in the file and the data file's last-object counter.

## Placing, copying and deleting

See [Room editor canvas](room-canvas.md) for the Place tool, copy, paste, duplicate, flip, delete and alignment. A room with layers needs an instance layer before an instance can be added.

## Element list

The element list sits beside the canvas behind a splitter. It lists the elements of the selected layer (or of every layer when none is selected, and for older rooms the room's instances): instances, and on asset layers the **sprite**, **sequence**, **particle** and **text** elements. It is hidden when it would be empty.

- A row reads `inst_00000001 : obj_player  (x, y)` for an instance, or the element kind, its name and position for the others.
- Clicking a row selects the element on the canvas and the reverse. Hovering a row shows a card for the object, sprite, sequence or particle system.
- The search box (`Search instances...`) keeps rows whose text contains the typed text, ignoring case. It matches the name and the nickname.
- **Show all** and **Hide all** set every row's check box.
- The check box of a row shows or hides that element on the canvas. Its tooltip: `Shown in the editor. Clear the box to hide this element here only; the room file is not changed and the setting is lost when the room closes.`

## Asset-layer elements

Select one sprite, sequence, particle system or text element on the canvas. A property list for that element appears in the Inspector under the layer panel, so you can edit all of its fields, including the text, font and colour of a text item. The element can be moved with the mouse or arrow keys and deleted. A selected particle system instance can be duplicated from the context menu; a copy gets a new random name.

## Creation order

**Room** > **Instance Creation Order...** opens a window titled `Creation order - {room}`. It lists the room's instances in the order the game creates them: `1  inst_00000001 : obj_player`. The note at the bottom reads `Creation order does not decide the order of events.`

| Button | Action |
| --- | --- |
| **To top** | Move the selected instance to the first position. |
| **Move up** | One position earlier. |
| **Move down** | One position later. |
| **To end** | Move to the last position. |

Each move is one undo step. The window refreshes on undo, redo and nickname changes. It opens only for a room that has an instance list.

## Troubleshooting

| Message | Cause |
| --- | --- |
| `Room '{room}' has no instance layer; add one first.` | Add a layer of kind **Instances** first. |
| `Object '{name}' does not exist.` | The object was deleted or renamed. |
| `'{text}' is outside the range of a whole number.` | A value was too large for a whole-number field. |

## See also

- [Rooms](rooms.md)
- [Room editor canvas](room-canvas.md)
- [Layers](room-layers.md)
- [Objects](objects.md)
- [Sequences](sequences.md)
- [Particle systems](particles.md)
