# Desktop shortcuts

The **Shortcut** action saves the current game, profile, selected mods, section selections, and priority steps into a launcher file. Use it to start a chosen setup without selecting the mods again in the main window.

## Create a shortcut

1. Configure the game path and select the intended profile.
2. Select the mods and review **Priority & Steps**.
3. Set Chapter Mode, direct section launch, Steam, or PortProton where applicable.
4. Click **Shortcut** and review the summary.
5. Choose whether plugin actions are included, and fill in any controls contributed by plugins.
6. Save the launcher file.

Shortcut files use `.vbs` on Windows, `.command` on macOS, and `.sh` on Linux. G3M makes the Unix launcher executable. Run only shortcut files whose contents and origin you trust.

## What the shortcut depends on

The shortcut refers to the local G3M installation, profile, mod IDs, and installed plugin code. It contains launch choices rather than a portable copy of the mods or game.

Shortcuts support several mods within a step as well as several steps. The saved selections remain tied to the referenced mod IDs; editing those mods changes the files used by the shortcut. Removing a mod or moving the G3M executable can make the shortcut unusable.

Create another shortcut after changing the setup you want it to represent. For failures, inspect the shortcut log in the [logs directory](../advanced/logging.md).
