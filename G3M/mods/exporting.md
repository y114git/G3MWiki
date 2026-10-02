# Exporting mods

Use the export action on a Library mod to create a ZIP package. The package contains its configuration and bundled files, so another G3M installation can import it into a profile.

Before sharing:

1. Check the name, authors, version, game, description, and homepage in the editor.
2. Verify that every source points to a bundled file and every destination uses the intended root.
3. Remove development files that are not needed to apply or document the mod.
4. Import the exported ZIP into a separate profile and test it against the intended game release.

Do not distribute original game resources unless their license permits it. A resource or binary patch is often preferable to a full DATA replacement. A ZIP container alone does not make its contents redistributable.

A mod export is different from a profile export. Use [Profile Manager](../features/profiles.md) to copy a whole Library setup, including its selection and steps. Use [Game Versions](../features/game-versions.md) to save an installed game's files.
