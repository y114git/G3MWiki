# Mods Browser

Mods Browser lists GameBanana uploads for the selected game. Games need a GameBanana ID to appear; add one to a custom entry in Game Manager when applicable.

## Find a mod

Choose a game, enter search terms, select tags, and choose the sort order. The direction button reverses that order. Content filters and [Blocklist](../features/blocklist.md) rules further limit displayed results.

Open a mod card to view its [details](mod-details.md). Select the intended download file when an upload offers several choices. Read the author's game-version and prerequisite instructions before installing.

## Badges and the G3M Verified filter

Cards can carry badges before the like count:

| Badge | Meaning |
| --- | --- |
| **G3M** | The mod, or some of its files, was confirmed to install in one click without Manual installation. |
| Work in progress | The author has not finished the mod. |
| Note | The listing has additional information to read first. |

The **G3M Verified** filter lists only mods with the G3M badge. The confirmation comes from a list kept by the G3M creator, which records the game and the supported game version of each mod; the version appears in the mod details. A mod without the badge can still be installed through Manual installation.

## Download and import

When a mod has several files, a list asks which one to download. The package for G3M is selected first. Its details show **Auto-install**: **Yes** only for DeltaMod files and for mods on the G3M Verified list. Every other file is downloaded and then installed manually from Downloads.

The file enters [Downloads](../features/downloads.md). With automatic use enabled, G3M attempts import after downloading. Otherwise, choose **Install** on the completed record. A download whose checksum does not match GameBanana's is kept with a warning, and you decide whether to install it.

A successful download can still need manual setup. GameBanana hosts many package formats; appearing in search does not guarantee that G3M can infer their file destinations or that they work on your platform.

## Missing or failed results

Check the selected game, search, tags, adult-content filter, and blocklist. If the request fails, inspect the status message and retry after checking connectivity. Local Library mods do not need online search to remain installed.
