# Property list

Every resource tab ends with a property list that shows the fields of the resource as stored in the data file. This page explains how to read and edit it.

## Layout

A resource tab has an optional editor on top (the room canvas, the object events, the path editor and so on) and the property list below, divided by a splitter you can drag. The height you give is remembered. Resources without their own editor show only the property list.

Above the rows a box with the placeholder `Filter properties` narrows the list. Text matches the field path and the value, ignoring case. While you filter, group headings fold on their own and every match is shown.

Rows come in these kinds:

| Kind | Editor |
| --- | --- |
| Text, whole number, real number | Text box. Number boxes stay narrow. |
| Yes or no | Check box. |
| Colour | A swatch that opens red, green and blue sliders, plus a box with the number. The change is committed when the popup closes. The top byte of the value is kept. |
| Choice | Drop-down list. |
| Flags | One check box per flag. |
| Reference to another entry | Link plus buttons, see below. |
| Group | A heading that folds (`▾` open, `▸` closed). Click the heading. |
| List | A heading with the number of items (`Items: {count}`, or `No items`). Lists start closed because they can hold thousands of rows. At most 50 entries of a list are shown. |
| Information | Grey text. Not editable. |

Labels use the app's own text where it has one, else the field name split into words. Nesting goes down three levels at most.

## Editing

Type a value and press `Enter` or leave the box. The change is committed as one undo step. If the field cannot hold the text, the message line shows `{path}: {reason}` and the old value stays. Strings of the data file's string table are never changed in place, because other entries may share them: the editor picks or adds a table entry instead.

Clearing **Debugger Disabled** (turning the debugger on) first asks for confirmation, because it can make the normal runner hang. The prompt can be silenced in Settings (**Turn the GameMaker debugger flag on**).

If the resource is locked (Inspector: **Read only**), every row is read-only.

## References

A reference field (the sprite of an object, a parent, the sprite of a room background) shows:

- a small picture where the target has one;
- the target name as a link. Click it to open the target in a tab. Middle click opens it in a tab of its own. Hover shows a card. The nickname is shown when one is set;
- **Set...** or **Change...**, which opens the picker: a searchable list of every entry of that type with pictures. Type in `Search...`, press `Down` to move to the list, `Enter` or click to pick. At most 2000 rows are listed. `Nothing matches.` shows when the search finds nothing;
- a cross (**Clear the link**) that sets the field to none. It appears only when the field has a value;
- a drop target: drag an item from the Explorer onto the row to set it.

Right click the link for **Find Usages** and, for sprites, tile sets, fonts and texture page items, **Edit Image...**.

A link the data file library cannot set shows the tooltip `This link is read-only: the data file library does not let it be changed.`

Lists of references (a texture group's sprites, for example) have **Add entry**, which opens the picker, and **Remove**, **Up** and **Down** per entry.

## Lists

Lists of strings and plain values have **Add entry** and per-row **Remove**, **Up**, **Down**. Lists that other data points into by id (events, layers, frames, tracks, glyphs, tile data, instances, room order and similar) have no generic buttons. Those lists have their own editors or commands.

## Grouped fields

For objects, the physics fields (**Uses Physics**, **Sensor**, collision shape and the rest) sit in a **Physics** group that starts closed. For sprites the picture and mask fields sit in **Texture settings** and **Collision mask**. A filter shows them in place.

## Fields the editor above already covers

To avoid listing a field twice, the property list leaves out: the shader source texts (shaders), and the lists of points (paths), glyphs (fonts), emitters (particle systems), tracks (sequences) and moments (timelines).

## Troubleshooting

| Message | Meaning |
| --- | --- |
| `{name} was not found in this data file.` | The resource was deleted or renamed while the tab was open. |
| `{path}: {reason}` | The typed text does not fit the field. |
| `Nothing matches.` | The picker search has no hit. |

## See also

- [Resource management](resources.md)
- [Objects](objects.md)
- [Rooms](rooms.md)
- [Groups](groups.md)
