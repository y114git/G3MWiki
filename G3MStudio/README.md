# G3MStudio

G3MStudio is an editor for GameMaker data files such as `data.win`. It browses and changes sprites, sounds, objects, rooms, code and more, and builds mods and patches from your changes. This page gives the overview and links to every area of the wiki.

## Who it is for

Modders, translators and tinkerers who work on a finished GameMaker game without its project. You open the game's data file, edit it, save, and run the game to test.

## Version and status

The version documented here is 1.0.0. G3MStudio changes game files, so work on a copy and keep a backup of the data file. Some data may not round trip exactly. Running a game on Linux and macOS is tested only with simulated setups so far.

G3MStudio is licensed under GPL-3.0-only.

## Platforms

Windows, Linux and macOS, each on x64 and arm64, as a portable zip (see [Install](getting-started/install.md) and [Platforms](reference/platforms.md)). It is built on .NET 10 and Avalonia 12 and reads and writes data files with G3MLib 1.1.5. The interface comes in seven languages (see [Languages](getting-started/languages.md)).

## What it opens

| File | Target |
| --- | --- |
| `.win` | Windows games |
| `.unx` | Linux games |
| `.ios` | iOS and macOS games |
| `.droid` | Android games |
| `audiogroupN.dat` | Audio group files |

`.symbian` is registered as a data file type for "Open with". The file picker also offers all files, and the reader decides by content. The GameMaker versions that open are the ones G3MLib 1.1.5 reads. See [Opening a data file](getting-started/opening-files.md) and [File formats](reference/file-formats.md).

## Where to start

1. [Install](getting-started/install.md) the zip for your system.
2. Read [First launch](getting-started/first-launch.md) for the welcome window and the tour.
3. Open a file from the [Start page](getting-started/start-page.md).
4. Learn the layout in [Main window](interface/main-window.md).

## Feature map

| Area | What it covers | Start with |
| --- | --- | --- |
| Getting started | Install, first run, Start page, opening files, updating, languages, command line, data folder | [Install](getting-started/install.md) |
| Interface | Main window, menus, command palette, panels, status bar | [Main window](interface/main-window.md) |
| Code | Code editor, GML intelligence, shortcuts | [Code editor](code/code-editor.md) |
| Graphics | Sprites, images, textures, tile sets, backgrounds, fonts | see [Summary](SUMMARY.md) |
| World | Rooms, objects and events, sequences, timelines, paths, particles, shaders, extensions, curves | see [Summary](SUMMARY.md) |
| Audio | Sounds, audio editor, audio groups | see [Summary](SUMMARY.md) |
| Projects | Workspaces and projects, saving, snapshots, history, Git, mods | [Projects overview](projects/overview.md) |
| Settings | Every Settings section, shortcuts, themes, privacy | see [Summary](SUMMARY.md) |
| Reference | Glossary, file formats, files and folders, troubleshooting, platforms | [Glossary](reference/glossary.md) |

## Relation to other G3M tools

| Tool | Role |
| --- | --- |
| G3MLib | The .NET library that reads, writes, decompiles and patches GameMaker data files. G3MStudio is built on it. |
| G3MTool | A GUI and command line tool on G3MLib. G3MStudio has no command line mode for editing; use G3MTool for batch work. |
| G3M | The mod manager. G3MStudio shares its language and design lineage and can create mods and patches. |

## Help and reports

**Help > Report a Problem...** opens `https://github.com/y114git/G3MStudio/issues`. Include the version from **Help > Copy Diagnostics**, what you did, what you expected and what happened, and the newest file from **Help > Open Logs Folder**. See [Troubleshooting](reference/troubleshooting.md).

## See also

- [Glossary](reference/glossary.md)
- [Changelog](reference/changelog.md)
- [Summary](SUMMARY.md)
