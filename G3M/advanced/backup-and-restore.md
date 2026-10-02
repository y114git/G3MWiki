# Backup and restore

## Back up your G3M setup

Close the game and let normal restoration finish. Close G3M, then copy the [data directory](data-directory.md) to another location.

This copy includes profiles and mods, shared settings, plugin data, themes, downloads, and game snapshots. Game installations and save folders outside the data directory require separate backups.

Use Profile Manager's export for a single Library setup. Use Game Versions for an installation snapshot. Neither is a complete substitute for backing up the data directory and game saves.

## Normal launch restoration

G3M backs up affected existing files and records files added by a modded session. In normal Launch mode, it restores matching session files after the game ends and removes additions that still match what G3M deployed.

If another process changes a tracked file, G3M avoids overwriting that changed file and reports the conflict. Recovery material is retained for inspection. This includes changes made by a game, external editor, updater, or another modding tool.

**Launch and keep changes** and **Patching only** intentionally retain deployed files. Their confirmation is a choice to keep changes, rather than a request for normal post-exit restoration.

## Interrupted sessions

G3M checks pending session information on startup. It restores a recoverable session when the tracked files match the deployed state. If tracked files have changed externally, it retains recovery data under `patching_backups/recovery_conflicts/` and leaves the changed files for review.

Keep that directory and its accompanying session information if you need manual recovery. Do not delete it to clear an error. Compare the recorded target paths and backup copies, or attach the relevant log to a support request before replacing files.

For a paid game's clean installation, its distributor's verification or reinstall feature can restore distributed game files. Back up saves and personal additions first; distribution verification is not a G3M profile backup.
