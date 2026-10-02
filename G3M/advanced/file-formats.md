# File reference

| File or format | Meaning | Reference |
| --- | --- | --- |
| `mod_config.json` | Mod package configuration, format `2.0.0` | [Mod configuration](../mods/mod-config.md) |
| `.g3mpatch`, `g3mpatch.json` | Resource patch archive and its manifest | [Patch format](../../G3MTool/patch-format.md) |
| `.xdelta`, `.vcdiff` | Binary patch | [xpatch](../../G3MTool/commands/xpatch.md) |
| `.csx` | Executable C# script | [Scripting](../../G3MTool/csx-scripting.md) |
| `.win`, `.unx`, `.ios`, `.droid` | GameMaker resource DATA | [Patch inputs](../mods/patching-formats.md) |
| `plugin_config.json` | Plugin manifest, integer configuration version `1`, API `1.3.0` | [Plugin development](../features/plugins-development.md) |
| `theme_config.json` | Theme manifest, format `1.0.0` | [Theme packages](../customization/theme-packages.md) |
| `game_version_data.json` | Exported game-snapshot manifest | [Game Versions](../features/game-versions.md) |
| Profile ZIP | Profile state and its mod library | [Profiles](../features/profiles.md) |

A ZIP's manifest determines its role. A mod ZIP, patch ZIP, plugin ZIP, theme ZIP, and game-version ZIP are not interchangeable.

Use `files` operations in a mod configuration to deploy content. Bundled files without an operation can serve as script dependencies or documentation without being copied to the game.

For application settings and saved-state locations, see the [data directory reference](data-directory.md).
