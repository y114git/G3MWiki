# Applying and restoring mods

The selected launch mode determines whether applied changes are temporary or retained.

In normal Launch, G3M checks the selected setup, backs up affected files, applies mod operations and priority steps, then starts and monitors the game. After exit it restores eligible files before another launch is allowed.

Operations can patch DATA, copy files or directories, extract archives, or provide documentation. Their destinations can refer to the game installation, game user-data folder, or user home. Read the [operation reference](../mods/mod-config.md) when authoring a package or inspecting an unusual destination.

Mods in one step derive changes from the same starting DATA. Steps run in sequence. See [Priority & Steps](../mods/modpacks.md) for ordering and addons.

## Failed application

If patching fails, G3M attempts to undo changes from the interrupted application. Read the error and check that rollback finishes before retrying. Missing sources, an incorrect original game version, denied write access, and script failures require different corrections.

### Patch does not fit the game file

G3M reports this message when an xdelta patch fails its source checksum:

> The patch {patch} of {mod} does not fit the game file. It was made for another game version, or for the original file while another mod already changed it. Put mods that patch the same file into one step, or install the matching game version.

G3M shows this message after it has restored the files. If the restoration itself fails, or the plan fails for any other reason, G3M shows **Mod operation failed. Check the log for details.**

### Read-only files

When G3M replaces a patched file and the target is read-only or briefly held by another program, it clears the read-only flag and tries up to four times in total, waiting 0.2 s, 0.4 s, and 0.6 s between tries. After the game closes, read-only temporary files and folders (Windows can mark folders read-only) are removed so that restoration and the next launch are not blocked.

## Changes made outside G3M

Files altered after deployment are not overwritten by restoration unless you choose **Force restore**. Review [backup and restore](backup-and-restore.md) for conflict recovery. Avoid running another patcher or updating the game during an active G3M session.

Use **Analyze Actual Launch Result** in the Diagnostics dialog to test the selected priority steps in an isolated game copy. The game does not start. Use **Launch patching only** only when you want changes written to the actual installation and kept there.
