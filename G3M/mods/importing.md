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

**Manual Mod Installation** collects the input files into a mod. Enter the game, name, and authors, then choose **Create and configure** to open the editor.

Only add operations for files that actually modify the game. Documentation and script dependencies can remain bundled without copy operations. Set each operation's type, source, and destination in the [Files tab](mod-editor.md); a loose patch does not describe its destination by itself.

Several dropped inputs without configuration can be collected into one manual installation. Complete the editor and save before using the mod.

## An installed ID already exists

The duplicate dialog offers **Merge** and **Replace**. Merge saves the incoming package as a version of the installed mod; it does not combine both configurations into one launch selection. Replace installs the incoming package in place of the existing mod.

Save or export the current mod if you want an independent backup. Review [Mod Versions](mod-versions.md) before switching to a merged import.
