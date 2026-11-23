# Tabs

Each open resource or code entry has a tab in the editor area. This page explains preview tabs, pinning, closing, reopening, the two kinds of back and forward, and splitting the editor area into groups.

## Opening a tab

| Action | Result |
|---|---|
| Single click on an entry in the Explorer | Opens a preview tab (title in italics). |
| Double click, `Enter`, or middle click on an entry | Opens a normal tab. |
| **Go to...** (`Ctrl+T`), **Go to Resource...** (`Ctrl+P`) | Opens a normal tab. `Shift+Enter` opens it in a split beside the tab in front. |

If the entry already has a tab, that tab comes to the front. A normal open turns an existing preview tab into a normal tab.

### Preview tab

A preview tab holds the entry you looked at last. A new preview tab replaces the preview tab of the same group, so browsing the Explorer does not fill the strip. A preview tab becomes normal when you open the same entry normally, or when you choose **Keep Tab Open**.

## Tab menu

Right-click a tab. The menu holds the tab commands, then the resource commands for what the tab shows (see [Explorer context menus](explorer-context-menus.md)).

| Item | Shortcut | Effect |
|---|---|---|
| **Close Tab** | `Ctrl+W` | Closes the tab. |
| **Close Other Tabs** | | Closes every tab in the window except this one and pinned tabs. |
| **Close All Tabs** | `Ctrl+Shift+W` | Closes every tab that is not pinned. |
| **Reopen Closed Tab** | `Ctrl+Shift+T` | Reopens the tab closed last whose resource still exists. Up to 100 closed tabs are remembered. |
| **Pin Tab** | | Pins the tab. Pinned tabs sit first in the strip and survive **Close Other Tabs** and **Close All Tabs**. |
| **Keep Tab Open** | | Turns a preview tab into a normal tab. |
| **Split Right** | `Ctrl+\` | Moves the tab into a new group on the right. |

A tab that holds an image with changes waiting for **Apply** asks before it closes. A tab closes with a short fade. **Window > Close All Windows** closes every tab, pinned ones included.

When a resource is renamed its tabs follow the new name, and follow back on undo. When a resource is deleted its tabs close.

## Moving between tabs

| Command | Shortcut | What it does |
|---|---|---|
| **Next Tab** | `Ctrl+Tab` | Next tab in the group, wrapping around. |
| **Previous Tab** | `Ctrl+Shift+Tab` | Previous tab in the group. |
| **Previous Tab (History)** | `Ctrl+Alt+Left` | The tab that was in front before this one. Also the left button at the left of the menu bar. |
| **Next Tab (History)** | `Ctrl+Alt+Right` | The tab that was in front after this one. Also the right button. |
| **Back** | `Alt+Left` or mouse back button | Shows the resource this tab showed before, as in a browser. |
| **Forward** | `Alt+Right` or mouse forward button | Goes forward again. |

**Back** and **Forward** are kept per tab: each tab remembers the resources it showed (up to 100). Resources that are gone, or that another tab shows, are skipped. A new visit drops the forward part. A preview tab that replaces another continues its history.

The two history commands follow the order in which tabs were in front, whatever their position. They are off when no earlier or later tab is open. Their buttons can be hidden or moved to the right of the menu bar in Settings (see [Appearance settings](../settings/README.md)).

## Tab groups

The editor area holds up to 4 groups. **Split Right** needs at least two tabs in the group. Drag a tab onto a dock marker to split, onto the empty part of another strip to join it, or off the window to float it. A preview tab replaces the preview of its own group only.

When no tab is open the editor area says "Nothing open. Press Ctrl+P to find a resource, or double-click one in the Explorer."

## Window title

The title shows the active tab first: see [Main window](main-window.md).

## See also

- [Explorer](explorer.md)
- [Docking and layouts](docking-and-layouts.md)
- [Command reference](command-reference.md)
