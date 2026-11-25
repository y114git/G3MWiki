# Troubleshooting

This page indexes the failures users hit most, with the message text and the fix. Each area page has its own troubleshooting section for details.

## Start and install

| Message or symptom | Fix |
| --- | --- |
| SmartScreen blocks the program | **More info**, then **Run anyway**. |
| macOS refuses to open the app | Right-click **Open**, or `xattr -dr com.apple.quarantine G3MStudio.app`. |
| Linux: window does not appear | Install `libfontconfig`; use an X11 or Wayland session; `chmod +x G3MStudio`. |

See [Install](../getting-started/install.md).

## Opening files

| Message | Fix |
| --- | --- |
| Could not open '{name}': {reason} | The file is not a readable data file. Use a backup. |
| '{name}' is in use by another G3MStudio window. Finish there first. | Close it in the other window. |
| Only files from this computer can be opened. Drop a data file from a folder. | Drop a real local file. |
| Link not recognised. | Check the link form (see [Opening a data file](../getting-started/opening-files.md)). |
| Data file not found | Choose the file again; **Keep the saved layout and sizes for this file** keeps your layout. |
| Unsaved edits recovered: {count} | The last session ended without saving; the edits came back from the journal. |

## Updates

| Message | Fix |
| --- | --- |
| The update failed: the program folder cannot be written to; move G3MStudio to a folder you own | Move the program or update by hand. |
| The update failed: The update file did not match its checksum and was deleted. | Retry. |
| The last update could not be installed, so the old version was restored. Try Help > Check for Updates again. | Shown at the next start after a failed swap. Run **Help > Check for Updates...** again. |

See [Updating](../getting-started/updating.md).

## Saving

| Message | Fix |
| --- | --- |
| Could not save '{name}': the file is in use by {holder}. Your edits are kept. | Close the program that holds the file, then save again. |
| Could not save '{name}': the file is read only. Clear the read-only flag and save again. Your edits are kept. | Clear the flag in the file properties. |
| The drive is full, so the last change could not be saved. Free some space and try again. | Free space, or save to another drive. |
| {name} was changed by another program after G3MStudio opened or last saved it. Overwrite it with your version, or save your version as a copy? | Pick one. Overwrite replaces the other program's change. |
| The G3MStudio data folder could not be written, so crash recovery and remembered settings may be out of date. Your edits stay in memory: {reason} | Check the free space and the rights on the [data folder](../getting-started/user-data.md). Your edits stay in memory. |

See [Saving](../projects/saving.md).

## Plugins and mod base

| Message | Fix |
| --- | --- |
| '{name}' failed to start and was turned off: {error} | Read the error in **Tools > Plugins...**, fix the source, press **Reload**. |
| plugin.json needs id, name and version. | Add the three fields to `plugin.json`. |
| plugin.json sits directly in the plugins folder. Put the plugin files in their own folder there, or use Plugins > Install ZIP. | Move the files into a folder of their own, or use **Install ZIP...**. |
| The mod base index cannot be read. Clear the cache in the settings. | **Settings > Mod base > Clear the cache**. |

See [Plugins](../settings/plugins.md) and [Mod base](../settings/mod-base.md).

## Reports

- **Help > Open Logs Folder** opens the folder with `crash-*.txt` and `hang-*.txt`. Attach the newest file to a report. A `hang-` file means the window did not answer for 10 seconds. The newest 10 of each kind are kept. They are written whether or not crash reports are on, and nothing is sent from them.
- **Help > Copy Diagnostics** copies version and system information.
- **Help > Report a Problem...** opens `https://github.com/y114git/G3MStudio/issues`. **Help > Send Feedback...** sends feedback from inside the app. **Send Feedback** says "Reporting is off. Turn on crash reports in Settings > Privacy to send feedback, or use Help > Report a Problem." while crash reports are off. If no browser starts: "No browser could be started. Open {url} yourself."

## Memory

Low memory shows "Memory is low, G3MStudio is running in low-memory mode." and the status bar adds "low-memory mode". If an operation still fails: "Not enough memory for {name}. G3MStudio freed what it could. Close other programs or tabs and try again." Lower the **Cache limit** or set a **Memory limit** under **Settings > Advanced**, or close tabs.

## See also

- [Files and folders](files-and-folders.md)
- [Platforms](platforms.md)
