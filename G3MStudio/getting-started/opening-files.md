# Opening a data file

G3MStudio opens GameMaker data files: the single file that holds a game's sprites, sounds, objects, rooms and code. This page lists the ways to open one, the files it accepts and what happens when opening fails.

## Supported files

| File | Target | Notes |
| --- | --- | --- |
| `data.win` | Windows | `*.win` |
| `game.unx` | Linux | `*.unx` |
| `game.ios` | iOS and macOS | `*.ios` |
| `game.droid` | Android | `*.droid` |
| `audiogroupN.dat` | Audio group file | Opens on its own; sounds that live in a group find it next to the data file. |
| `.symbian` | Symbian | Registered as a data file type for "Open with"; not in the picker filter. |

The picker (**File > Open**) filters the type **GameMaker data** (`*.win`, `*.unx`, `*.ios`, `*.droid`, `audiogroup*.dat`) and also offers all files. The reader decides by content, not by extension, so a renamed file opens when it is a data file.

The GameMaker versions that open are the ones the reading library G3MLib 1.1.5 handles. G3MStudio shows the format version of an open file (for example on its Recent card). A file that cannot be read gives the error below.

## Ways to open a file

1. **File > Open**, shortcut `Ctrl+O`, or **Open Data File...** on the Start page.
2. Click a card in **Recent** on the Start page.
3. Drag a file onto the window. With a data file open, dropped pictures, sounds and GML files become resources instead (see the editor pages); a dropped `.csx` file runs as a script after a warning.
4. Right-click the file in your file manager and choose **Open with** (needs the registration described in [First launch](first-launch.md); G3MStudio also lists itself under "Open with" on every start).
5. Give the path on the command line (see [Command line](command-line.md)).
6. A `g3mstudio://` link (see below).

Opening a second file while one is open closes the first. If it has unsaved edits, G3MStudio asks what to do with them. A newer open request cancels one that is still running. A progress overlay shows "Opening {name}... {percent}%" with **Cancel**.

Only one G3MStudio runs per user. A file or link given to a second launch is handed to the running window, and the second launch ends.

## g3mstudio:// links

| Link | Result |
| --- | --- |
| `g3mstudio://file:///path/to/data.win` | Opens that local file. It must exist. Network paths are refused. |
| `g3mstudio://https://host/path` | Asks "Download the file at {address} from {host}?" first. |
| `g3mstudio://action/open` | Action link. |
| `g3mstudio://action/new-mod` | Action link. Needs an open data file: "Open a game data file first, then start a new mod from its changes." |
| `g3mstudio://action/patch-tools` | Action link. |

Anything else shows "Link not recognised." The text is cut at the first comma; write a comma inside a link as `%2C`.

## Messages and fixes

| Message | Cause and fix |
| --- | --- |
| Could not open '{name}': {reason} | The file is not readable as a data file. G3MStudio may offer to rebuild it. Restore a backup if it is damaged. |
| '{name}' is in use by another G3MStudio window. Finish there first. | The file is open in another window. |
| Only files from this computer can be opened. Drop a data file from a folder. | The drop was a link, text or a file without a local path. |
| Data file not found | The file moved. The window offers files with the same content nearby and **Choose file...**, **Try again**, **Remove from Recent**, **Go to Home**. **Keep the saved layout and sizes for this file** keeps your layout. |
| {name} is not the file G3MStudio remembered, and your settings say not to open it. | **Settings > Files** changes this. |

## See also

- [Start page](start-page.md)
- [Command line](command-line.md)
- [File formats](../reference/file-formats.md)
- [Troubleshooting index](../reference/troubleshooting.md)
