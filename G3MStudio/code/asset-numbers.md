# Replace Numbers with Asset Names

Older GameMaker data files (before 2023.8) store the sprites, objects and other resources that code uses as plain numbers, so the decompiled code reads `draw_sprite(5, 0, x, y)`. This feature writes the asset name instead (`draw_sprite(spr_player, 0, x, y)`), for one entry or for all code.

## What counts as an asset number

A whole number is replaced when it is:

- the whole of an argument of a function that takes an asset of some type there. Built-in functions come from the bundled reference. A parameter that takes a script counts for `script_execute`, `script_execute_ext`, `script_exists` and `script_get_name`. Functions of the game itself are learned from calls that already pass an asset name: if the game calls `scr_play(snd_item)` and `scr_play(snd_coin)`, the first argument of `scr_play` is known to be a sound. At least two such calls are needed. An argument that received two asset types is left alone.
- the whole right side of `=`, `==` or `!=` for a built-in variable that holds an asset (for example `sprite_index = 5;`), when a `;`, `)`, `,`, `&&`, `||` or `}` follows. The variable is the plain name, or after `self.` or `other.`. A declaration with `var`, `static` or `globalvar`, and a variable after another dot (`global.room = 3`), are left alone.

Only plain whole numbers count. Hex numbers, numbers with a decimal point and numbers inside an expression are not changed. A number is also left alone when no asset of that type exists at that index.

## Settings

In **Settings > Code editor**:

| Option | Default | Meaning |
|---|---|---|
| Numbers that are assets (draw_sprite(5), sprite_index = 3) | Offer in the menu and by command | **Do nothing**: the code context menu has no entries for it. The commands in the **Code** and **Edit** menus still work. **Offer in the menu and by command**: the context menu entries and the commands work. **Show names when an entry opens**: as the previous choice, and a code tab shows names right after decompiling. |
| Also learn the functions of the game itself | on | The all-code run reads every entry once first, to learn which game functions take assets. |
| Asset types to replace | all ten | Sprites, Objects, Rooms, Sounds, Paths, Fonts, Shaders, Scripts, Tile Sets, Particle Systems. Unchecked types are never replaced. |

With **Show names when an entry opens**, the shown text is not an edit. The entry stays unchanged until you edit it, and saving then writes the names.

## Replace in the open entry

1. Put the caret in the code tab. To limit the change, select the lines.
2. Run **Replace Numbers with Asset Names** from the **Code** menu, the context menu or the command palette. It has no default key. It changes the selection, or the whole entry when nothing is selected. To change one number, right-click it and choose **Replace with {name}** (for example "Replace with spr_player"), the first item of the menu.
3. The names appear in the text as one undo step. Press **Save changes** to apply.

The command does nothing in a read-only tab.

## Replace in all code

1. Choose **Edit > Replace Numbers with Asset Names in All Code...**.
2. Confirm the question (titled "Replace Numbers with Asset Names in All Code"): "Write asset names instead of numbers in every code entry that passes a number where a function takes a sprite, object, room, sound, path, font, shader, script or tile set? Entries that do not compile afterwards are left as they are. One Undo takes it all back."
3. A progress window shows the work (reading entries, then writing).
4. A notice reports "Wrote asset names for {numbers} numbers in {entries} code entries." or "No numbers in the code stand for assets."

Entries whose new text does not compile stay as they were and a warning lists up to eight of them: "Left unchanged because they did not compile afterwards: {entries}". One **Edit > Undo** takes back the whole run.

## See also

- [Code Editor](code-editor.md)
- [GML Language Support](gml-language.md)
- [Dead Resources](dead-resources.md)
