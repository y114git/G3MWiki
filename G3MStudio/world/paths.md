# Paths

A path is a list of points an instance can follow (`path_start`). This page covers the path tab: the canvas, the point list and the tools.

## Canvas

The canvas draws the path with its points. A read-only view (opened without a project) cannot be edited and shows `This view is read-only. Open the item from a project to edit it.` on the buttons.

| Action | Result |
| --- | --- |
| Click empty space | Adds a point. On a segment it is inserted after that segment's start point, elsewhere at the end. The new point is selected. |
| Click a point | Selects it. |
| Drag a point | Moves it, as one undo step. If several points are picked, the whole group moves. |
| `Ctrl`+click a point | Adds or removes it from the picked points. |
| `Delete` | Deletes the selected point, or all picked points when several (but not all) are picked. Deleting the last remaining point is a risky edit and asks first, because a path needs at least one point (`Delete the last point of {name}?`). |
| Pan button (middle unless changed in [Image editor settings](../settings/image.md)) or right button drag | Pans. |
| Wheel | Zooms (0.1 to 64) around the pointer. `Shift`+wheel, a horizontal wheel or a touchpad swipe pans sideways. |

## Side column

| Control | What it does |
| --- | --- |
| Point list | `1  x, y` per point. Selecting a row selects the point on the canvas. |
| **X**, **Y** | Position of the selected point, whole numbers, -100000 to 100000. Disabled with `Select a point in the list or on the canvas first.` when none is selected. |
| **Speed %** | Speed of the selected point, 0 to 10000 (100 means factor 1). |
| **Reverse** | Reverses the order of all points, whatever is picked. |
| **Mirror** | Mirrors the picked points (or all when none is picked) horizontally, about the centre of their bounds. |
| **Flip** | Flips the picked points (or all) vertically, about the centre of their bounds. |
| **Reset view** | Fits the path to the canvas again. |
| **Room behind the path** | Draws the outline and instance positions of a room behind the path. Editor state only, nothing is saved. |
| **Snap to grid** | Grid step in path units for new and dragged points. 0 keeps whole units. Maximum 1024, step 8. |
| **Closed** | Joins the last point to the first. |
| **Smooth** | Draws the path as a curve. |
| **Move all** with **X** and **Y** | Shifts every point by that amount (-100000 to 100000 each). Nothing happens when both are 0. |

Point positions are rounded to whole numbers. Every change is one undo step. See [Property list](property-grid.md) for the remaining path fields.

## Make a path

1. Choose **File** > **New** > **New Path...**, type `path_patrol` and press **Create**. The tab opens with an empty canvas.
2. Click on the canvas to add points. Each click adds the next point. Click on a segment to insert a point between two others.
3. Drag a point to move it, or select it and type **X** and **Y** in the side column. Turn on **Snap to grid** and type a step to keep points on a grid.
4. Tick **Closed** for a loop and **Smooth** for a curve. Select a point and set **Speed %** to slow the instance down there (50) or speed it up (200).
5. Choose a room in **Room behind the path** to see where the points fall on the level.
6. Use it in code, for example in a Create event: `path_start(path_patrol, 4, path_action_restart, false);`

## See also

- [Rooms](rooms.md)
- [Resource management](resources.md)
