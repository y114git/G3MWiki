# DR Save Manager

DR Save Manager manages DELTARUNE's main save slots and additional named collections. It includes a save editor. Install and enable the plugin in **Settings > Catalog > Plugins**, then open its main panel.

This guide covers the G3M plugin's controls. It does not document DELTARUNE's story flags or recommend values for a particular route.

## Select the save folder

Use **Change saves folder** if automatic detection does not find the intended DELTARUNE saves. The plugin also exposes **Save folder** in its catalog settings. Select the folder containing saves, not the game installation or an individual save file.

The panel has chapter tabs for Chapters 1 through 5 and shows the three main slots for the chosen chapter. A slot reports available player information and completion status, or shows that it is empty. Its contents come from the selected save folder.

## Collections and copying

**Additional slots** opens collection storage. Create a named collection when prompted, then use the collection navigation controls to select it. Collections hold chapter save files alongside your main slots.

| Control | Action |
| --- | --- |
| Copy from main | Copy main-slot saves into the selected collection. |
| Overwrite main | Replace main-slot saves with the selected collection's saves. |
| Rename collection | Change the collection's displayed name. |
| Delete collection | Remove the collection and its saved files after confirmation. |
| Show | Open the selected save for viewing. |
| Edit | Open the selected save in the editor. |
| Erase | Delete the selected save after confirmation. |
| Import | Read a save from an external file, folder, or another collection, as offered by the dialog. |
| Export | Copy the chosen save to an external destination. |

Copy dialogs identify the destination and whether the action affects a selected slot or all three slots. When offered, **Only Current Chapter** limits the copy to that tab; **All Chapters** includes every supported chapter. Read the overwrite confirmation before replacing main saves.

Export an independent copy before erasing a slot, replacing main saves, or deleting a collection. A G3M profile export does not back up saves in the game's user-data folder.

## Edit a save

**Simple mode** presents named fields organized by their use, including player information, party, inventory, battle values, and chapter-specific story or customization fields. Available controls depend on the chapter and recognized save format.

**Advanced mode** displays the save's rows and values. Use **Show row details** to expose row information. **Flags** has a search field; the **Manual Flag Editor** selects a flag ID and value, then applies it to the edited save.

**Undo** and **Redo** move through edit history. **Save** shows a changed-field summary and writes to the original save file after confirmation. **Cancel** discards unsaved changes after confirmation when needed.

Simple mode is unavailable when the editor cannot recognize the save's structure. A UTF-8 decoding error prevents loading. Switching modes does not repair an invalid save or establish that a combination of story values is playable.

Some numeric values are valid file contents but inconsistent with the game's current state. Keep a separate copy and test edits in the intended chapter before replacing a working playthrough.

## Use a collection for a session

When the launch asks for **Save Collection**, choose **Main slots** to keep the normal saves or select a collection to substitute its saves temporarily. Cancelling this selection cancels that launch operation.

The plugin backs up existing main-slot files before substitution and restores them during session cleanup. Changes to already populated collection slots are not copied back when the session ends. Newly created saves are copied back for collection slots that were empty when the session started.

For a playthrough you intend to continue, back up your main saves, use **Overwrite main** to copy the collection into the main slots, and choose **Main slots** at launch. This lets the game keep its progress in those main saves. Use temporary collection sessions for tests where you do not need to retain changes to existing slots.

The [shortcut dialog](shortcuts.md) offers a collection choice when collections are available for DELTARUNE. It records a collection selection rather than copying saves into the shortcut. Keep the collection available and recreate the shortcut after changing the intended selection.
