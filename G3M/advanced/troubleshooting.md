# Troubleshooting

Identify the stage that fails before changing settings: startup, download, import, application, game startup, or restoration. Preserve the relevant error and log.

## G3M does not start

Check the [OS and architecture requirements](../getting-started.md) and keep release files together. The Linux builds need glibc 2.38 or newer (Ubuntu 24.04, Debian 13, Fedora 39); on older systems G3M does not start.

A startup failure shows a dialog titled **G3M** and is also appended to `g3m-startup-error.log` in the system temporary folder (readable by its owner only) and to `logs/g3m.log`. Each file is written only when its location is writable.

| Message | Cause and action |
| --- | --- |
| Failed to initialize: ... | The data folder, log file, or temporary cleanup failed. Check the text after the colon: usually a read-only or full drive. Fix it or choose another [data location](data-directory.md). |
| Another G3M window is open but did not answer. | A running G3M does not respond. Close it from the task manager and start again. |
| Another G3M instance holds the startup lock. | A second start waited 30 seconds for the first one. Wait for it to finish starting or close it. |
| Could not prepare the single-instance lock: ... | G3M cannot create the data folder. Check permissions and free space. |
| Could not acquire the single-instance lock: ... | The lock file `g3m.instance.lock` cannot be written. Check the folder permissions. |
| G3M data location unavailable | The custom data folder is missing or not writable. Choose **Try again** after reconnecting the drive, **Choose another folder**, or **Use default location**. |
| Dialog titled **Startup Error**, message "An error occurred during startup: ..." | The main window failed to build. Send `logs/g3m.log`. |
| "An error occurred during startup: Main window did not become visible." | The startup watchdog gave up before the window appeared. Send `logs/g3m.log`. |

`G3M --force-start` skips only the check for an already running game process at startup.

Check whether G3M is already running. Do not delete session state to force a second instance. If a custom appearance asset or plugin is implicated, back up the data directory before making local changes.

## A download fails

Read the Downloads record. Check the source URL, network connection, destination write access, and free space. A 404 needs an available file URL; a 429 needs time before retrying; a certificate failure should not be solved by turning off certificate verification.

## A package needs manual setup

The archive may lack supported configuration. Use **Continue Setup** on the Downloads item, or [Manual Mod Installation](../mods/importing.md#files-without-configuration), select the game, and assign an action to each file or folder. Use **Skip** for bundled files that should not be installed. A game web page is not a download archive.

A patch verification warning allows **Save** or **Cancel**. Check the selected destination and required game version before saving anyway. If a save button is disabled, hover over it for the reason. Incomplete choices and an active check or save still prevent saving.

For an invalid `mod_config.json`, compare the error with the [configuration reference](../mods/mod-config.md). Check required fields, source existence, destinations, group names, and relationships.

## A patch fails

If the message says **The patch ... does not fit the game file**, the patch was made for another game version, or for an original file that another mod already changed. Put mods that patch the same file into one step, or install the matching game version. Use the original game version required by the mod. Binary patches often need exact original bytes. Check that another mod is not in the same step when the patch expects its already-applied result.

Run Diagnostics with a smaller selection. Verify G3MTool and xdelta paths if overridden, and inspect the patching log for the first tool error rather than just the final failure message.

## The game does not start

Test with no mods selected. Check the installation folder, executable override, Steam configuration, and Wine or PortProton for Windows-game execution on Linux. If normal launch works, test each mod alone and add the others back gradually.

## Launch remains unavailable

A supported game process can still be running, or restoration can still be finishing. Read the status message. Close the game normally and wait for cleanup rather than editing its files during restoration.

## Restoration reports external changes

G3M asks you to choose **Force restore** or **Keep external changes**. Keeping external changes never overwrites your files and lets the game launch. Back up game files you changed on purpose before choosing **Force restore**. See [backup and restore](backup-and-restore.md).

## Report a reproducible problem

Provide the installed G3M version, OS and architecture, selected game and game release, mod versions and step order, reproduction steps, and the relevant log. A [Diagnostics report](../features/diagnostics.md) or [support package](../features/support-packages.md) can supply additional evidence.

Use [GitHub issues](https://github.com/y114git/G3M/issues) or the community help links. Review shared files for personal information and copyrighted game content before attaching them.
