# Patches

A patch (`.g3mpatch`) holds the differences between an original data file and a changed one. The **Mods** menu creates, validates, merges and applies patches. G3MTool uses the same patch format.

## Commands

| Command | Needs an open file | What it does |
|---|---|---|
| **Create Patch...** | Yes | Compares the original with the open file and writes a patch. |
| **Apply Patch...** | No | Applies a patch to the open file or to a file you pick. |
| **Merge Patches...** | No | Merges two or more patches made from the same original. |
| **Validate Patch...** | No | Checks that a patch is valid, against the open file when there is one. |
| **Apply Mod...** | No | Applies a mod ZIP (see [Mods](mods.md)). |

None has a default shortcut. All work in the background with a progress bar and a Cancel button. Failures read "{what} failed: {reason}". Cancelling says "Cancelled."

## Create Patch...

1. Save your changes first: "Save your changes first: the patch is made from the file on disk."
2. Choose **Mods > Create Patch...**
3. G3MStudio needs the original. It uses the remembered copy when it has one, otherwise it asks "Choose the original data file (before your changes)" (see [The mod base](mods.md#the-mod-base)).
4. Pick the output in "Save the patch as".

Result: "Patch saved to {path}."

## Apply Patch...

With a file open, the dialog asks "Where should the result go? Applying to the current data file cannot be undone; Revert to Saved is the way back."

| Choice | Effect |
|---|---|
| **Apply to the current data file** | Changes the open data in memory. It cannot be undone and clears the undo history. Confirmed with "Apply {name} to the current data file? It changes the data in memory and cannot be undone; Revert to Saved is the way back." Save to keep it. |
| **Save the result as a new data file** (default) | Pick the original ("Original data file (the one the patch applies to)" or the open file), the patch and "Save the patched data file as". The output must be another file: "The output must be a different file from the one it is made from." |

Save or revert your changes before applying to the open file. Results: "{name} was applied to the current file. Save to keep it." or "Patched data file saved to {path}." with an **Open** action. If it fails on the open file: "The current file may be partly changed. Use Revert to Saved."

## Merge Patches...

1. Choose **Mods > Merge Patches...** The open file is the original; without one you pick it ("Original data file (the one the patch applies to)").
2. Pick two or more patches ("Patches to merge (two or more, made from the same original)"). With fewer than two: "Choose at least two patches to merge."
3. Pick the output in "Save the patch as".

Code and property changes are merged automatically. Result: "Merged patch saved to {path}: {merged} changes merged automatically, {conflicts} conflicts."

## Validate Patch...

Choose a patch ("Patch file"). Result: "{name} is a valid patch." or the error. With a file open, it is checked against that file.

## See also

- [Mods and the mod base](mods.md)
- [Mod packs and Compare Data Files](mod-packs.md)
- [Saving and backups](saving.md)
