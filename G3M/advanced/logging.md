# Logs

Open the log viewer from **Windows**. Its application, patching, and conflict views show the relevant log stream. Select current output or an available archived log in the history control.

Use the folder action to open the log directory and the export action to save the selected output. The live view follows output when you are at the bottom; scrolling upward lets you inspect earlier messages.

| Path under the data directory | Purpose |
| --- | --- |
| `logs/g3m.log` | Current application session. It is rewritten at every start. |
| `logs/g3m/g3m_<YYYYMMDD_HHMMSS>.log` | Copy of the previous `g3m.log`, made at start when it was not empty |
| `logs/shortcut.log` | Desktop shortcut runner, rewritten at every shortcut launch |
| `logs/patching.log`, `logs/conflicts.log` | Read by the **Patching** and **Conflicts** views when present. G3M does not create them itself, so those views normally show "No patching log file found yet." Archives for them are read from `logs/patching/` (`patching_*.log`, `conflicts_*.log`). |

## Format

Each line is `<date time> <LEVEL> <logger name>: <message>`, for example `2026-11-04 12:00:00,000 INFO services.launch_service: ...`. The file records INFO and above; the console shows WARNING and above. Qt warnings are included as `Qt message: ...`, and crash diagnostics are appended after a `crash diagnostics start` marker.

A failure caused by the machine, files, or network (a missing file, a damaged archive, a refused connection) is written as one line: the message, the error type and text, the G3M source file and line in brackets, and the cause if any. A traceback is kept only for failures that point at a bug in G3M and for critical records. Set the environment variable `G3M_LOG_TRACEBACKS` to any value to keep full tracebacks for every failure.

Entries from Remote Install name the type and IDs of queue entries, never the key.

For a failure, include the error and the preceding operation messages. A final generic failure line alone may omit the missing source or tool error that explains it.

Logs can contain local paths and mod names. Review exported text before sharing, or use [Support Packager](../features/support-packages.md) for selectable diagnostic collection and recognized-value redaction.
