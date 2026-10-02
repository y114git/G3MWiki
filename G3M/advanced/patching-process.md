# Applying and restoring mods

The selected launch mode determines whether applied changes are temporary or retained.

In normal Launch, G3M checks the selected setup, backs up affected files, applies mod operations and priority steps, then starts and monitors the game. After exit it restores eligible files before another launch is allowed.

Operations can patch DATA, copy files or directories, extract archives, or provide documentation. Their destinations can refer to the game installation, game user-data folder, or user home. Read the [operation reference](../mods/mod-config.md) when authoring a package or inspecting an unusual destination.

Mods in one step derive changes from the same starting DATA. Steps run in sequence. See [Priority & Steps](../mods/modpacks.md) for ordering and addons.

## Failed application

If patching fails, G3M attempts to undo changes from the interrupted application. Read the error and check that rollback finishes before retrying. Missing sources, an incorrect original game version, denied write access, and script failures require different corrections.

## Changes made outside G3M

Files altered after deployment can be retained rather than overwritten by restoration. Review [backup and restore](backup-and-restore.md) for conflict recovery. Avoid running another patcher or updating the game during an active G3M session.

Use **Analyze Actual Launch Result** for a temporary application test. Use **Patching only** only when you want changes written to the actual installation and kept there.
