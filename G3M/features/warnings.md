# Manage Warnings

Open **Settings > Game > Patching / Merging > Manage Warnings** to choose which warning prompts interrupt an operation. Checked entries keep their prompts enabled. Click **OK** to save the choices, or **Cancel** to discard edits to the dialog.

The **Critical**, **Major**, and **Minor** headings group the entries and collapse or expand their lists. The question-mark control beside an entry explains that warning.

| Warning | Reason for the prompt | What to check |
| --- | --- | --- |
| XDELTA patch failed | A binary patch could not apply to the selected DATA. | The exact original game file and version required by the mod. |
| G3MPatch apply failed | A resource patch could not apply cleanly. | The game version, original resource state, and preceding mod steps. |
| Merge failed | G3MTool could not combine the selected inputs. | The merge log or report and the smallest selection that reproduces the failure. |
| Steam may launch a different game folder | Steam can start an installation other than the folder G3M patched. | Whether the configured game path matches Steam's installation. |
| Other patching warning | An operation reports a patching or merging risk. | The specific details in the prompt and operation log. |
| Mod uses direct absolute paths | A mod names machine-specific filesystem locations. | Whether those locations are intended and whether you trust the mod's author. |
| Required mods are missing | A selected mod needs mods that are not installed, or installing them did not work. | The mod's requirements on its page, and whether the required mods exist for the selected game. |
| Required mods are not active | A selected mod needs installed mods that are not in the selection. G3M offers to activate them. | Whether you want those mods in this launch. |
| Mod order can be improved | The order of the selected mods differs from the order their authors recommend. | The proposed order, then apply it, keep yours, or cancel. |
| Mods need a newer G3M | An installed mod uses a newer mod format than this G3M reads, so it is missing from the Library. | Update G3M, then check the Library again. Launching anyway runs without that mod. |

**Skip All Warnings** suppresses these prompts together. The individual choices become unavailable while it is enabled. This option can let a workflow continue after an incomplete patch or failed merge, so the game can start even though some selected changes were not applied.

These preferences control warning prompts. Configuration validation, missing required inputs, and other errors can still stop an operation. Keep the relevant prompt enabled while investigating a failure; use [Diagnostics](diagnostics.md) and [logs](../advanced/logging.md) to inspect it.
