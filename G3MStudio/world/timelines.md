# Timelines

A timeline runs code at chosen steps after it starts. This page covers the timeline tab.

## Moments

The tab lists the moments of the timeline. Each moment is a row with:

- the label `Moment {number} step` and a number box for the step (0 to 1,000,000). Changing it is one undo step;
- **Duplicate**, **Copy to...**, **Move to...** and **Remove**;
- one line per action, showing the code entry the action runs as a link (click to open the code), with its own **Remove**;
- a box `Code entry name` with **Add action**. The name must be an existing code entry; otherwise nothing happens.

Below the moments:

- **Shift moments** with `From step, by`: moves every moment from the given step on by a number of steps (-1,000,000 to 1,000,000). One undo step.
- **Add moment** adds a moment one step after the highest step in the timeline (step 0 when there is none).

## Copying a moment

**Duplicate** copies the moment into the same timeline. **Copy to...** and **Move to...** open a menu of the other timelines (first 200). The moment keeps its step and lands in the target timeline in step order. **Move to...** takes the moment out of the source, keeps its code entries and asks nothing. A copy keeps the code of the source or gets code of its own: the prompt `Copy moment` asks `Copy the code too, so the new moment has code entries of its own, or share the code entries so an edit shows in both moments?` with **Copy the code** and **Share the code**. The choice can be remembered with `Don't ask again`. Settings holds **Copying a timeline moment: always copy the code** and **Copying a timeline moment: always share the code entries**. See [Settings](../settings/README.md).

The rest of the timeline's fields are in the [property list](property-grid.md); the moments list is left out of it.

## Make a timeline

1. Choose **File** > **New** > **New Timeline...**, type `tl_boss` and press **Create**.
2. Press **Add moment**. The moment gets the step after the last one. Type the step in the number box (for example 60 for two seconds at 30 steps per second).
3. In `Code entry name` of that moment type the name of an existing code entry and press **Add action**. Find exact names under Code in the Explorer. An unknown name does nothing.
4. Add more moments for later steps. **Shift moments** moves everything from a step on, for example to open a gap before step 120.
5. Start it from code: `timeline_index = tl_boss; timeline_position = 0; timeline_running = true;`

## See also

- [Property list](property-grid.md)
- [Resource management](resources.md)
- [Code editor](../code/README.md)
