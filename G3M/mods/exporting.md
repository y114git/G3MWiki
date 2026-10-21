# Exporting mods

Use the export action on a Library mod to create a ZIP package. The package contains its configuration and bundled files, so another G3M installation can import it into a profile.

## Saved versions

When the mod has [saved versions](mod-versions.md), exporting first asks what to include. **Export only the active version** is on by default and produces a package without `mod_versions/`. Turn it off to choose which saved versions travel with the mod; all of them are ticked at first. If you tick none, only the active version is exported. Dragging a mod out of the Library shows the file's icon and name next to the cursor and builds the archive only after you drop it. With the question on, it appears first; because the dialog interrupts the drag, answer it and then drag the mod again. In **Settings → Library → Mod Versions**, **When exporting** can be changed from *Always ask* to *Only the active version* or *All versions*, which skips the dialog.

Before sharing:

1. Check the name, authors, version, game, description, and homepage in the editor.
2. Verify that every source points to a bundled file and every destination uses the intended root.
3. Remove development files that are not needed to apply or document the mod.
4. Import the exported ZIP into a separate profile and test it against the intended game release.

Do not distribute original game resources unless their license permits it. A resource or binary patch is often preferable to a full DATA replacement. A ZIP container alone does not make its contents redistributable.

A mod export is different from a profile export. Use [Profile Manager](../features/profiles.md) to copy a whole Library setup, including its selection and steps. Use [Game Versions](../features/game-versions.md) to save an installed game's files.
