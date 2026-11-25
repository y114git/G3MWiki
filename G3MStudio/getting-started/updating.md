# Updating

G3MStudio updates from its GitHub releases, from inside the app or by hand. The app checks only when you choose **Help > Check for Updates...**. It never checks on its own.

## Update from inside the app

1. Choose **Help > Check for Updates...** (command `help.checkUpdates`, no default shortcut). A second choice while a check or download is still running does not start another one.
2. If no newer version is offered, a note says "G3MStudio {version} is the newest version."
3. Otherwise a window titled **Update** asks: "G3MStudio {version} is available (you have {current}). Download it and restart to install?" Confirm to continue.
4. A note says "Downloading G3MStudio {version}...". The zip is checked against the SHA-256 digest that GitHub publishes for it, then unpacked into the work folder. Nothing in the program folder changes yet.
5. G3MStudio asks about unsaved edits as on close (see **Settings > Files**, "Ask before closing with unsaved changes"). If you cancel, nothing is installed. Otherwise it starts the update script and exits.
6. The script waits up to 120 seconds for G3MStudio to exit. It then moves each file of the new version into the program folder, keeping the old file in a backup folder. If any move fails, it puts every old file back. Last, it starts G3MStudio again: the new version after a good update, the old one after a failed one.

### Which release is offered

- Only a version higher than the one you run is offered, never a lower or equal one.
- Drafts are skipped.
- Pre-releases are skipped unless **Settings > Files > Also offer pre-releases when checking for updates** is on (setting `updatePrereleases`, default off). It counts for any version you run, not only pre-release builds.
- A release is the bare version as its tag, for example `1.0.0`, with a zip named `G3MStudio-<OS>-<arch>-<version>.zip` for your system whose `<version>` equals the tag. Without that zip, or without a SHA-256 digest on it, the release is not offered. A leading `v` on a tag is tolerated.
- The zip must come from `https://github.com/y114git/G3MStudio/releases/download/`. Any other address is ignored.
- Among several eligible releases the highest version wins. A release is higher than its own pre-releases (`1.0.0` is higher than `1.0.0-rc1`).

### Download

An interrupted download leaves a `.part` file and resumes from it at the next check. If the server ignores the resume or the checksum is wrong after a resume, the download starts again from zero. A wrong checksum after a full download deletes the file and shows an error. A zip that is already downloaded and matches its checksum is used again.

### Work folder

Work files live in `.g3mstudio-update` next to the program: the downloaded zip, `new` (the unpacked release), `old` (backups while the script runs) and `result.txt` (`ok` or `failed`). On macOS "next to the program" means next to `G3MStudio.app`. The script is `g3mstudio-update-<pid>.cmd` on Windows and `.sh` elsewhere, written to a temporary folder, and it deletes itself when it ends. On Linux and macOS the unpacked files keep the executable bit of the zip.

The next start removes `.g3mstudio-update` after every update, good or failed. A work folder without `result.txt` (an interrupted download) is kept so the download can resume.

### After a failed update

When the script had to restore the old version, the next start shows the message "The last update could not be installed, so the old version was restored. Try Help > Check for Updates again." If G3MStudio did not exit within 120 seconds, the script gives up and does not restart it; the next start you make by hand shows the same message.

Files that exist only in the old version stay in the program folder. The updater adds and replaces files, it does not delete any.

## Update by hand

1. Download the zip for your system from the releases page (see [Install](install.md)).
2. Close G3MStudio.
3. Unpack over the old files. Your settings and recent files stay: they are in the data folder (see [Data folder](user-data.md)), not next to the program.

## Messages

Each message starts with "The update failed: ".

| Reason | Fix |
| --- | --- |
| the program folder cannot be written to; move G3MStudio to a folder you own | Move the program out of a protected folder such as `Program Files` and retry, or update by hand. |
| The update file did not match its checksum and was deleted. | Try again; the download was damaged. |
| The update file could not be unpacked. | Download the zip by hand. |
| The update file holds no files. | Download the zip by hand. |
| The update list has an unexpected format. | GitHub sent an unusual answer; try later. |
| {network error} | No connection. The request to GitHub times out after 20 seconds. |

When usage statistics are on, the result of a check (newest, available or failed) is part of the anonymous usage summary (see [First launch](first-launch.md)).

## See also

- [Install](install.md)
- [Changelog](../reference/changelog.md)
- [Troubleshooting index](../reference/troubleshooting.md)
