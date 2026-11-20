# Running instances

G3M keeps one primary application instance. Opening the application again does not create an independent second modding session.

Import links are forwarded to the running window for confirmation. This keeps requests in the same application that owns the active launch and profile state.

Different application folders do not provide independent games or user data by themselves. Use profiles for separate selections and separate game installations when you need independent on-disk game states.

A second start connects to the running instance through a local socket named `g3m.single-instance-lock` and asks it to show its window, or hands it an import link. If the running window does not answer, the second start waits up to 30 seconds and then reports **Another G3M instance holds the startup lock.**

The `--force-start` option does not bypass this: it only skips the check for an already running game process at startup.

If a second launch appears to do nothing, check the taskbar and the existing G3M window before trying to remove any lock or session files. The lock file is `g3m.instance.lock` in the [data directory](data-directory.md).
