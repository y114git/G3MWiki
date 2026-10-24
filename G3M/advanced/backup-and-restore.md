# Backup and restore

## Back up your G3M setup

Close the game and let normal restoration finish. Close G3M, then copy the [data directory](data-directory.md) to another location.

This copy includes profiles and mods, shared settings, plugin data, themes, downloads, and game snapshots. Game installations and save folders outside the data directory require separate backups.

Use Profile Manager's export for a single Library setup. Use Game Versions for an installation snapshot. Neither is a complete substitute for backing up the data directory and game saves.

## Normal launch restoration

G3M backs up affected existing files and records files added by a modded session. In normal Launch mode, it restores matching session files after the game ends and removes additions that still match what G3M deployed.

If another process changes a tracked file, G3M does not overwrite it silently. It asks whether to restore anyway. **Force restore** puts G3M's backup copies back over the external changes. **Keep external changes** leaves the files as they are, deletes the session's backups, and lets the game launch. This includes changes made by a game, external editor, updater, or another modding tool.

**Launch and keep changes** and **Patching only** intentionally retain deployed files. Their confirmation is a choice to keep changes, rather than a request for normal post-exit restoration.

## Interrupted sessions

G3M checks pending session information on startup. It restores a recoverable session when the tracked files match the deployed state. If tracked files have changed externally, it asks the same question as after a normal session.

Choose **Force restore** only when you want the game files back in their state before the session, and copy any file you changed on purpose first. Closing the question counts as **Keep external changes**, so no file is overwritten. Attach the relevant log to a support request before replacing files by hand.

For a paid game's clean installation, its distributor's verification or reinstall feature can restore distributed game files. Back up saves and personal additions first; distribution verification is not a G3M profile backup.
