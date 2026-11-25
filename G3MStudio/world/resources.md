# Creating, copying, renaming and deleting resources

This page covers the commands that add, copy, rename, move and remove resources (objects, rooms, scripts and so on) in the open data file, and which resource types each command accepts.

## Resource types

| Type (Explorer heading) | New | Duplicate | Delete | Editor above the property list |
| --- | --- | --- | --- | --- |
| Objects | yes | yes | yes | Links, events and physics vertices. See [Objects](objects.md). |
| Scripts | yes | yes | yes | Property list only. See [Property list](property-grid.md). |
| Paths | yes | yes | yes | Path editor. See [Paths](paths.md). |
| Timelines | yes | yes | yes | Moment list. See [Timelines](timelines.md). |
| Sounds | yes | yes | yes | See [Sounds](../audio/README.md). |
| Rooms | yes | yes | yes | Room editor. See [Rooms](rooms.md). |
| Sprites | yes | yes | yes | See [Sprites](../graphics/README.md). |
| Shaders | yes | yes | yes | Source editor. See [Shaders](shaders.md). |
| Fonts | yes | yes | yes | See [Fonts](../graphics/README.md). |
| Sequences | yes | yes | yes | Sequence editor. See [Sequences](sequences.md). |
| Animation Curves | yes | yes | yes | Curve editor. See [Animation curves](animation-curves.md). |
| Particle Systems | yes | yes | yes | Preview and emitters. See [Particle systems](particles.md). |
| Extensions | yes | no | yes | Extension editor. See [Extensions](extensions.md). |
| Tile Sets | no | yes | yes | See [Tile sets](../graphics/README.md). |
| Others (texture page items, groups, strings, variables and so on) | no | no | no | Property list only. Functions and variables have their own delete rules. |

Other types, such as Code, Game, Texture Groups and Audio Groups, are shown as property lists and are covered elsewhere.

## New resource

1. Use **File** > **New** (one command per type in the File menu) or right click a type heading in the Explorer and choose the New command. The command is disabled when the data file has no section for that type; the notice reads `This data file has no section for {type}, so it cannot hold a new one.`
2. The window `New {type}` asks for `Name of the new {type}`. Press **Create**.
3. The resource is made, selected in the Explorer and opened in a tab.

The window for a new script has one more box, **Global script (runs at start, its functions can be called by name)**. It is checked by default. A global script gets the code entry `gml_GlobalScript_name` and is added to the list of scripts that run when the game starts. Unchecked, the script is a function body in `gml_Script_name`. The box writes its state to **Settings > Code editor** (`newScriptGlobal`), so the next new script starts the same way. A name is taken when either entry already exists.

What a new resource starts with:

| Type | Template |
| --- | --- |
| Room | 1024 by 768. One layer named `Instances` when the game's rooms use layers. |
| Sprite | One transparent frame of 32 by 32 pixels, origin at the top left, a rectangle collision box over the whole frame. The frame goes on a small texture page of its own in the first texture group. |
| Sound | One second of silence (WAV, embedded), in the first audio group. |
| Font | Family `Arial`, size 12, characters 32 to 127, no glyphs yet. Add glyphs and draw them as described in [Fonts](../graphics/fonts.md). |
| Shader | A pass-through GLSL ES and GLSL pair that draws the texture times the draw colour. The HLSL9 sources are empty. |
| Extension | Version `1.0.0`, no files, no options. |
| Object, script, path, timeline, sequence, animation curve, particle system | Empty. |

### Names

The name must be free. An unusual name (not letters, digits and underscores, or starting with a digit) is warned about once: `'{name}' is not a plain name (letters, digits and underscores, not starting with a digit). GML code cannot use it, and other tools may fail on it. Press the button again to use it anyway. Undo removes it until you save.` A name that is already taken gives `A resource or code entry named '{name}' exists already. With two of one name, the editor and delete reach only the first. Press the button again to use the name twice anyway. Undo removes it until you save.` The Settings risk option can allow or refuse these names without asking.

## Duplicate

**Edit** > **Duplicate Resource...** (also in the Explorer context menu). The window `Duplicate {name}` asks for `Name of the copy`.

The copy owns its events, layers, frames and code entries. For sprites, fonts and tile sets the pixels are copied too, so editing the copy does not change the original. Other links (the sprite of an object, its parent, texture group) point at the same assets as the original. With several resources selected, no name is asked: copies are named `name_copy` and the notice reads `Resources copied: {count}.` One undo step.

## Rename

**Edit** > **Rename...** (`F2`). The window has the new name, a nickname box (`Nickname (optional, shown instead of the name)`, leave empty to show the name) and, where it applies, **Also rename matching strings**. A preview lists the names and strings to change and the places that use the resource. A nickname must not equal the name or nickname of another resource.

## Move up and down

**Edit** > **Move Up** and **Move Down** swap the selected resource with its neighbour in its list. At the end of a list nothing happens. The order of the list is the order in the data file.

## Delete

**Edit** > **Delete Resource...** (also the Delete key in the Explorer and the context menu).

1. G3MStudio searches for every place that uses the resource.
2. If nothing uses it, the window `Delete {name}` says `Delete {name}? Undo brings it back until the file is saved.`
3. If something uses it, the window `Delete {name} anyway?` lists up to 8 places (`Places that use {name}: {count}`) and the rest as `... +n`. Confirm to go on.
4. The references are cleared first (object sprite and parent set to none, entries taken out of lists such as a texture group or the room order), then the resource is removed, as one undo step. Open tabs of the resource close.
5. A use in code cannot be cleared. G3MStudio then refuses and asks `Delete {name} and leave the code that uses it?`. If you confirm, the code keeps its stale reference.

Several resources: `Delete these resources ({count})?` lists the first 8, and `Places outside the selection that use them: {count}. Those uses are cleared or removed:` lists the uses. Uses inside the selection do not count.

The prompts can be silenced in Settings (**Delete a resource**, **Delete several resources**). See [Settings](../settings/README.md).

## Find usages from the resource side

- **Find Usages** (`Shift+F12`) lists the places that use the selected resource. Also available on any reference field in the property list (right click the link).
- **Go to Object** (`Ctrl+F12`) opens the object of the selected room instance. **Go to Sprite** (`Alt+Shift+F12`) opens the sprite of the selected instance, the tile set or sprite of the selected room layer, or the sprite of the object in front. In the Explorer, the right-click menu of an object has **Go to Sprite** too. Both are greyed out when there is nothing to go to. If the target no longer exists, the notice `{name} does not exist in this data file.` appears; with nothing to follow it says `Nothing here to go to.`
- **Find What It Uses** lists the assets the resource uses.
- **Find Unreferenced Assets** lists assets nothing uses. Assets the game finds by name while it runs (`asset_get_index`, a room started by name, the first room, an object created by name) may be listed although they are used. Exclude them before you remove anything.
- **Find Child Objects** lists the objects whose parent chain contains the selected object.

Result lines can be excluded (**Exclude selected**) or deleted (**Delete selected**). Messages: `No usages to show yet. Use Find Usages on a resource.`, `{name} is not used anywhere.`, `Assets that are not used anywhere: {count}.`

## Read-only resources

The Inspector shows **Read only** for the selected resource. While it is checked G3MStudio refuses edits of that resource. The data file itself is not write protected. A locked resource shows the notice `This entry is locked. Unlock it to change the link.` on its link buttons.

## See also

- [Property list](property-grid.md)
- [Rooms](rooms.md)
- [Objects](objects.md)
- [Groups](groups.md)
