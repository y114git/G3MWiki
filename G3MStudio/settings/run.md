# Run settings

The **Run** category sets how the game starts from G3MStudio. Every control saves at once. The behavior of a run (how the game is found, temporary copies, stopping) is explained on [Run settings and profiles](../projects/run-settings.md) and [Run the game](../projects/run-game.md). This page lists each control in page order.

## Controls

| Label | Control | Default | Range | Saved as |
| --- | --- | --- | --- | --- |
| **Runner (empty runs the game executable next to the data file)** | Text | empty | any program | `runRunner` |
| **Extra arguments** | Text | empty | | `runArguments` |
| **Working folder (empty is the data file folder)** | Text | empty | | `runWorkingDir` |
| **Files and folders to link** | Multi-line text | empty | one item per line | `runLinks` |
| **Environment variables (NAME=value, separated by semicolons)** | Text | empty | | `runEnvironment` |
| **Seconds to wait for the game to close before it is killed** | Number | 5 | 1 to 60 | `runStopSeconds` |
| **When the game hands over to another process (Steam, launchers)** | List: **Wait for it**, **Do not wait** | **Wait for it** | `runHandover` (`Wait`, `DoNotWait`) |
| **Runtime folders (separated by semicolons)** | Text | empty | | `runtimeFolders` |
| **Write the game log (-debugoutput)** | Checkbox | Off | | `runDebugOutput` |
| **Save the data file and run on it instead of a working copy** | Checkbox | Off | | `runSaveInPlace` |
| **Restart the running game after an edit** | Checkbox | Off | | `runAutoRestart` |
| **Warn that a run is a temporary copy** | Checkbox | On | | `warnTempRun` |
| **Run profile** | List and **Delete profile** | **Default** | saved profiles | `activeRunProfile` |
| **Profile name** with **Save as profile** | Text and button | empty | | `runProfiles` |
| **Where run options are stored** | List | **G3MStudio's folder** | **G3MStudio's folder**, **Next to the data file** | `runOptionsLocation` (`StudioFolder`, `BesideDataFile`) |
| **Project file**: **Save these options for this data file**, **Move Run Options to G3MStudio's Folder** | Buttons | | | See below. |

## Hand-over to another process

Steam games often end the first process at once with code 0 and let Steam start the game again. With **Wait for it**, a game that ends within 20 seconds with code 0 is followed: if the same program runs now or starts within 15 seconds, the run stays open and the working copy stays until that process ends. **Do not wait** treats the first exit as the end of the run, as for any other game. See [Run the game](../projects/run-game.md#games-started-through-steam).

## Files and folders to link

The tip reads: "One per line: source, or source => name. A relative source starts at the folder of the data file. The items are made available next to the game while it runs (a link, or a copy where links are not allowed) and removed afterwards."

- A line is `source` or `source => name`. `name` is the item's name in the run folder. Without it, the name of the source is used.
- A relative source is read from the folder of the data file.
- Lines that start with `#` and empty lines are skipped. A name that holds `/` or `\` is skipped.
- An item that already exists in the run folder is left alone.
- A symbolic link is tried first. Where links are not allowed (Windows without developer mode), the item is copied.
- A source that does not exist is reported and the other items still go in.

Example for a game started from a chapter folder that needs the `mus` folder one level up:

```text
../mus
../lang => lang
```

## Compatibility group

The group titled **Compatibility (Windows games on Linux and macOS)** sits on the page between **Runtime folders (separated by semicolons)** and **Write the game log (-debugoutput)**. It is shown on every system. On Windows the detection line reads "Not needed on Windows."

| Label | Control | Default | Saved as |
| --- | --- | --- | --- |
| **Run Windows games with** | List: **Automatic**, **Native only**, **Wine**, **Proton**, **umu**, **PortProton**, **Custom command** | **Automatic** | `runCompat` |
| **Tool program or folder (empty detects it)** with **Browse...** | Text | empty | `runCompatProgram` |
| **Wine prefix folder (empty uses the Steam prefix of a Steam game, else the tool's default)** | Text | empty | `runWinePrefix` |
| **Custom command: {exe} {data} {dir} {prefix} {args}** | Text, shown only for **Custom command** | empty | `runCompatCommand` |
| **Detected now** with **Detect** | Text and button | | Shows what **Automatic** would use now. |

The detection line reads "Auto would use: {tool}", "Auto would use: {tool}, prefix {prefix}" or "Nothing found: no Wine, Proton, umu or PortProton". See [Run on Linux and macOS](../projects/run-linux-macos.md).

## Profiles

A profile stores the runner, arguments, debug output flag, working folder, environment and the four compatibility fields. Picking a profile copies its values into the controls. The profile chosen for a data file is remembered for that file (`fileRunProfiles`). The note reads: "The run profile you pick is remembered for each data file." **Files and folders to link** is not part of a profile.

## Project file

**Save these options for this data file** writes the current run options to a project run file for the open data file, in the place chosen by **Where run options are stored**. With no file open it says "Open a data file first." A file written next to the data file is trusted automatically for the owner who wrote it. **Move Run Options to G3MStudio's Folder** (command `run.migrateOptions`) asks "Copy these run options into G3MStudio's folder? They then need no approval. The file next to the data file stays and is no longer used." and, after you accept, reports "Run options moved to G3MStudio's folder." With no run file next to the data file it says "No run options file next to the data file." The question can be silenced (see [Prompts](prompts.md)). Files next to a data file that you did not write ask for approval before they are used, because they can name any program; hashes you approved are kept in `trustedRunFiles`.

## Restore defaults

Resets every control above to its default, and the chosen run profile to **Default**. It keeps the stored profiles, the per-file profile map and the approved run files. It does not reset **When the game hands over to another process (Steam, launchers)**, which stays as you set it.

## See also

- [Run settings and profiles](../projects/run-settings.md)
- [Run the game](../projects/run-game.md)
- [Run on Linux and macOS](../projects/run-linux-macos.md)
- [Prompts](prompts.md)
