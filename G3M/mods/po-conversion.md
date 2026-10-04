# PizzaOven and CYOP/AFOM imports

G3M recognizes supported PizzaOven-style packages and CYOP/AFOM level content for Pizza Tower. GMLoader packages are detected as unsupported rather than treated as ordinary G3M packages.

## PizzaOven

Open the package through import. If the conversion dialog appears, configure the Pizza Tower game folder and choose **Start PO Conversion**.

Use original game files matching the mod's requirements. G3M prepares a temporary game copy, applies the package's PizzaOven changes there, and builds a G3M mod from that result. The configured game folder is not used as the conversion output.

The converted mod can be edited, selected in the Library, saved as a mod version, and exported. A successful conversion does not establish compatibility with other selected mods.

## CYOP and AFOM levels

Custom level imports use the game's user-data location for tower content. Check **Settings > Game > data folder** and install the framework required by the level itself.

The imported level is a Library mod with file operations, rather than a replacement for CYOP or AFOM. Select it for the play session. Normal launch restoration removes session-installed content or restores files that it replaced; a mode that keeps changes retains them.

If import cannot identify the package, use [manual installation](importing.md#files-without-configuration) and review each action's destination before saving. Use the intended tower folder for level content.
