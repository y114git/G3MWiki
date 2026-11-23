# Mods and the mod base

A G3M mod is a ZIP with a `mod_config.json` and a patch. G3MStudio applies mods and exports your changes as one. This page covers **Apply Mod...**, **Export as Mod ZIP...** and the mod base cache that makes exports possible without asking for the original.

## Apply Mod...

1. Choose **Mods > Apply Mod...** and pick the mod ZIP ("Pick the mod ZIP").
2. G3MStudio reads `mod_config.json` in memory. Unusable ZIPs say "This is not a usable mod: {reason}".
3. It keeps the operations that target a data file with the same name as yours (case does not matter).
4. Without a file open, or if you choose a new file, the result is saved as a new file. The question reads "Apply {name} {version} (patches: {count}) to {file}? The result is saved as a new file; {file} is not changed."
5. With a file open you can instead apply to the open file (it cannot be undone; **Revert to Saved...** is the way back).

Result: "{name} was applied. Saved to {path}." with an **Open** action.

### What is supported

G3MStudio applies `patch` operations whose payload is a `.g3mpatch` file inside the ZIP. Anything else is skipped: "Operations that need xdelta or a script and are skipped: {count}." If nothing is usable: "{name} only has patches G3MStudio cannot apply (xdelta or script payloads). Use G3MTool or Tools > Run Script for {file}." A mod with no patch for your file says "{name} has no patch for {file}."

Limits: `mod_config.json` up to 4 MiB; a payload up to 4 GiB. A payload path may not leave the ZIP.

## Export as Mod ZIP...

Command `mods.exportMod`, reached from **File > Transfer Resources...** (group "Mods": "Save your changes as a mod ZIP.") or the command palette. It needs a saved file: "Save your changes first: the patch is made from the file on disk."

1. Provide the original file (see the mod base below).
2. Pick the output in "Save the mod as".
3. G3MStudio writes a ZIP with `mod_config.json` (config version `2.0.0`) and `mod.g3mpatch`.

The config holds an id made from the file name (lowercase letters, digits, `_` and `-`, starting with a letter, up to 50 characters), the name (up to 50 characters), version `1.0.0`, an empty authors list, the game id and a `patch` operation with source `${mod_path}/mod.g3mpatch` and target `${game_path}/<data file name>`.

Result: "Mod saved to {path}. Edit its mod_config.json to set authors and the game ID." Authors are not asked, so edit the file with a text editor.

## The mod base

Exports compare your file with the original. G3MStudio keeps a compressed copy of each data file when it first opens it, so it does not have to ask. The cache lives in the G3MStudio data folder; copies are named by their SHA-256 and shared between paths with the same content.

| Setting (Settings > Mod base) | Default |
|---|---|
| Keep a copy of each data file when it is first opened, as the base for Export Mod Pack | On |
| Size limit of the cache | 2048 MB (range 64 to 1,048,576) |
| Clear the cache | Button. "The cache takes {size} MB." |

The oldest copies are dropped when the limit is reached. A file larger than the limit is not kept: "{name} is larger than the cache limit, so no copy was kept."

Commands in the **Mods** menu:

| Command | What it does |
|---|---|
| **Remember This File as the Mod Base** | Keeps the open file as the original. Message: "{name} is now the mod base." |
| **Clear the Mod Base Cache** | Empties the cache: "The mod base cache is empty." If a file is in use: "The cache could not be cleared. Some files are in use." |

If the file on disk changed since it was remembered, a dialog asks "The file changed since it was remembered. Use the remembered original, or remember this file as the new base?" with **Use the remembered original** and **Remember this file as the new base**. If the saved copy is missing or damaged: "The saved copy of the original is missing or damaged. Choose the original file." If the file you pick does not match the saved original: "That file does not match the saved original (the hash differs). Nothing was exported."

## See also

- [Patches](patches.md)
- [Mod packs and Compare Data Files](mod-packs.md)
- [Guide: share a mod](guide-share-mod.md)
