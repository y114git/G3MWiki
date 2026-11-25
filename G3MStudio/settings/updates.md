# Update settings and channels

G3MStudio checks for updates only when you ask. It never checks by itself at start. One option on **Settings** > **Files** chooses whether pre-releases are offered.

## Check for updates

**Help** > **Check for Updates...** (command `help.checkUpdates`) reads the releases of the project on GitHub.

| Result | Message |
| --- | --- |
| You run the newest version | "G3MStudio {version} is the newest version." |
| A newer version exists | "G3MStudio {version} is available (you have {current}). Download it and restart to install?" with **Update** as the title. |
| The check or download failed | "The update failed: {reason}". |
| The program folder cannot be written | "the program folder cannot be written to; move G3MStudio to a folder you own". |

Only one check runs at a time. After you agree, the message "Downloading G3MStudio {version}..." appears, the zip is downloaded and verified, and G3MStudio asks to close so an update script can replace the files and start it again.

After the update, the work folder `.g3mstudio-update` in the program folder is removed. If the script could not install the new version and restored the old one, the next start shows "The last update could not be installed, so the old version was restored. Try Help > Check for Updates again."

## Channels

| Setting | Label | Default | Saved as |
| --- | --- | --- | --- |
| Pre-releases | **Also offer pre-releases when checking for updates** (on **Settings** > **Files**) | Off | `updatePrereleases` |

- With the option off, only stable releases are offered.
- With the option on, pre-releases (versions with a suffix such as `1.1.0-rc1`) are offered as well as stable releases.
- Drafts are never offered.
- A version equal to or lower than the one you run is never offered, so an update is never a downgrade.
- The highest eligible version is offered.

The offered file is the zip for your system and processor, named `G3MStudio-<OS>-<arch>-<version>.zip` (for example `G3MStudio-Windows-x64-1.0.0.zip`), with a SHA-256 digest published by GitHub. A release without that zip or digest is skipped. Downloads are accepted only from the releases of the project repository. The release tag is the bare version, such as `1.0.0`.

The zip holds the program (`G3MStudio.exe`, `G3MStudio` or `G3MStudio.app`), `LICENSE` and `THIRD-PARTY-NOTICES.txt`.

The result of the check (current, available, failed) is part of the usage statistics when those are on. See [Privacy and crash reports](privacy.md).

## See also

- [Files and startup settings](files.md)
- [Updating](../getting-started/updating.md)
- [Privacy and crash reports](privacy.md)
