# Troubleshooting

Identify the stage that fails before changing settings: startup, download, import, application, game startup, or restoration. Preserve the relevant error and log.

## G3M does not start

Check the [OS and architecture requirements](../getting-started.md) and keep release files together. If a configured data drive is unavailable, use the startup choices to retry or select an accessible location.

Check whether G3M is already running. Do not delete session state to force a second instance. If a custom appearance asset or plugin is implicated, back up the data directory before making local changes.

## A download fails

Read the Downloads record. Check the source URL, network connection, destination write access, and free space. A 404 needs an available file URL; a 429 needs time before retrying; a certificate failure should not be solved by turning off certificate verification.

## A package needs manual setup

The archive may lack supported configuration. Use **Continue setup** or [Manual Mod Installation](../mods/importing.md#files-without-configuration), select the game, and assign an action to each file or folder. Use **Skip** for bundled files that should not be installed. A game web page is not a download archive.

A patch verification warning allows **Save** or **Cancel**. Check the selected destination and required game version before saving anyway. If a save button is disabled, hover over it for the reason. Incomplete choices and an active check or save still prevent saving.

For an invalid `mod_config.json`, compare the error with the [configuration reference](../mods/mod-config.md). Check required fields, source existence, destinations, group names, and relationships.

## A patch fails

Use the original game version required by the mod. Binary patches often need exact original bytes. Check that another mod is not in the same step when the patch expects its already-applied result.

Run Diagnostics with a smaller selection. Verify G3MTool and xdelta paths if overridden, and inspect the patching log for the first tool error rather than just the final failure message.

## The game does not start

Test with no mods selected. Check the installation folder, executable override, Steam configuration, and Wine or PortProton for Windows-game execution on Linux. If normal launch works, test each mod alone and add the others back gradually.

## Launch remains unavailable

A supported game process can still be running, or restoration can still be finishing. Read the status message. Close the game normally and wait for cleanup rather than editing its files during restoration.

## Restoration reports external changes

Keep `patching_backups/recovery_conflicts/` and the associated logs. Back up the changed game files before attempting manual recovery. See [backup and restore](backup-and-restore.md).

## Report a reproducible problem

Provide the installed G3M version, OS and architecture, selected game and game release, mod versions and step order, reproduction steps, and the relevant log. A [Diagnostics report](../features/diagnostics.md) or [support package](../features/support-packages.md) can supply additional evidence.

Use [GitHub issues](https://github.com/y114git/G3M/issues) or the community help links. Review shared files for personal information and copyrighted game content before attaching them.
