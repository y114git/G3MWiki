# Guide: import from UTMT

This guide converts an UndertaleModTool project into a G3MStudio project. You end with a project folder, a patched data file in its `build` folder and, if you choose, a Git repository with one commit.

## Before you start

- The UTMT project folder (it holds `project.json`) and the original data file it was made for.
- Free space of about 3 times the data file size (4 times with Git).
- Know whether the project runs scripts. Scripts run with your user rights.

## Steps

1. Choose **Project > Open Project...** and select the UTMT folder or its `project.json`. Dropping the folder on the window does the same.
2. In **Open UTMT project**, press **Browse...** next to **Original data file of the game** and choose the original file.
3. Check **New project folder**. The suggested folder is in your projects folder. Use an empty or new folder.
4. Tick or untick **Create a Git repository in the new folder**.
5. If the dialog shows **Run the project scripts**, tick it only for a project you trust.
6. Press **Convert**. If the data file is not the one the project was made from, answer the question **Different data file**.
7. Wait through the stages "reading the data file", "applying the project", "writing the data file", "writing the project" and "setting up Git".
8. Read the report **UTMT project converted**. Check **Warnings**, then press **Open project**.

Result: the patched data file from `build` opens with its project and the status bar shows **Synced**.

## Afterwards

- The UTMT folder was not changed. Keep it or remove it.
- Scripts and options UTMT stored are not kept. The warnings list them.
- To share the result as a mod, see [Guide: share a mod](guide-share-mod.md).

## If something goes wrong

| Message | Fix |
|---|---|
| "The project runs scripts (...). Allow the scripts to convert it, or remove them from the project options." | Tick the scripts box, or remove the scripts. |
| "\"{path}\" is not empty. Choose a new or empty folder." | Choose another folder. |
| "Not enough free space on the drive of ..." | Free space or pick another drive. |
| "Could not convert the project: {reason}" | Nothing was kept. Read the reason and try again. |

## See also

- [Import a UTMT project](utmt-import.md)
- [Projects and Project mode](overview.md)
- [Git panel](git.md)
