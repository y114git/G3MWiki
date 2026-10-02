# Modding Tools

Open **Modding Tools** for five tabs: **Convert DATA Files**, **Patch**, **Merge**, **Info**, and **Diff**. These operate on GameMaker resource files and patch inputs through G3MTool.

Use separate output paths when you want to keep your originals. Producing a patch or DATA file does not automatically add it to the Library unless the specific action saves a mod version.

## Convert DATA Files

Select a profile, choose the target format, and check the installed mods to convert. Target choices are `g3mpatch`, `xdelta`, `data.win`, `game.ios`, `game.unx`, and `game.win`.

Click **Run** to convert eligible patch/DATA operations into a saved version of each selected mod. Open that mod's [Mod Versions](../mods/mod-versions.md) and choose Switch to use the result. The current mod is not directly replaced by the conversion action.

Configure the relevant game path first: conversion needs the correct original DATA file. Changing the output's filename or extension does not make a Windows game or incompatible patch work on another platform.

## Patch

Choose the format **Mode** and action **Create**, **Apply**, or **Convert**.

| Action | Inputs | Output |
| --- | --- | --- |
| Create | Original DATA and modified DATA or supported patch/script input | A patch representing their differences |
| Apply | Original DATA and a patch, script, or DATA input | Patched DATA |
| Convert | Original DATA and a source patch | Selected output patch format |

Browse for each required path and click **Run**. Supported inputs include `.g3mpatch`, patch ZIPs with `g3mpatch.json`, `.xdelta`, `.vcdiff`, `.csx`, and readable GameMaker DATA files.

For supported `.g3mpatch` create/apply actions, **Batch mode** uses one original and a list of inputs. **Add** and **Remove** manage the list. Choose an output folder; each item is processed independently against that original.

**Continue on error** lets later batch items run after a failed item. **Embed xdelta fallback** stores an exact binary fallback when creating a resource patch; it increases package size. When applying with that option, G3MTool can use the embedded binary path. It is not a solution for mismatched original bytes.

## Merge

Choose an original DATA file and add at least two inputs. **The G3M list is highest priority first.** Use **Up** and **Down** to arrange the list. Inputs can mix supported resource patches, binary patches, scripts, and modified DATA files.

Set the DATA output and optionally the merged `.g3mpatch` output, then click **Merge**. Each input derives changes against the same original; the action does not apply the list as separate sequential steps.

- **Code merge** attempts to combine overlapping game-code changes.
- **Properties merge** combines supported property changes, including structured JSON changes.
- **Report** writes a merge report for inspecting decisions and conflicts.

In **Batch mode**, build a file list and click **Add set**. Repeat for each independent combination. **Remove set** removes the selected combination. Set the DATA output folder and optional patch output folder, then run the batch. Continue on error affects whether later sets run after a failure.

G3M's list order differs from the [G3MTool CLI](../../G3MTool/commands/patch.md), which takes lower-priority inputs first.

## Info

Select a DATA file or patch and click **Get Info**. The output area displays metadata and resource information. **Verbose** requests additional details.

## Diff

Select the two files and click **Compare**. G3M generates and opens a difference report where possible. **Full report** includes detailed resource comparisons; it can take longer and produce a larger report.

## Errors

Read the status and application log if an action fails. Check input compatibility, output permissions, configured tool paths, and xdelta availability. Clearing the G3MTool cache removes reusable analysis data, not installed mods. Use [Diagnostics](diagnostics.md) to inspect a complete launch selection rather than a pair of manually selected files.
