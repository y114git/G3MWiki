# Command line

G3MStudio takes one file or link and a few flags. There is no `--help` or `--version` and no command line mode for editing: use G3MTool for that.

## Syntax

```text
G3MStudio [file-or-link] [--run | --run-with-runtime] [--smoke]
```

The first argument that does not start with `--` is the file or link. Other plain arguments are ignored.

| Argument | Effect |
| --- | --- |
| `<path>` | Opens that data file (see [Opening a data file](opening-files.md)). |
| `g3mstudio://...` | Handles the link (open, new-mod, patch-tools or a download question). |
| `--run` | Opens the file, then runs the game once it is open. Used by the Explorer entry "Run game with G3MStudio". The game program is found as described in [Run the game](../projects/run-game.md). |
| `--run-with-runtime` | Same, but asks you to pick a runtime first. Used by "Run game with G3MStudio, pick a runtime". |
| `--smoke` | Shows the window, waits 2 seconds, exits with code 0 if the window is visible, 1 if not. Registers nothing, listens for nothing. Used by the release build to test each zip. |

With a file argument and `--smoke`, the file is opened as well. `--smoke` alone leaves the window on the Start page.

Two more arguments exist for the maintainers and are not for daily use: `--memory-probe=<name> [file]` (a memory measurement run that then exits) and `--sentry-test <result-file>` (the release check of crash reporting).

## Second launches

When G3MStudio already runs for your user, a launch with a file or link hands it to the running window, which opens it and comes to the front. The new process then ends with exit code 0 before it shows a window. This does not apply with `--smoke`, `--run`, `--run-with-runtime` or `--memory-probe`.

## Environment variables

| Variable | Effect |
| --- | --- |
| `G3MSTUDIO_DATA` | Uses this folder instead of the default data folder (see [Data folder](user-data.md)). |
| `G3MSTUDIO_NO_REGISTRATION=1` | Skips the registration of `g3mstudio://` links and "Open with" entries at start. |
| `G3MSTUDIO_NO_ONBOARDING=1` | Skips the welcome window. |

## Example

```sh
./G3MStudio /games/mygame/data.win
./G3MStudio --run /games/mygame/data.win
```

## See also

- [Opening a data file](opening-files.md)
- [Platforms](../reference/platforms.md)
