# Updating

G3MStudio updates from its GitHub releases, from inside the app or by hand. The app checks only when you ask.

## Update from inside the app

1. Choose **Help > Check for Updates...** (command `help.checkUpdates`, no default shortcut).
2. If your version is the newest, a note says "G3MStudio {version} is the newest version."
3. Otherwise a window titled **Update** asks: "G3MStudio {version} is available (you have {current}). Download it and restart to install?" Confirm to continue.
4. A note says "Downloading G3MStudio {version}...". The zip is checked against its SHA-256 digest from GitHub.
5. G3MStudio asks about unsaved edits as on close, then exits. A script waits for the exit, replaces the program files, and starts the new version. If a step fails it puts the old files back.

Rules the check follows:

- It picks the highest release above your version, never a downgrade.
- Drafts are skipped. Pre-releases are offered only when you run a pre-release version (a version with a suffix such as `-rc1`).
- The release needs a zip named `G3MStudio-<OS>-<arch>-<version>.zip` for your system with a SHA-256 digest, downloaded from the `y114git/G3MStudio` releases over HTTPS.
- An interrupted download resumes. A wrong checksum deletes the file.

Work files live in `.g3mstudio-update` next to the program (`new`, `old` and `result.txt` holding `ok` or `failed`). The script is `g3mstudio-update-<pid>.cmd` on Windows and `.sh` elsewhere, written to a temporary folder.

## Update by hand

1. Download the zip for your system from the releases page (see [Install](install.md)).
2. Close G3MStudio.
3. Unpack over the old files. Your settings and recent files stay: they are in the data folder (see [Data folder](user-data.md)), not next to the program.

## Messages

| Message | Fix |
| --- | --- |
| The update failed: the program folder cannot be written to; move G3MStudio to a folder you own | Move the program out of a protected folder such as `Program Files` and retry, or update by hand. |
| The update failed: The update file did not match its checksum and was deleted. | Try again; the download was damaged. |
| The update failed: The update file could not be unpacked. | Download the zip by hand. |
| The update failed: The update list has an unexpected format. | GitHub sent an unusual answer; try later. |
| The update failed: {network error} | No connection; the request times out after 20 seconds. |

## See also

- [Install](install.md)
- [Changelog](../reference/changelog.md)
- [Troubleshooting index](../reference/troubleshooting.md)
