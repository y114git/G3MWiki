# Changelog

Release notes for every version are on the GitHub releases page of G3MStudio: `https://github.com/y114git/G3MStudio/releases`. This page lists what each version brings and says how to read the version.

## Find your version

**Help > About G3MStudio** shows the version. **Help > Copy Diagnostics** copies the version information for a report. The version in this documentation is 1.0.0.

## Update

**Help > Check for Updates...** offers a newer release (see [Updating](../getting-started/updating.md)). Only stable releases are offered unless **Settings > Files > Also offer pre-releases when checking for updates** is on. A lower version is never offered. G3MStudio never checks on its own.

## 1.0.0

First release. It uses G3MLib 1.1.7, .NET 10 and Avalonia 12.

### Platforms and updates

- Addition: Portable zips for Windows, Linux and macOS, each for x64 and arm64: `G3MStudio-<OS>-<arch>-<version>.zip`. A zip holds the program (`G3MStudio.exe`, `G3MStudio` or `G3MStudio.app`), `LICENSE` and `THIRD-PARTY-NOTICES.txt`.
- Addition: **Help > Check for Updates...** downloads the new zip, checks its SHA-256 digest and swaps the files with a script. A failed swap restores the old version and the next start says so. The update work folder `.g3mstudio-update` is removed after every update.
- Addition: Updates check only when you choose the command. Pre-releases are offered only when **Settings > Files > Also offer pre-releases when checking for updates** is on.
- Addition: Release tags are the bare version (`1.0.0`).
- Addition: The interface is available in English, Russian, Spanish, Japanese, Korean, Simplified Chinese and Traditional Chinese.
- Addition: **Help > Interface Tour...**, **Help > Getting Started...** and an optional file association with "Open with" entries, `g3mstudio://` links and, on Windows, **Run game with G3MStudio**.

### Opening and saving

- Addition: Opens `.win`, `.unx`, `.ios` and `.droid` data files, with a second data file beside the first and **New Data File**.
- Addition: Save writes and checks a new file first, keeps 1 to 100 rotating backups (default 3) and then replaces the data file. It names the program that holds a locked file on Windows.
- Addition: An edit journal brings back unsaved edits after a crash.
- Addition: A moved or renamed data file is found again and keeps its layout, tabs and sizes.
- Addition: File, save and folder dialogs open in the folder of the last pick.
- Addition: Memory limit, undo limit and cache limit, and a low-memory mode that frees caches instead of crashing.

### Editing resources

- Addition: Property editor with undo for every resource type, and create, duplicate, delete and bulk rename for resources, with warnings before a delete that breaks references.
- Addition: Code editor with GML colors, completion, folding, go to definition, find usages, rename across code, search and replace with a preview, and a bundled GML reference.
- Addition: Room editor with layers, tiles, instances, auto-tile rules, views, creation order, CSV export and import, and PNG export.
- Addition: Sprite editor and image editor with layers, frame strip, collision masks, nine-slice guides, sheet import and GIF export.
- Addition: Sound player with waveform, cut, volume and fades for WAV, and OGG and MP3 import.
- Addition: Editors for fonts (including TrueType and OpenType replacement), paths, animation curves, sequences, particle systems, timelines, shaders, strings, texture pages, texture groups, audio groups and included files.
- Addition: Hex view, chunk export and import, disassembly, debug data and chunk offset map.
- Addition: Explorer with folders, collections, tags, saved searches and filters, tabs, a list layout for tabs and an optional Open Editors panel.
- Addition: Copy resources to another open data file.
- Addition: Nicknames for resources (set in the rename dialog), a per-file **Read only** lock for any resource and **Find Dead Resources...**.
- Addition: **Generate Sound...** and **Record Sound...**, with a level meter and spectrum in the sound player.
- Addition: **Command Palette...** (`Ctrl+Shift+P`) and an option to reopen the last file, tabs and layout at start.

### Mods, projects and scripts

- Addition: Patches, mod packs, mod ZIPs, patch merge and data file compare, with a mod base cache.
- Addition: Projects that link a data file to a folder of text files, with autosave, sync, snapshots, a Git panel and conversion of UndertaleModTool projects.
- Addition: C# scripts (`.csx`) from **Tools > Run Script...**, **Tools > Run Recent Script** (the last 10, **Clear Items** empties the list) and **Tools > Run C# Command...**.
- Addition: C# plugins with commands, panels, editors, code hints, code actions and options. Plugins install from a ZIP, a folder or a GitHub address.

### Running the game

- Addition: **Run Game** with live output, restart, stop, run profiles, run links for folders outside the game folder and a GameMaker runtime picker.
- Addition: Windows games run on Linux and macOS through Wine, Proton, umu, PortProton or your own command.

### Settings and reports

- Addition: Theme colors, sizes, fonts, density, accent color and theme import and export. Every command has a rebindable shortcut, including mouse buttons.
- Addition: Optional anonymous crash reports and usage statistics. A local crash log and hang report are always written.
- Addition: **Help > Open Logs Folder**, **Help > Copy Diagnostics**, **Help > Report a Problem...** and **Help > Send Feedback...**.
