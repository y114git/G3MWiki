# Import a UTMT project

G3MStudio converts an UndertaleModTool (UTMT) 0.9 project into a G3MStudio project. The UTMT folder is only read, never changed. This page covers the dialog, the checks, the result and the limits.

## What a UTMT project is

A UTMT project is a change set over a data file: a `project.json` main file and folders such as `code`, `objects`, `rooms`, `sprites`, `sounds`. G3MStudio applies it to the original data file in memory, then writes the result as a full G3MStudio project and as a data file.

Recognised project folders: `backgrounds`, `fonts`, `shaders`, `sounds`, `sprites`, `animcurves`, `code`, `objects`, `particlesystems`, `paths`, `rooms`, `scripts`, `sequences`.

## Start the import

Any of these starts it:

- **Project > Open Project...** and choose the UTMT folder or its `project.json`.
- Drop a UTMT project folder on the window.

G3MStudio treats a path as a UTMT project when it holds a JSON file with a `Name` and at least one UTMT option key. A folder with one such `.json` file counts even if it is not called `project.json`. An unreadable path says "'{path}' is not a UTMT project: there is no project.json with a project name and options."

## The dialog

The dialog **Open UTMT project** says "Convert this UndertaleModTool project into a G3MStudio project. The UTMT folder is not changed." and shows "Project: {name}, {count} resources".

| Field | Meaning |
|---|---|
| **Original data file of the game** | The data file the project was made for. Press **Browse...** ("Choose the original data file"). |
| **New project folder** | An empty or new folder. The suggested folder is the name of the project inside the projects folder (`name`, then `name-2`, `name-3` when taken). **Browse...** opens "Choose the new project folder". |
| **Create a Git repository in the new folder** | On by default when Git is installed, off and disabled when it is not. |
| **Run the project scripts ({detail}). They run with your user rights.** | Shown only when the project lists scripts. Off by default. |

Press **Convert**. Without a data file or folder: "Choose the data file and the new project folder first."

### Scripts

If the project runs scripts and you leave the box off, the conversion stops: "The project runs scripts (...). Allow the scripts to convert it, or remove them from the project options." Scripts run with your user rights, so tick the box only for projects you trust. They run once and are not kept in the new project.

### Wrong data file

When the project records the data file it was made from, G3MStudio compares size and MD5 hash. On a mismatch, the dialog **Different data file** asks "This data file is not the one the project was made from. Convert anyway?"

## Checks before anything is written

- The new folder must be new or empty. It may hold only `.git`, `.gitignore` and `.gitattributes`. Otherwise: "\"{path}\" is not empty. Choose a new or empty folder."
- The new folder and the UTMT folder must not contain each other.
- The data file must not lie inside the new folder.
- Free space: about 3 times the size of the data file, 4 times when Git is on. Otherwise: "Not enough free space on the drive of \"{path}\": about {n} MB are needed, {n} MB are free. Free some space or choose another folder."

## Steps

The progress bar shows the current stage: "reading the data file", "applying the project", "writing the data file", "writing the project", "setting up Git". Then:

1. The result data file is written into `build` inside the new folder (with files such as audio groups the project wrote next to it).
2. The project is exported to the new folder, with a resource count.
3. Original code sources from the UTMT `code` folder replace the exported decompiled text, so comments and layout are kept.
4. With Git: a repository is created and one commit is made with the message `Import UTMT project {name}`. An existing repository is kept and nothing is committed.
5. The project is registered for the data file in `build`.

A failed or cancelled conversion removes what it created and leaves the destination as it was. Cancelling says "Project export cancelled. Nothing was kept."

## The report

The dialog **UTMT project converted** says "{count} resources were written to {folder}. The data file with the project applied is {file}." and the Git result ("A Git repository was created with a first commit.", "The existing Git repository was kept; nothing was committed." or "No Git repository was created."). **Details** lists counts per type. **Warnings** lists what could not be carried over, or "Nothing was lost.":

| Warning | Meaning |
|---|---|
| Options | "These project options are not stored in a G3MStudio project. Their effect was applied once: {detail}". Options: Flags, VerifyStrings, ErrorOnWarnings, AssetsDataFilePath, ExcludeDirectories, SubProjects, Patches, ExternalFiles, FileCopies, FileDeletes, PreImportScripts, PreAssetImportScripts, PostImportScripts. |
| Scripts | "Scripts of the project ran and are not kept: {detail}" |
| Ignored folders | "Folders that UTMT does not read were left out: {detail}" |
| History | "The UTMT folder keeps its own Git history ({detail}). The new project starts a new one." |
| Git | "Git is not installed, so no repository was created." or "Git setup did not finish: {detail}" |

Press **Open project** to open the new data file with its project, or **Close**.

## Limits

- Only UTMT 0.9 project files are supported.
- The result project and the data file belong to the new folder; the original game file is not changed.
- A UTMT project that holds nothing to convert is refused.

## See also

- [Projects and Project mode](overview.md)
- [Git panel](git.md)
- [Guide: import from UTMT](guide-utmt-import.md)
