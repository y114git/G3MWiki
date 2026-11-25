# Running instances

G3M keeps one primary application instance. Opening the application again does not create an independent second modding session.

Import links are forwarded to the running window for confirmation. This keeps requests in the same application that owns the active launch and profile state.

Different application folders do not provide independent games or user data by themselves. Use profiles for separate selections and separate game installations when you need independent on-disk game states.

A second start connects to the running instance through a local socket named `g3m.single-instance-lock` and asks it to show its window, or hands it an import link. If a running window accepts the connection but does not take the request, the second start reports **Another G3M window is open but did not answer.** If the startup lock stays held and no window answers, the second start waits up to 30 seconds and then reports **Another G3M instance holds the startup lock.**

The `--force-start` option does not bypass this: it only skips the check for an already running game process at startup.

## Command line

| Command | Effect |
| --- | --- |
| `G3M --force-start` | Skips the check for a game process that is already running at startup. |
| `G3M g3m://...` or `G3M deltahub://...` | Passes an import link. A running G3M asks for confirmation. Link formats are in [One-click install links](one-click-install.md). |
| `G3M --shortcut <config>` | Runs a saved desktop launch configuration. Desktop launch files pass this option for you. See [Shortcuts](../features/shortcuts.md). |

If a second launch appears to do nothing, check the taskbar and the existing G3M window before trying to remove any lock or session files. The lock file is `g3m.instance.lock` in the [data directory](data-directory.md).
