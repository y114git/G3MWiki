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

**Skip All Warnings** suppresses these prompts together. The individual choices become unavailable while it is enabled. This option can let a workflow continue after an incomplete patch or failed merge, so a successful launch afterward does not prove that all selected changes were applied.

These preferences control warning prompts. Configuration validation, missing required inputs, and other errors can still stop an operation. Keep the relevant prompt enabled while investigating a failure; use [Diagnostics](diagnostics.md) and [logs](../advanced/logging.md) to inspect it.
