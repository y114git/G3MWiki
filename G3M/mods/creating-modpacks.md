# Create a modpack

**Create Modpack** builds one local mod from the selected mods and their current priority steps. It is useful for keeping a tested combination as a package rather than selecting its component mods on every launch.

## Build the package

1. Configure the game folder and select at least two mods in the Library.
2. Review **Priority & Steps** and test the combination.
3. Click **Create Modpack**.
4. Enter the package name.
5. Choose whether to use **XDELTA instead of data.win/.ios**.
6. Click **OK** and wait for creation to finish.

In Chapter Mode, the action uses the active chapter's selection. Outside Chapter Mode, it includes supported sections from the shared selection.

G3M applies the selected game-file changes to a temporary game copy and bundles the result. The real installation is not the build destination. The new mod appears in the active profile's Library.

With the XDELTA option off, changed game files are bundled as replacements. With it on, eligible changed DATA files become binary patches against the configured original. Other changed files still use the required copy or extraction operations. The checkbox does not produce an xdelta patch for every payload.

Documentation and operations for destinations outside the game installation are carried into the package. Such operations still depend on their destination folders being configured when the modpack is used.

## Use and share it

Unuse the component mods before selecting the resulting modpack, unless you deliberately want to apply additional changes on top. The modpack already contains the built combination. Editing or updating a component mod does not rebuild an existing modpack.

Test the modpack alone on the intended original game files. An xdelta modpack requires matching original bytes. A full DATA replacement still needs a compatible game runner and companion resources.

Use the Library's export action to save a ZIP. Check the included files and each component author's redistribution terms before sharing it. Profile export is a different option when you want to preserve the editable component mods and their arrangement.
