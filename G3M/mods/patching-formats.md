# Patch and file formats

| Input | Purpose | Compatibility considerations |
| --- | --- | --- |
| `.g3mpatch` | Changes GameMaker resources | Suitable for inspecting and combining resource changes; still depends on compatible game data |
| `.xdelta`, `.vcdiff` | Changes file bytes | Usually requires the exact original file the patch was made against |
| `.csx` | Runs a C# patch script | Executes code and can depend on bundled files or other scripts |
| `.win`, `.unx`, `.ios`, `.droid` | Supplies a complete GameMaker DATA file | Can replace changes from another mod; must match the game runner |
| Files and directories | Copies content with overwrite, soft, or hard operations | Destination and conflict behavior come from the mod configuration |
| Archives | Extracts content into a destination | Uses the extraction operation's soft or hard behavior |

A `.g3mpatch` is a ZIP-based patch with `g3mpatch.json`, rather than a mod package with `mod_config.json`. A Library mod can contain one or more patches alongside other files.

Use [Modding Tools](../features/modding-tools.md) to create, apply, compare, and convert supported DATA inputs. See [configuration operations](mod-config.md) for copying into game folders, user-data folders, and archive members.

Changing an extension does not convert a file. A Windows DATA file and a macOS DATA file may differ even when both are readable by the tools.
