# Importing mods

Import adds a mod to the active profile's Library. Downloading an archive and importing its contents are separate actions; [Downloads](../features/downloads.md) reports both.

## Choose a source

Use **Add Mod > Import**, drop files or folders into the Library, or select a download in Mods Browser. The import dialog accepts local content and download URLs. A URL must point to downloadable content, rather than a web page containing a download button.

Supported archive inputs include ZIP, 7z, RAR, TAR and compressed TAR variants, and LZMA where applicable. A package with `mod_config.json` supplies its own metadata and operations. G3M can also open supported DATA files, `.g3mpatch`, `.xdelta`, `.vcdiff`, and `.csx` inputs for manual configuration.

## Packages with configuration

G3M reads and validates the configuration, checks package paths, and copies the prepared mod into the active profile. Invalid configurations require correction; selecting another game does not make an invalid operation valid.

Supported G3M configurations in the older 1.x formats are converted to the current `2.0.0` structure during import. DELTAMOD packages with JSON or TOML metadata are also converted. Script dependencies remain in the mod folder so relative `#load` and resource paths can resolve without installing dependency-only files into the game.

Keep the original archive if you need an untouched copy. The installed mod uses the [current configuration format](mod-config.md).

## Files without configuration

**Manual Mod Installation** collects the input files into a mod. Configure its contents on **Installation** and read bundled documents on **Files and instructions**. Available GameBanana metadata, including the description, icon, and selected download's version, is retained.

### Installation

Select the game and enter the mod name. G3M checks XDelta and G3MPatch inputs against files in the configured game installation and data folders, including subfolders. A confirmed match fills in the patch's destination. The check does not change game files. Switching games repeats it. Choices without a confirmed match remain available for manual configuration.

Matching replacement files and documentation can also receive actions automatically. Review each assignment before saving.

| Action | Effect |
| --- | --- |
| Copy / Replace | Copy a file or folder to the selected destination. |
| Apply patch | Apply the patch to the selected destination file. |
| INFO/README file | Include documentation without installing it into the game. |
| Extract | Place a folder's or archive's contents into a destination folder or supported writable archive. |
| Skip | Keep the item in the package without installing it. |

Choose an action for every file or folder and a destination where required. Expand a folder to configure its files individually, or assign an action to the whole folder. Use **Ctrl** + click to select several rows and set their actions or destinations together. **Browse** selects a destination file or folder and stores the appropriate folder reference.

The filter searches the file list. **Hide configured files** shows only items that still need a choice; G3M remembers this option between installations and restarts.

### Files and instructions

Select a bundled file to read TXT, Markdown, HTML, or PDF content. **Open externally** opens the selected file in the system's default application.

### Save

**Save** validates the setup and adds the mod to the Library. **Save and configure** also opens [Mod Editor](mod-editor.md) for metadata, action order, placeholders, and compatibility. Hover over a disabled save button to read why saving is unavailable.

If an XDelta or G3MPatch cannot be verified against the selected game files, saving shows a warning. Choose **Save** to keep the configuration anyway, or **Cancel** to revise it. Saving does not establish that the patch will work at launch.

Several dropped inputs without configuration can be collected into one manual installation. Documentation and script dependencies can remain bundled without copy operations.

## An installed ID already exists

The duplicate dialog offers **Merge** and **Replace**. Merge saves the incoming package as a version of the installed mod; it does not combine both configurations into one launch selection. Replace installs the incoming package in place of the existing mod.

Save or export the current mod if you want an independent backup. Review [Mod Versions](mod-versions.md) before switching to a merged import.
