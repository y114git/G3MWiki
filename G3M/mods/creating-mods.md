# Create a mod

This walkthrough creates a ZIP containing a data-file patch and a README. It assumes that you have an unmodified game data file and a modified copy from the same game version.

## Create the patch

1. Open **Modding Tools** and select its patch-creation controls.
2. Select the unmodified data file as the original.
3. Select the modified data file as the input.
4. Choose a `.g3mpatch` destination and run the operation.

The command-line equivalent is:

```sh
G3MTool patch create original.win modified.win my_mod.g3mpatch
```

The original is the base your players need. Creating a patch from an already modified base makes that modification a requirement for applying the patch. Keep the original and modified files separate while preparing the mod.

For a byte-exact XDelta patch, select XDelta output or use `G3MTool xpatch create`. A resource patch can combine changes from multiple mods, but resource conflicts still need review. See [Patching formats](patching-formats.md).

## Build the package

1. Open **Add Mod** and create a mod, or use the [web editor](web-editor.md).
2. In **Metadata**, select the game and enter a name, authors, and version.
3. Add the patch file to the package.
4. In **Files**, add a `patch` operation using `${mod_path}/my_mod.g3mpatch` as its source.
5. Set its target to the appropriate game data file, such as `${game_path}/data.win` for UNDERTALE.
6. Add `README.md` and an `info` operation with `${mod_path}/README.md` as its source.
7. Save the mod, then export its ZIP.

The resulting package has this structure:

```text
my_mod.zip
├── mod_config.json
├── my_mod.g3mpatch
└── README.md
```

Describe the supported game version, required mods, installation choices, and known conflicts in the README. An icon, homepage, and relevant tags help users identify the mod in their library.

## Add more than a data patch

Use `overwrite` for loose replacement files and `extract` for a folder's contents or an archive. Use a soft variant when existing destination files must remain. Use a hard variant only when the mod needs to replace a dedicated destination's complete contents.

A script's helper files can be bundled without operations. An `info` operation is for user-readable documentation, not a declaration that a script may access a file.

For DELTARUNE, give each chapter action an explicit chapter target. For example, `${game_path}/chapter1_windows/data.win` and `${game_path}/chapter2_windows/data.win` can appear in one configuration. Named groups make those actions easier to edit.

## Declare requirements

Add dependencies and conflicts on the **Compatibility** tab. Use stable mod IDs. If a script expects the output of a base mod, require that base in an earlier library step.

Use a target hash for a strict file-version requirement. A readable **Game version** label is useful information, but does not enforce the bytes of the target.

## Test the ZIP

Import the exported ZIP into a separate testing profile. Launch the intended game with the mod selected, check its behavior, and verify restoration after exit. Then test any claimed multi-mod arrangement and each chapter the package changes.

Test the exported package rather than the original editing folder. This catches omitted helper files, wrong package-relative paths, and icons or documents that exist only on your computer.

If the mod contains direct absolute paths, replace them with placeholders unless the package is deliberately machine-specific. Do not include a game executable or unrelated game assets merely to make a missing path disappear.
