# File formats

This page lists the file kinds G3MStudio reads or writes. Detail for each kind is on the page of the feature that uses it.

## Game files

| Kind | Extension | Read | Write |
| --- | --- | --- | --- |
| GameMaker data file | `.win`, `.unx`, `.ios`, `.droid` (`.symbian` is registered for "Open with") | Yes | Yes, by **Save** (see [Saving](../projects/saving.md)) |
| Audio group file | `audiogroupN.dat` | Yes | Not by the picker; groups beside the data file are read for sounds |
| Temporary save file | `<name>.g3mnew` | | Written during a save, then moved over the target |
| Temporary picture file | `<name>.<32 hex>.g3mtmp` | | Written while an included picture is replaced |

## Mods and projects

| Kind | Extension | Use |
| --- | --- | --- |
| Patch | `.g3mpatch` | Changes between two data files (**Create Patch...**, **Apply Patch...**). |
| Mod pack | `.g3mpack` | Several patches in one file (**Export Mod Pack...**, **Apply Mod Pack...**). |
| Mod ZIP | `.zip` | **Export as Mod ZIP...**, **Apply Mod...**. |
| Project | `<name>.g3mproj` and a folder of resource files | Linked projects (see [Project menu](../projects/project-menu.md)). |
| Run options | `<data file>.g3mrun.json` | Run options and profiles kept next to the data file (see [Run settings](../projects/run-settings.md)). |
| Plugin | folder with `plugin.json` and `src/*.cs`, or a ZIP of it | See [Writing a plugin](plugin-api.md). |
| UndertaleModTool project | `project.json` folder | Converted once on open. |

## Resource files

| Kind | Extension | Use |
| --- | --- | --- |
| Pictures | `.png` | Sprites, textures, room images. |
| Audio | `.wav`, `.ogg` | Sound import and export. |
| Code | `.gml` | Code import and export. |
| Tables | `.csv` | Strings and room tiles. |
| Scripts | `.csx` | C# scripts run from **Tools > Run Script** or dropped on the window. |
| Auto-tile and other rules | `.json` | Auto-tile rule export, settings and per-file state. |

## Internal files

Settings, recent files, journals and caches are JSON files in the data folder (see [Files and folders](files-and-folders.md)). They are not meant to be edited by hand, except for backing up `settings.json`. They are written to a temporary `<name>.<32 hex>.tmp` file and renamed, so a crash leaves the old or the new file. A leftover older than one day is deleted at start.

## See also

- [Opening a data file](../getting-started/opening-files.md)
- [Files and folders](files-and-folders.md)
- [Glossary](glossary.md)
