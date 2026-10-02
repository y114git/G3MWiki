# Logs

Open the log viewer from **Windows**. Its application, patching, and conflict views show the relevant log stream. Select current output or an available archived log in the history control.

Use the folder action to open the log directory and the export action to save the selected output. The live view follows output when you are at the bottom; scrolling upward lets you inspect earlier messages.

| Path under the data directory | Purpose |
| --- | --- |
| `logs/g3m.log` | Main application session |
| `logs/patching.log` | Patching information where a separate stream is available |
| `logs/conflicts.log` | Conflict information |
| `logs/shortcut.log` | Desktop shortcut runner |
| `logs/g3m/` | Archived application sessions |
| `logs/patching/` | Archived patching output where available |

For a failure, include the error and the preceding operation messages. A final generic failure line alone may omit the missing source or tool error that explains it.

Logs can contain local paths and mod names. Review exported text before sharing, or use [Support Packager](../features/support-packages.md) for selectable diagnostic collection and recognized-value redaction.
