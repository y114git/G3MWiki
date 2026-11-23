# Projects settings

The **Projects** category holds the default autosave and autosync rules for new projects. Changing them also changes the open project. The page note reads: "These are the defaults for new projects; changing them also changes the open project."

## Controls

| Label | Control | Default | Allowed values | Saved as |
| --- | --- | --- | --- | --- |
| **Autosave to the project** | List and number | **After a delay**, 2 seconds | **Off**, **After a delay**, **When the window loses focus**, **On save**; 1 to 3600 seconds | `project.autoSave`, `project.autoSaveSeconds` |
| **Autosync to the data file** | List and number | **On save**, 30 seconds | the same four triggers; 1 to 3600 seconds | `project.autoSync`, `project.autoSyncSeconds` |

The seconds apply to the **After a delay** trigger. Triggers in the file are `Off`, `AfterDelay`, `OnFocusLoss`, `OnSave`.

Autosave writes changed resources into the project. Autosync writes the data file. They are separate settings. A saved answer to the prompt **Saving in a project (yes: also sync the data file, no: save progress only; a remembered answer overrides the autosync setting)** overrides autosync (see [Prompts](prompts.md)).

## Restore defaults

Resets the `project` object.

## See also

- [Project settings](../projects/project-settings.md)
- [Saving](../projects/saving.md)
- [Prompts](prompts.md)
