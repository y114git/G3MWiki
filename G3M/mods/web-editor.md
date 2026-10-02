# Web editor

[G3M Mod Creator/Editor](https://y114git.github.io/G3M-MCE/) creates G3M mod ZIPs in a browser. It uses the same configuration format and operation types as the desktop editor.

## Create or open a mod

Choose **Create Mod** for an empty package or **Edit Mod** to open a G3M ZIP, Deltamod ZIP, or `mod_config.json`. You can also drop an archive or config onto the start page or import area.

Supported G3M input configurations are converted into `mod_config.json 2.0.0`. Deltamod packages can contain JSON or TOML metadata and `Modding.XML` instructions. Opening a configuration file alone does not include the files it references; add those files before exporting.

The editor has **Metadata**, **Compatibility**, **Files**, **Placeholders**, and **Help** tabs. The [desktop editor guide](mod-editor.md) explains the corresponding configuration values.

## Add files and actions

**Add files** adds package files, and **Add folder** retains directory structure. Files remain in the ZIP even when no operation points to them. This is useful for scripts that load helper files.

You can drop files or folders anywhere in the editor to bundle them. More specific drop targets have these meanings:

| Drop target | Result |
| --- | --- |
| Source | Add one file or folder and use its package path as the operation source. |
| Icon | Add one image and use it as the mod icon. An image URL can also be dropped here. |
| Placeholder path | Add one file or folder and use its package path. |
| Source hash or target hash | Calculate the dropped file or folder's checksum without adding it to the ZIP. |

Drag a bundled file onto a path or hash field to reuse it. Hover another tab during the drag to open that tab. Operations can also be dragged to reorder them or move them into groups; arrow controls provide the same ordering action without dragging.

**Browse bundled ZIP** selects a file or folder within a ZIP already included in the package. A source hash can use that member's extracted contents. Hashes for members of other archive formats require desktop G3M.

Author names use one line per author in the web editor. The mod ID is editable and follows the [ID rules](mod-config.md#top-level-fields). Keep the ID unchanged when editing a published mod.

## Save ZIP

**Save ZIP** validates all editable tabs and package references, then downloads the package. Invalid fields are highlighted. Error links open the relevant tab and field, including operations inside collapsed groups.

The ZIP contains `mod_config.json`, bundled files, and directories, including empty directories. It does not include the original game files used only to calculate target hashes.

The editor limits a package to 10,000 file and directory entries, 1 GiB of uncompressed content, and 512 MiB per file. Duplicate paths, conflicting filename case, traversal paths, symbolic links in imported ZIPs, and invalid configurations are rejected.

## Export to G3M

**Export to G3M** performs the same validation and ZIP download as **Save ZIP**, then opens a handoff dialog.

1. Save the downloaded ZIP.
2. Enter its full local path, such as `C:/Users/Name/Downloads/my_mod.zip` or `/home/name/Downloads/my_mod.zip`.
3. Choose **Open G3M**.
4. Accept the browser's request to open G3M if it appears.

G3M must be installed and registered for `g3m://` links. A browser does not disclose the absolute download path to the page, so the dialog cannot fill it automatically. The path must refer to the saved ZIP on the same computer.

## Browser limitations

The editor packages mods. It does not read installed games, apply patches, run CSX scripts, launch games, or check a dependency against your desktop profile. Game paths remain placeholders in the exported configuration.

Import the ZIP into G3M and test it against the intended game version before sharing it. Leaving or reloading the page can discard unsaved edits. The editor asks before navigation when it has unsaved changes.
