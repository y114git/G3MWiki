# Animation curves

An animation curve maps a position from 0 to 1 to a value. Sequences use curves for keyframe easing. This page covers the curve editor, which is also the editor behind a sequence keyframe's curve.

## Controls

| Control | What it does |
| --- | --- |
| **Channel** | Box that picks the channel of the curve. With none: `This curve has no channels yet.` |
| Curve type | **Linear**, **Smooth** or **Bezier**. Applies to the channel. |
| **Preset** | Replaces the points with a preset: **Linear**, **Ease in**, **Ease out**, **Ease in and out**, **Step**, **Bounce**, **Bezier ease in**, **Bezier ease out**, **Bezier ease in and out**. |
| Point list | The points of the channel. Selecting a row selects the point on the graph. |
| **X**, **Value** | Position and value of the selected point, -1,000,000 to 1,000,000, step 0.1, 3 decimals. |
| **Add point**, **Delete point** | **Add point** adds a point one unit after the last point of the channel, with the same value, and selects it. **Delete point** removes the selected point. |

Without a project the editor is read-only (`This view is read-only. Open the item from a project to edit it.`). Every edit is one undo step.

## Graph

The graph draws the channel as lines through its points, with a grid. The visible range covers the points with a margin and is at least 0 to 1.

| Action | Result |
| --- | --- |
| Click empty space | Adds a point there (values rounded to 3 decimals). |
| Click a point | Selects it. |
| Drag a point | Moves it. Points stay sorted by X. |
| Drag a handle square (Bezier type, selected point only) | Sets the incoming or outgoing control handle. |
| `Delete` | Deletes the selected point. |

A click that moves less than 3 pixels only selects.

## See also

- [Sequences](sequences.md)
- [Property list](property-grid.md)
