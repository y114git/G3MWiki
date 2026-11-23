# Guide: share a mod

This guide packages your changes so another player can apply them. You end with a mod ZIP, and optionally a mod pack, that others can open with G3MStudio or G3MTool.

## Before you start

- Save your changes. A patch is made from the file on disk: "Save your changes first: the patch is made from the file on disk."
- Have the original, unchanged data file. If you opened the file for the first time in G3MStudio, the original is already remembered (see [The mod base](mods.md#the-mod-base)).

## Share as a mod ZIP

1. Open your changed data file and save it with **Ctrl+S**.
2. Open **File > Transfer Resources...**, group "Mods", and choose **Export as Mod ZIP...** (or run it from the command palette).
3. If asked, choose the original data file ("Choose the original data file (before your changes)").
4. In "Save the mod as", enter a name for the ZIP.
5. Wait for "Packaging mod". The notification says "Mod saved to {path}. Edit its mod_config.json to set authors and the game ID."
6. Open `mod_config.json` in a text editor and fill in `authors` and check `game`.

Result: a ZIP with `mod_config.json` and `mod.g3mpatch`. Send it to others. They use **Mods > Apply Mod...** (see [Mods](mods.md)).

## Share as a mod pack

A pack re-applies to other versions of the game and lets the user review conflicts.

1. Open **File > Transfer Resources...**, group "Mods", and choose **Export Mod Pack...**.
2. Fill in **Name**, **Version** and **Authors (separated by commas)**. The dialog shows "The pack will hold {count} changes."
3. Press **Export** and pick the file.

Others use **Mods > Apply Mod Pack...** (see [Mod packs](mod-packs.md)).

## Share a patch only

Use **Mods > Create Patch...** for a bare `.g3mpatch` file. See [Patches](patches.md).

## Check before you send

- Apply the mod to a copy of the original: **Mods > Apply Mod...**, choose "Save the result as a new data file", and open the result.
- Do not include copyrighted game files in the ZIP. A patch holds only differences.

## See also

- [Mods and the mod base](mods.md)
- [Mod packs and Compare Data Files](mod-packs.md)
- [Patches](patches.md)
