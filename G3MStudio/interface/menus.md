# Menus

This page lists every menu of the menu bar with its items in order. Each item is also a command with an ID, so it can be found in the command palette and given another shortcut; the IDs and default keys are in the [Command reference](command-reference.md).

## How the menus behave

- An item is greyed out when its command does not apply. For example most items are off until a data file is open. The tooltip of an item shows its name and shortcut, also while it is off.
- A **New** item that stays off while a file is open means the data file has no section for that type. Its tooltip says: "This data file has no section for {type}, so it cannot hold a new one."
- The menus **Sprite**, **Sound**, **Font**, **Tile Set**, **Room**, **Image** and **Code** show only while a tab or the Explorer selection of their kind is in front. They sit between **Project** and **Run** in the bar.
- **Plugins** shows only when a plugin has added a command. It sits before **Layout** and **Help**.
- Some commands have no menu item because another place holds them: the tab commands (right-click a tab), **Move Run Options to G3MStudio's Folder** and **Ask Again for All Prompts** (buttons in Settings). The palette and the shortcut editor still list them.
- The transfer commands (import, copy, export) have one entry, **Transfer Resources...**. The single commands stay in the palette and the Explorer context menu.
- Two submenus are not commands: **Open Recent** (File) and **Recent Projects** (Project).

## File

| Item | Notes |
|---|---|
| New Data File... | Asks where to save the new file. |
| New (submenu) | New Object..., New Script..., New Path..., New Timeline..., New Sound..., New Room..., New Sprite..., New Shader..., New Font..., New Sequence..., New Animation Curve..., New Particle System..., New Extension... |
| Open... | Opens a data file. |
| Open Recent | Last 10 files. Last entry **Clear Recently Opened**. Shows "No recent files." when empty. |
| Transfer Resources... | One window for import, export, copy and mod export. |
| Save, Save As..., Save a Copy..., Revert to Saved... | See [Saving](../projects/README.md). |
| Settings | Opens the Settings page. |
| Close | Closes the data file. |
| Exit | Closes the window. |

## Edit

Undo, Redo, Rename..., Duplicate Resource..., Move Up, Move Down, Copy Name, Add to or Remove from Favourites, Add Tag..., Delete Resource..., Select All, Find, Find in Code..., Replace in Code..., Find Usages, Find What It Uses, Find Unreferenced Assets, Find Child Objects, then Go to Object, Go to Sprite.

Replace Numbers with Asset Names in All Code... comes after Replace in Code.... **Go to Object** (`Ctrl+F12`) and **Go to Sprite** (`Alt+Shift+F12`) are on when the thing in front leads somewhere (a selected room instance, an object, a layer or a sprite element). Otherwise a toast says "Nothing here to go to." See [Command palette and Go To](command-palette-and-go-to.md#go-to-object-and-go-to-sprite).

Separators fall after Redo, after Select All, after Replace Numbers with Asset Names in All Code..., after Find Child Objects.

## Organize

New Folder..., Move to Folder..., Add to Another Folder..., Cut, Paste into Folder, Add to Collection..., Remove from Folder, Collection or Tag, Set Colour..., then Rename with Pattern..., Find Dead Resources..., Move to Texture Group..., Move to Audio Group..., then Select All of This Type, Select Everything in the Group, then Switch Folder View, Open Folder, Up One Folder. See [Explorer](explorer.md) and [Folders, collections and tags](explorer-organizing.md).

## View

Command Palette..., Go to Resource..., Go to..., Reveal in Explorer, then the **Panels** submenu (Explorer, Search, Inspector, Problems, Output, Usages, Function Reference, History, Git), then Zoom In, Zoom Out, Reset Zoom, Toggle Full Screen, then Focus Next Region, Focus Previous Region.

## Project

Open Project..., Recent Projects, Close Project, then New Project..., Enable Project, then Sync to Data File, Resolve Outside Changes..., then Project Settings..., Change Data File Location..., Disable Project.... **New Project...** and **Enable Project** work only while no project is linked; the others only while one is. See [Projects](../projects/README.md).

## Sprite

Visible with a sprite in front. Edit Image..., Add Sprite Frame, Duplicate Sprite Frame, Delete Sprite Frame, Replace Sprite Frame..., Import Sheet or Strip..., Resize Image/Canvas..., then Play or Pause Sprite, Loop Sprite Playback, Onion Skin, Lock Origin, then Recalculate Collision Mask, Import Mask..., Export Mask..., Export as GIF.... See [Sprites](../graphics/README.md).

## Sound

Visible with a sound in front or selected. Edit Audio..., Replace Audio..., Generate Sound..., Record Sound..., Export Audio..., Export All Audio..., then Play or Pause Sound, Stop Sound, Loop Sound. See [Audio](../audio/README.md).

## Font and Tile Set

**Font**: Edit Image..., Replace Font File.... **Tile Set**: Edit Image....

## Room

Visible with a room in front. Play This Room, then Room Creation Code..., Instance Creation Order..., Export Tiles as CSV..., Import Tiles from CSV..., Convert Image to Tile Layer..., Save Room as PNG..., then Toggle Auto-Tile Brush, Re-Tile Selection, then Add Layer, Duplicate Layer, then Show or Hide Grid, Snap to Grid On or Off, Fit Room to Window, Zoom to 100%. See [Rooms](../world/README.md).

## Image

Visible with the image editor in front. All items are image editor commands. Submenus, in order of first appearance:

| Submenu | Items |
|---|---|
| Tools | Pencil, Eraser, Replace Colour, Remove Colour, Fill, Line, Rectangle, Rectangle (Filled), Ellipse, Ellipse (Filled), Polygon, Polygon (Filled), Curve, Text, Colour Picker, Rectangle Select, Lasso Select, Magic Wand, Move Selection, Hand (Move View) |
| Brush and colour | Bigger Brush, Smaller Brush, Swap Primary and Secondary Colour, Smooth Drawing |
| View | Grid, Mirror Left-Right, Mirror Top-Bottom, Tile Preview, Onion Skin, Zoom In, Zoom Out, Fit, Actual Size (1:1) |
| Frames | Previous, Next, Duplicate Frame, Delete Frame, Play |
| Selection | Select All, Select None, Invert Selection, Expand or Shrink Selection..., Copy, Cut, Paste, Clear, Fill Selection, Stroke Selection... |
| Transform | Flip Horizontally, Flip Vertically, Rotate 90 Degrees Clockwise, Rotate 90 Degrees Anticlockwise, Rotate by Angle..., Resize Image..., Skew... |
| Adjust | Hue, Saturation, Brightness, Contrast..., Invert Colours, Posterize..., Greyscale..., Dither..., Outline with Primary Colour, Blur... |
| Canvas | Resize Canvas..., Crop to Selection, Trim to Content |
| Layers | New Layer, Delete Layer, Duplicate Layer, Move Layer Up, Move Layer Down, Merge Layer Down, Show or Hide Layer, Lock or Unlock Layer, Rename Layer |

Two items stay at the top level: **Discard Image Edits** and **Go to Original Resource**. See [Image editor](../graphics/README.md).

## Code

Visible with a code editor in front. Groups, separated by lines:

1. Go to Definition, Peek Definition
2. Parameter Hints, Quick Fix, Insert Snippet
3. Toggle Line Comment, Toggle Block Comment, Replace Numbers with Asset Names, Indent Selection, Outdent Selection, Duplicate Line, Move Line Up, Move Line Down
4. Toggle Bookmark, Next Bookmark, Previous Bookmark, Next Problem, Previous Problem
5. Go to Symbol in File, Show Changes Against Original, Help on Word Under Caret, Open in Manual
6. Go to Line..., Go to Matching Bracket, Expand Selection, Shrink Selection
7. Delete Line, Join Lines, Sort Selected Lines, Trim Trailing Whitespace, Convert to Upper Case, Convert to Lower Case, Convert to Title Case

See [Code editor](../code/README.md).

## Run

Run Game, Restart Game, Run with Runtime..., Stop Game. See [Run the game](../projects/README.md).

## Mods

Create Patch..., Apply Patch..., Apply Mod..., Merge Patches..., Validate Patch..., then Compare Data Files..., Apply Mod Pack..., Remember This File as the Mod Base, Clear the Mod Base Cache.

## Tools

Strings..., Room Order..., Texture Groups..., Audio Groups..., then Hex View of Data File..., Write Chunk Offset Map, Export Raw Chunk..., Replace Chunk from File..., Write Debug Data..., Disassembly of Code Entry..., then Repack Texture Pages..., Export Texture Page..., Replace Texture Page..., Resize Texture Page..., then Run Script..., Run C# Command..., Plugins....

## Window

Next Tab, Previous Tab, Previous Tab (History), Next Tab (History), Back, Forward, then Show All Windows, Close All Windows. See [Tabs](tabs.md).

## Layout

G3MStudio Layout, Wide Editor Layout, Code Layout, Canvas Layout, Minimal Layout, then Save Layout..., Saved Layouts..., Reset Layout, then Hide or Show All Docks, Fold or Open Left Dock, Fold or Open Right Dock, Fold or Open Bottom Dock. The items are listed flat. See [Docking and layouts](docking-and-layouts.md).

## Help

Getting Started..., Interface Tour..., Documentation, Check for Updates..., then Send Feedback..., Report a Problem..., Copy Diagnostics, then Open Data Folder, Open Logs Folder, then About G3MStudio.

## See also

- [Command reference](command-reference.md)
- [Command palette and Go To](command-palette-and-go-to.md)
- [Main window](main-window.md)
- [Dialogs and windows](dialogs-and-windows.md)
