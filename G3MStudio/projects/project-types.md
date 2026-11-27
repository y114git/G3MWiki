# Full or Delta projects

A project comes in two kinds. A **Full** project writes every resource of the game to the project folder. A **Delta** project writes only what you add, change or delete, like a UTMT project. This page explains when each one fits, where you choose, and how a Delta project works.

## Choose the kind

G3MStudio asks for the kind in three places:

- **Enable project** (the status bar item, the notification, or **Project > Enable Project**). When the projects folder is not remembered yet, the same dialog also asks for it.
- **Project > New Project...**, which always asks.
- [Import a UTMT project](utmt-import.md). Here **Delta** is picked by default, because a UTMT project is already a change set.

Each option shows what it is good and bad at. Tick **Remember my choice (Settings > Projects > New project type)** and G3MStudio stops asking on enable. You can change the answer at any time in [Settings > Projects](../settings/projects.md) (**New project type**: **Ask each time**, **Full** or **Delta**).

## Compare the two

| | Full | Delta |
|---|---|---|
| What the folder holds | Every resource of the game | Only resources you added, changed or deleted |
| Size and first export | Large, takes a while | Small, quick |
| Sharing the folder | Shares all of the game's assets | Holds no untouched game assets |
| Reading the whole game outside G3MStudio | Yes, every file is there | No, the rest of the game is not in the folder |
| Git history | Of everything | Of your changes |
| Building the data file | From the folder alone, over the last synced file | Needs the game's own data file |
| A new version of the game | Merge as an outside change | Your changes are built over the new version |
| Kept in G3MStudio's data folder | Snapshots and the merge base | The same, plus one compressed copy of the game's own data file |

Pick **Full** when you want the whole game as files, for example to search all code in another editor or to keep a complete backup. Pick **Delta** when you make a mod that you share with others, work in a team, or want to move to a new game version later.

## How a Delta project works

When the project is made, G3MStudio keeps the game's own data file once, gzip compressed, in its data folder under `projects/<id>/original/`, with the list of its resources. If the open file was already changed before (it differs from the copy G3MStudio kept when the file was first opened, see [Mods and the mod base](mods.md)), that first copy is used as the original and a notification says "{name} was changed before. The project compares with the copy of the original that G3MStudio kept when the file was first opened."

From then on:

- An edit writes only the resources it touched, as in a full project.
- An edit that names no resource (for example a bulk change) compares the open file with the kept original and writes what differs.
- A resource of the game that you deleted or renamed is recorded as deleted in the project file, so building the project over the game's own file removes it there too.
- The project file records the game's own data file as its base file. A sync does not change that.
- A resource that matches the game's version again stays in the folder until the next comparison drops it. It does no harm.

The project file has `"mode": "delta"`. It is the same `.g3mproj` format, so G3MLib and G3MTool build it like any project.

## Move a mod to a new game version

When the game is updated, open the new data file as usual. The project finds it, and the dialog **Data file changed outside** has an extra line: "This project keeps only your changes. If the game was updated, choose Replace data file with project: the new file becomes the game's own file and your changes are built over it."

Choose **Replace data file with project**. The new file becomes the project's original and your changes are built over it. Check the result: a change to a resource that the update also changed replaces the update's version of that resource.

**Bring changes in** works as in a full project, with changes compared to changes: the update's untouched resources never land in the project folder.

## Open a Delta project someone shared

Use **Project > Open Project...** and pick the project folder or its `.g3mproj`. When G3MStudio asks for the data file, pick the game's own data file, the one the project was made for. Its size and MD5 must match the project's base file. G3MStudio keeps it as the project's original, builds the changes into it, and opens it. If that file is already open with unsaved edits, it says "Save this data file first: the project's changes are built into it."

## Change the project type

**Project > Change Project Type...** turns the open project into the other kind. Edits not synced yet are synced first. The Git files of the folder (`.git`, `.gitignore`, `.gitattributes`) stay, so the change shows up in Git as one diff.

- **Delta to Full** writes every resource of the game to the folder, clears the recorded deletions and drops the kept copy of the original.
- **Full to Delta** needs the game's own data file. G3MStudio uses the copy it kept when the file was first opened (see [Mods and the mod base](mods.md)); without one it asks "Choose the game's own data file, without any mod". The new project is written beside the old one and built over that file first. Only when the result is the open file does it replace the folder; otherwise nothing changes and you get "The project type was not changed: built over the chosen file, the Delta project would differ from {name} in {count} resources."

Pick a clean copy of the game for Full to Delta. A file that already holds part of your mod becomes the original, and that part of the mod leaves the project.

## Limits

- The comparison covers sprites, tile sets, sounds, paths, scripts, shaders, fonts, timelines, objects, rooms, sequences, animation curves, particle systems, code, strings, variables and functions. A change to anything else (extensions, UI layers, options, audio or texture groups) is written only when the edit itself names it.
- Removing the kept original (deleting `projects/<id>/` in the data folder) leaves the project unable to compare. Open the project again from the game's own data file.

## See also

- [Projects and Project mode](overview.md)
- [Import a UTMT project](utmt-import.md)
- [Outside changes, merge and conflicts](outside-changes.md)
- [Guide: share a mod](guide-share-mod.md)
- [Projects settings](../settings/projects.md)
