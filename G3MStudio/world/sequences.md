# Sequences

A sequence is an animation made of tracks with keyframes. This page covers the sequence tab: preview, timeline, graph mode, moments and messages, and tracks.

## Tab layout

From top to bottom: the hint `This sequence has no tracks yet.` (only when empty), the preview canvas, the **Graph** toggle, the timeline (or the graph), a message line, the moments and messages lists, the tracks, and the [property list](property-grid.md). Without a project the editors are read-only, the moments and messages lists are not shown and the tracks are listed as plain text, indented by depth.

## Preview

The canvas draws the sprites of the sequence at the current frame around the sequence origin. A drag bar under it sets its height. Below it: **Play** / **Pause**, a frame slider and the label `Frame {n} / {length}`. Play loops: after the last frame it starts again at 0. The speed comes from the sequence: frames per second, or 30 times the value for frames per game frame, limited to 1 to 240.

## Timeline

The timeline shows a ruler, a playhead and one row per track, plus rows named **Moments** and **Broadcast messages** when they have entries. Each keyframe is a bar whose length is the keyframe length.

| Action | Result |
| --- | --- |
| Click the ruler | Moves the playhead (and the preview) to that frame. |
| Drag a bar | Moves the keyframe by whole frames, never before frame 0. The bar snaps to the edges of other keyframes in the row and to the playhead (within 6 pixels). One undo step on release. |
| Right click a track row | Menu: **Add keyframe at playhead**, **Split keyframe at playhead**, **Delete keyframe**. Split and delete need a keyframe under the pointer. The menu appears only on track rows, not on the **Moments** and **Broadcast messages** rows. |

A refused menu edit shows its reason in the message line. **Add keyframe at playhead** copies the shape of an existing keyframe of the track, so it is refused on a track with no keyframe, and when the playhead frame already holds a keyframe. **Split keyframe at playhead** needs the playhead inside the keyframe, after its start and before its end. Keyframe lengths are not changed on the timeline; use the property list of the track.

## Graph mode

The **Graph** toggle swaps the timeline for a graph of one number track at a time (position, scale, rotation and so on). Pick the track and the channel. Drag a point to change its value, or type **Frame** and **Value** under the graph. A keyframe stays between its neighbours. Frame range 0 to 10,000,000, value -1,000,000 to 1,000,000. Pick the channel by its number (0, 1, ...). The graph lists only tracks whose keyframes hold numbers. A dragged point lands on a whole frame, and the value is rounded to 3 decimals. The timeline is the place to add, split and delete keyframes. Lengths are in the property list of the track.

The curve of the selected keyframe is shown as **Curve of this keyframe**, a property list for an embedded curve. A keyframe that uses a shared curve shows `Uses the animation curve {name}`. See [Animation curves](animation-curves.md).

## Moments and broadcast messages

Two lists, **Moments** and **Broadcast messages**. Each row has a frame number box, a text box and **Remove**. The text box holds one function name per line for a moment, or one message per line for a broadcast message, and is applied when it loses focus. A frame that another row already uses is refused and the old value comes back. A box next to **Add moment** or **Add message** sets the frame (0 to 10,000,000; default: one after the highest frame, or 0 for the first). One undo step each.

## Tracks

Each track is a collapsible row named `track name  model name` that opens into the property list of the track. **Remove** deletes the track. **Add group track** adds a group track named `Group`. A track with a sprite has **Edit Image...**.

## What the editor can add

The editor moves keyframes, splits them, changes moments and broadcast messages and edits every field of a track in its property list. It adds only group tracks (**Add group track**). It does not add sprite, text or other track types, so a new sequence from **File** > **New** > **New Sequence...** starts empty and stays a shell for group tracks. To change a sequence that exists, work on a copy so the original stays: select it and choose **Edit** > **Duplicate Resource...**.

## Change the timing of a sequence

1. Open the sequence. The preview shows the sprites at the playhead.
2. Click the ruler to move the playhead to a frame.
3. Drag a keyframe bar to move it. It snaps to the neighbouring keyframes and to the playhead. Right click a row and choose **Split keyframe at playhead** to cut one in two.
4. Turn on **Graph**, pick a track and a channel, and drag a point to change a value. Type **Frame** and **Value** for exact numbers.
5. Add a moment with **Add moment** (the box beside it sets the frame) to mark a frame for code.
6. Press **Play** in the preview. The speed comes from the sequence.

## See also

- [Animation curves](animation-curves.md)
- [Room instances and elements](room-instances.md#asset-layer-elements)
- [Resource management](resources.md)
