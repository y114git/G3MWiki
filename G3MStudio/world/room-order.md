# Room order

The room order is the list of rooms the game can reach in sequence. This page describes the Room Order window.

## Open it

**Tools** > **Room Order...** (one window per open data file). The window title is `Room Order`. The hint reads: `The first room is where the game starts. Rooms that are not listed cannot be reached with room_goto_next.`

## Controls

| Control | Action |
| --- | --- |
| List | Rooms in order, as `1  room_name`. Names show nicknames when set. |
| **Up**, **Down** | Move the selected room one place. Disabled at the ends. |
| **Remove** | Take the selected room out of the order. The room itself stays in the file. |
| Room box and **Add room** | Add a room that is not in the order yet, at the end. The box lists only rooms not yet listed. |

Each change is one undo step. The window follows undo, redo and nickname changes. If a change is refused, the reason shows in the message line at the bottom of the window.

To run the game from a particular room without changing the order, use **Room** > **Play This Room** (`Ctrl+Shift+F5`). See [Rooms](rooms.md).

## See also

- [Rooms](rooms.md)
- [Resource management](resources.md)
