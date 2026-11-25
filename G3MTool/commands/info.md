# info

```text
G3MTool info <target> [--verbose] [--cache <dir>] [--json]
```

`info` prints metadata of a DATA file or a patch. It never applies a patch. A target with the extension `.g3mpatch` or `.zip` is read as a patch; every other target is read as a DATA file.

| Argument or option | Short | Meaning |
| --- | --- | --- |
| `target` | none | DATA file or patch |
| `--verbose` | `-v` | Full per-resource listing (DATA) or per-type counts (patch) |
| `--cache <dir>` | none | Store and reuse the non-verbose DATA snapshot in this folder. Verbose mode always reads the DATA file |
| `--json` | none | Machine-readable output |

## DATA file

```text
G3MTool info original.win
```

Without `-v` the output has these parts: file name, size, game name, bytecode version, version; a `Resources:` table (Sprites, Sounds, Code, GameObjects, Rooms, Backgrounds, Fonts, Scripts, Shaders, Paths, Timelines, Extensions, Variables, Functions, Strings, AudioGroups, EmbTextures, TexPageItems, TexGroupInfo, Tilesets); general info (names, game ID, version numbers, branch, window size, flags, Steam ID, timestamp, targets); then short lists: variables by instance type, first and last function, code entry counts, audio groups, extensions, and the first 10 rooms of the room order. A run that reads the cache prints a shorter general info block (`GeneralInfo:`).

With `-v` a `DETAILED RESOURCE LISTING` follows the table, with one line per resource (index, name, and key properties) for sprites, sounds, backgrounds/tilesets, fonts, paths, scripts, shaders, game objects, rooms, code entries, functions, variables, strings (cut at 80 characters), embedded textures, texture page items, audio groups, extensions, and timelines. This can be very long; redirect it to a file.

`--json` prints one indented object with `File`, `Size`, `Game`, `BytecodeVersion`, `Version`, `GeneralInfo`, and `Resources` (a count per type). `-v` does not change the JSON. With a cache hit (`--cache`, non-verbose), the cached snapshot is printed instead. It has the same purpose but its own field names (for example `ResourceCounts`), so do not mix cached and uncached output in one parser.

## Patch

```text
G3MTool info interface.g3mpatch -v
```

```text
G3M Patch: interface.g3mpatch
Created: 2026-11-20T04:31:37.6076751Z
Tool: G3MTool v1.3.7

Original:
  File: orig.win
  Size: 3,996 bytes
  GMS: GM 2024.14 or later

Statistics:
  Changed: 1
  New: 0
  Deleted: 0

Resources by type:
  Rooms: 1 changed, 0 new, 0 deleted
```

`Resources by type:` appears only with `-v`. With `--json` the whole `g3mpatch.json` manifest is printed (see [Patch format](../patch-format.md)). It has no `success` field.

If the manifest cannot be read, `info` prints `Warning: <reason>` and `Attempting to show basic patch information...` on stdout, then the archive name, size, entry count, and top-level folders (with `-v`, up to 50 entry names). Exit code `0` in that case. With `--json` nothing of this is printed: the result is `{"success":false,"command":"info","error":"<reason>"}` and the exit code is `1`.

## Errors

| Message | Cause |
| --- | --- |
| `Error reading data file: <reason>` (stderr, exit code `1`) | The DATA file is missing or unreadable |
| `Failed to read patch file: <reason>` (stderr, exit code `1`) | The patch is missing or is not a ZIP archive |

To check a patch against a specific original, use [patch validate](patch.md#validate).
