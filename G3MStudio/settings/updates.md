# Update settings and channels

G3MStudio has no update option on the Settings page. The check runs when you ask for it, and the channel follows the version you run.

## Check for updates

**Help** > **Check for Updates...** (command `help.checkUpdates`) reads the releases of the project on GitHub.

| Result | Message |
| --- | --- |
| You run the newest version | "G3MStudio {version} is the newest version." |
| A newer version exists | "G3MStudio {version} is available (you have {current}). Download it and restart to install?" with **Update** as the title. |
| The check or download failed | "The update failed: {reason}". |
| The program folder cannot be written | "the program folder cannot be written to; move G3MStudio to a folder you own". |

Only one check runs at a time. After you agree, the message "Downloading G3MStudio {version}..." appears, the zip is downloaded and verified, and G3MStudio asks to close so an update script can replace the files and start it again.

## Channels

There is no channel switch. The rule is:

- A running pre-release version (a version with a suffix such as `1.0.0-rc1`) is offered pre-releases and releases.
- A running release version is offered releases only.
- Drafts are never offered, and an update is never a downgrade.

The offered file is the zip for your system and processor, named `G3MStudio-<OS>-<arch>-<version>.zip`, with a SHA-256 digest published by GitHub. A release without that zip or digest is skipped. Downloads are accepted only from the releases of the project repository.

The result of the check (current, available, failed) is part of the usage statistics when those are on. See [Privacy and crash reports](privacy.md).

## See also

- [Updating](../getting-started/updating.md)
- [Privacy and crash reports](privacy.md)
