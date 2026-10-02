# mod_config.json reference

`mod_config.json` describes a mod's identity, compatibility, bundled files, and installation actions. It sits at the root of the mod folder or ZIP. The supported configuration version is `2.0.0`.

For an interface that writes this file, see [Mod editor](mod-editor.md). For a complete packaging walkthrough, see [Create a mod](creating-mods.md).

For complete examples with loose files and multiple chapters, see [Package examples](package-examples.md).

## Minimal patch mod

This package applies `patch.g3mpatch` to UNDERTALE's data file. The patch must match the player's game data.

```text
my-mod.zip
├── mod_config.json
└── patch.g3mpatch
```

```json
{
  "config_version": "2.0.0",
  "id": "my_mod",
  "name": "My mod",
  "version": "1.0.0",
  "authors": ["Your name"],
  "game": "undertale",
  "files": [
    {
      "source": "${mod_path}/patch.g3mpatch",
      "target": "${game_path}/data.win",
      "type": "patch"
    }
  ]
}
```

## Top-level fields

| Field | Required | Meaning |
| --- | --- | --- |
| `config_version` | Yes | Exactly `"2.0.0"`. |
| `id` | Yes | Stable identifier for the mod, also used in dependencies and conflicts. |
| `name` | Yes | Display name in the library. |
| `version` | Yes | Nonempty version label. It is a string, not a JSON number. |
| `authors` | Yes | Array of author names. An empty array is accepted. |
| `game` | Yes | Game identifier. |
| `files` | Yes | Ordered array of operations and groups. An empty array describes a package without installation actions. |
| `description` | No | Description, with line breaks if needed. |
| `homepage` | No | Absolute HTTP or HTTPS URL. |
| `icon` | No | Bundled image path such as `${mod_path}/icon.png`, or an HTTP or HTTPS image URL. |
| `game_version` | No | Human-readable label for the intended game version. It does not replace a file hash check. |
| `tags` | No | Array containing `textedit`, `customization`, `gameplay`, `other`, or `CYOP/AFOM`. |
| `placeholders` | No | Named paths built from the standard path roots. |
| `dependencies` | No | Array of required mod IDs, optionally with an ordering condition. |
| `conflicts` | No | Array of incompatible mod IDs, optionally with an ordering condition. |

Unknown fields are rejected. Omit optional values that you do not use. Empty `tags`, `placeholders`, `dependencies`, and `conflicts` collections are not accepted. Optional placeholders and relationship collections also accept `null`.

IDs contain lowercase ASCII letters, digits, underscores, or hyphens. They begin with a letter and contain at most 64 characters. This rule applies to both `id` and `game`. Do not change a published mod's ID when issuing another version.

Built-in game IDs are `deltarune`, `deltarunedemo`, `undertale`, `undertaleyellow`, `pizzatower`, `sugaryspire`, and `frickbears3`. A custom game's ID comes from [Game Manager](../games/game-manager.md).

## Path roots

| Root | Location |
| --- | --- |
| `${mod_path}` | Installed folder of this mod. |
| `${game_path}` | Selected game's installation folder. |
| `${game_data_path}` | Selected game's user-data folder. This is distinct from the installation folder. |
| `${user_path}` | Player's home folder. |

Use forward slashes on every platform. A path starts with one placeholder and includes a suffix below that root. For example, `${game_path}/data.win` is a file and `${game_path}/textures/` is a directory. The final slash matters when specifying a directory destination.

Targets cannot point into `${mod_path}`. Sources can use any supported root. Raw absolute paths are accepted, but are machine-specific and receive a warning when used in operations. Avoid them in distributed mods.

Paths cannot contain traversal components such as `..`, network or device paths, backslashes, reserved filenames, or placeholders in the middle of a path.

For DELTARUNE, a chapter target such as `${game_path}/chapter1_windows/data.win` identifies that chapter. G3M maps `chapter1_windows` to `chapter1_mac` for a macOS runtime. A package can contain actions for several chapters.

### DATA filenames and game runtimes

G3M adjusts DATA target filenames to the game's runtime:

| Runtime | Target for `data.win`, `game.unx`, or `game.ios` |
| --- | --- |
| Windows | `data.win` |
| Linux | `game.unx` |
| macOS | `game.ios` |

A Windows game launched through Wine uses the Windows target, even on a Linux host. Other filenames ending in `.win`, `.unx`, or `.ios` keep their base name and receive the runtime's extension. DELTARUNE chapter folder names also follow the runtime.

This adjustment applies to targets, including archive members. Bundled source paths keep their specified names. Renaming the target does not convert patch content or make a patch compatible with another game release. Test each runtime you claim to support.

### Custom placeholders

```json
{
  "chapter_assets": "${game_path}/chapter1_windows/assets",
  "bundled_textures": "${mod_path}/textures"
}
```

These are values for the top-level `placeholders` object. An operation can then use `${chapter_assets}/logo.png` or `${bundled_textures}/logo.png`.

A custom name begins with an ASCII letter and contains only letters, digits, or underscores, up to 64 characters. Names are unique without regard to case and cannot reuse a built-in root name. Each value uses one built-in root followed by a path. Custom placeholders cannot refer to other custom placeholders.

A placeholder based on `${mod_path}` remains a source-only path. Giving it another name does not permit writing into the installed mod.

## Operations

Each operation has a `source` and a `type`. All types except `info` require `target`. Optional `source_hash` and `target_hash` fields contain file or directory checksums.

| Type | Action |
| --- | --- |
| `patch` | Apply a supported patch or data-file input to an existing target data file. |
| `overwrite` | Copy a file or folder, replacing matching destination files and retaining unrelated files. |
| `soft-overwrite` | Copy only items that do not already exist at the destination. |
| `hard-overwrite` | Clear the affected destination before copying. See the clearing rules below. |
| `extract` | Copy the contents of a source folder or unpack an archive into the target directory. |
| `soft-extract` | Extract while retaining existing destination items. |
| `hard-extract` | Clear the target directory, then extract into it. |
| `info` | Make a bundled document available as mod information. It does not modify the game. |

### Copy a file

```json
{
  "source": "${mod_path}/textures/logo.png",
  "target": "${game_path}/assets/logo.png",
  "type": "overwrite"
}
```

If the target is `${game_path}/assets/`, G3M uses the source filename and writes `assets/logo.png`.

### Copy a folder or its contents

`overwrite` copies the source folder itself when the target ends in `/`. Copying `${mod_path}/textures/` to `${game_path}/assets/` creates `assets/textures/`.

`extract` copies the folder's contents. Extracting the same source into `${game_path}/assets/` puts the textures directly in `assets/`.

The soft variants retain existing files and existing conflicting objects. They do not mean "replace only changed files."

### Clear a destination

The hard variants remove destination content within the mod operation's backup-and-restore session:

- A file `hard-overwrite` to a directory clears that directory before copying the file.
- A file `hard-overwrite` to a filename clears its parent directory before copying.
- A folder `hard-overwrite` to a directory replaces the destination subfolder named after the source folder.
- A folder `hard-overwrite` to a path without a final slash clears that path's parent before copying to the requested path.
- `hard-extract` clears the target directory before placing the source contents there.

Several hard operations clearing the same destination during one application clear it once, so later actions do not erase earlier actions in that sequence. Use a dedicated destination folder. A hard operation against the game root can remove files the game needs.

Normal launch restores managed changes after the game exits. [Keep changes and patching-only modes](../games/launch-modes.md) intentionally retain them.

### Add documentation

```json
{
  "source": "${mod_path}/README.md",
  "type": "info"
}
```

Supported documentation extensions are `.md`, `.markdown`, `.html`, `.htm`, `.pdf`, and `.txt`. An `info` source must be a file inside the mod folder, directly or through a custom placeholder. It cannot have `target` or `target_hash`.

Files without an operation still belong to the package. Include script dependencies, images, and helper files without inventing copy actions for them.

## Order and groups

G3M processes `files` in array order. A group is an object with exactly one name whose value is another operation array:

```json
[
  {
    "Chapter 1": [
      {
        "source": "${mod_path}/chapter1.g3mpatch",
        "target": "${game_path}/chapter1_windows/data.win",
        "type": "patch"
      },
      {
        "source": "${mod_path}/chapter1_assets/",
        "target": "${game_path}/chapter1_windows/",
        "type": "extract"
      }
    ]
  },
  {"source": "${mod_path}/README.md", "type": "info"}
]
```

Groups organize actions without adding a separate installation step. G3M visits a group's actions before continuing to the next sibling. Names must be nonempty, unique throughout the configuration without regard to case, and at most 128 characters. `source` and `type` are reserved group names.

These operation groups are separate from the library's [mod steps and priorities](modpacks.md), which arrange whole mods.

## Archive-member paths

An operation can refer to a file or folder inside an archive by appending its member path:

```json
{
  "source": "${mod_path}/resources.zip/images/logo.png",
  "target": "${game_path}/assets.zip/images/logo.png",
  "type": "overwrite"
}
```

Use a final slash for an archive directory, such as `${game_path}/assets.zip/images/`. This path addresses archive members; it does not mean a directory on disk named `assets.zip`.

Readable archive formats are ZIP, 7z, RAR, TAR, compressed TAR variants, and LZMA streams. RAR members can be sources but cannot be targets. A standalone `.lzma` stream has no addressable members. Nested archive-member paths are not supported.

G3M rewrites a writable target archive after modifying its contents. For packing one file into an LZMA stream, `extract` and `soft-extract` accept a file source and a `.lzma` target. `hard-extract` does not support this case.

## Hash checks

Both hash fields use `sha256:` followed by 64 lowercase hexadecimal digits:

```text
sha256:0123456789abcdef0123456789abcdef0123456789abcdef0123456789abcdef
```

`source_hash` checks the supplied source before use. `target_hash` checks the existing destination before the action changes it. A target hash therefore refers to the data at that point in the sequence, not necessarily an untouched installation. A missing hashed target or a mismatch stops application.

For a file, the hash is SHA-256 of its bytes. For a folder, G3M includes normalized relative names, file contents, and directories, including empty directories. A ZIP-member hash checks the extracted member, not the entire ZIP. Use the editor's hash controls for folders and archive members.

To calculate a file hash outside the editor:

```python
from hashlib import file_digest

with open("patch.g3mpatch", "rb") as source:
	print("sha256:" + file_digest(source, "sha256").hexdigest())
```

## Dependencies and conflicts

A relationship names the other mod, not a version or a filename. A bare ID requires that mod to be selected, or declares an incompatible selection when placed in `conflicts`.

Ordering suffixes refer to the related mod's position relative to the mod containing the declaration:

| Suffix | Condition |
| --- | --- |
| `before` | The related mod is in an earlier step, or a higher row of the same step. |
| `after` | The related mod is in a later step, or a lower row of the same step. |
| `before-step` | The related mod is in an earlier step. |
| `after-step` | The related mod is in a later step. |
| `before-priority` | Both mods are in the same step and the related mod has lower priority, below this mod. |
| `after-priority` | Both mods are in the same step and the related mod has higher priority, above this mod. |

For example, `"dependencies": ["base_mod:before-step"]` requires `base_mod` in an earlier step. `"conflicts": ["other_mod:after"]` reports a conflict where `other_mod` occurs later. Conditional conflicts do not prohibit every possible arrangement of those two mods.

Dependencies use AND: all entries must be satisfied. Any matching conflict produces a reported issue. Missing or inactive dependencies and unsatisfied ordering are also reported when inspecting a selection. These findings include warnings; they are not all unconditional launch blockers. Duplicate relationships and self-references are invalid. A dependency cycle or contradictory ordering cannot be solved by adding another step.

The library offers relationship inspection and arrangement. An arrangement can reorder selected mods; it cannot install a missing dependency or make incompatible patches compatible.

## Limits and validation

| Item | Limit |
| --- | --- |
| Configuration file | 4 MiB, UTF-8 JSON. |
| JSON nesting | 64 levels. |
| Operations | 5,000. |
| Groups | 2,000, with up to 32 nested group levels. |
| Custom placeholders | 128. |
| Dependencies or conflicts | 512 entries per collection. |
| Authors | 64 entries, up to 128 characters each. |
| Name, version, group name, game-version label | 128 characters. |
| Description | 16,384 characters. |
| URL | 2,048 characters. |
| Operation path | 4,096 characters. |

JSON duplicate keys, unknown fields, invalid types, unsupported operation names, and unsafe paths are rejected. Display strings use Unicode NFC normalization. A structurally valid configuration can still fail if a bundled file is missing, the game version is wrong, or selected mods conflict. Test the packaged ZIP through G3M before publishing it.
