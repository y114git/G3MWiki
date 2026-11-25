# Launch modes

The arrow beside the main action button opens a menu that selects what G3M does with the applied mod files. The menu also holds the **Launch via Steam** checkbox and any options or actions added by plugins. The choice is saved and used by shortcuts.

| Menu item | Starts the game | Restores files when the game closes |
| --- | --- | --- |
| **Launch** | Yes | Yes |
| **Launch and keep changes** | Yes | No |
| **Launch patching only** | No | No |

The latter two modes ask for confirmation because they leave mod changes in place. The prompt is titled **Keep patched files?**, and its default answer is No. Use a separate game installation when producing a patched copy or testing persistent changes.

## Launch

With no selected mods, G3M starts the game without applying a mod selection. With mods selected, it backs up affected files, applies the selection, starts the game, and restores affected files after the monitored game session ends.

Let cleanup finish before starting another session. Launch remains unavailable while restoration is in progress, even if the main window is visible.

### Required mods and suggested order

G3M checks the dependencies and conflicts declared by the selected mods before applying them. The prompts distinguish installed requirements that are not selected from requirements absent from the profile.

- **Activate dependencies** adds installed requirements to the selection. **Launch without dependencies** keeps the current selection, and **Cancel** stops the launch.
- **Install and activate dependencies** downloads supported missing requirements from GameBanana and selects them after installation. **Launch selected mods** continues with the existing selection. Requirements that need manual setup are identified in the prompt and Downloads.
- **Apply recommendation** saves a suggested arrangement of selected mods when their dependency conditions can be satisfied by changing priority or steps. **Launch without changes** retains the current arrangement.

Automatic downloading applies to GameBanana IDs such as `gb_mod_12345` and `gb_wip_12345`. An arbitrary mod ID does not tell G3M where to obtain its package. Install those requirements yourself from the author's documented source.

If a download fails or needs manual setup, finish its installation in Downloads and check the selection before retrying. Continuing without a requirement can leave a mod incomplete even if its files apply successfully. A suggested order cannot resolve incompatible gameplay changes or conflicting requirements.

Mod authors define these conditions in [mod_config.json](../mods/mod-config.md#dependencies-and-conflicts). You can inspect and edit the selection in [Priority & Steps](../mods/modpacks.md).

## Launch and keep changes (**Launch and keep changes**)

G3M applies the selected mods and starts the game, then keeps their changes after exit. Use this mode only when you intend to keep the patched installation. A later launch starts from the files that are currently on disk.

## Patching only (**Launch patching only**)

G3M applies the selection without starting the game. The resulting files stay in the installation. This mode is useful for creating a patched copy to inspect or run separately.

## Chapter selection

In DELTARUNE **Chapter Mode**, sections have independent mod selections and steps. Double-click a chapter tab to turn direct launch on or off for it; the game then opens on that chapter instead of the main menu. Direct launch is not available for the Main Menu tab. Direct chapter launching and Steam launching cannot be combined for DELTARUNE.

Outside Chapter Mode, G3M uses the shared selection and applies each mod to the sections it supports.

## Launch options and plugins

Steam, Wine, and PortProton control how the game starts. They do not change which mods work together. Plugins can add launch options or their own actions to the menu; a plugin action follows the plugin's behavior rather than automatically running the standard launch.

Read [Priority & Steps](../mods/modpacks.md) when selecting multiple mods, and [restoration](../advanced/patching-process.md) before using a mode that keeps changes.

## Changed game files after a session

If game files changed after G3M applied the mods, for example because another program edited them, G3M does not overwrite them silently. A **Patching Warning** dialog offers **Force restore**, which restores the backup, and **Keep external changes**, which leaves the files as they are and removes the leftover backup. Closing the dialog keeps the files. Choosing **Keep external changes** does not block the next launch. If a restore fails, G3M reports **Could not restore game files from the previous mod session** and does not start the game; the recovery backup is kept. G3M clears the read-only flag on files and folders that Windows marked read-only before it restores them.
