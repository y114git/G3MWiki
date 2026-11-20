# Resource patch format

A `.g3mpatch` file is a ZIP archive. It holds a manifest, `g3mpatch.json`, and the exported files of every resource that differs between an original and a modified GameMaker DATA file. This page describes the format as G3MLib 1.1.5 writes and reads it. Create patches with G3MTool instead of writing them by hand.

## Archive layout

```text
interface.g3mpatch
  g3mpatch.json
  Rooms/room_first/room.json
  CodeEntries/gml_Object_obj_a_Create_0/gml_Object_obj_a_Create_0.gml
  CodeEntries/gml_Object_obj_a_Create_0/gml_Object_obj_a_Create_0.asm
  Helpers/asset_order.txt
  Xdelta/<name>.xdelta
```

| Path | Contents |
| --- | --- |
| `g3mpatch.json` | The manifest (UTF-8 JSON, a leading byte order mark is accepted) |
| `<ResourceType>/<resource name>/<file>` | Exported files of one changed or new resource, for example `Rooms/room_first/room.json`. The exact paths are listed in the manifest (`files`) |
| `CodeEntries/<key>/<key>.gml` and `<key>.asm` | Code of changed or new code entries, as GML text and/or assembly text. Child entries add `CodeEntries/<key>/<child>.asm` |
| `Helpers/` | Supporting data needed to rebuild references and order, for example `asset_order.txt`. Older patches name this folder `AssetOrder/`; both are read |
| `Xdelta/<name>.xdelta` | Optional exact binary copy, present only when the patch was created with `--xdelta-fallback` (**Use XDelta fallback**). Older patches use `Exact/` |

A patch without differences in a type has no folder for it. Entry names are case-insensitive when read.

## Manifest

The manifest is indented JSON with these keys.

| Key | Contents |
| --- | --- |
| `createdAt` | UTC creation time, ISO 8601 (`2026-11-20T04:31:37.6076751Z`) |
| `tool` | `name` and `version` of the program that wrote the patch (`G3MTool`, `1.3.5`) |
| `library` | `name` and `version` of G3MLib (`G3MLib`, `1.1.5`). Omitted when not recorded |
| `original`, `modified` | Description of each DATA file (below) |
| `resources` | Changes grouped by resource type (below) |
| `statistics` | `totalChanged`, `totalNew`, `totalDeleted`, `totalChangedFiles`, `totalNewFiles` |
| `applyPlan` | How the patch is applied (below) |

`original` and `modified`:

| Key | Contents |
| --- | --- |
| `filename`, `size`, `md5` | Name, size in bytes, and MD5 (lowercase hex) of the file |
| `bytecodeVersion` | Bytecode version number |
| `gmsVersion` | GameMaker version text, for example `GM 2024.14 or later` |
| `generalInfo` | `displayName`, `name`, `fileName`, `config`, `gameID`, `directPlayGuid`, `major`, `minor`, `release`, `build`, `defaultWindowWidth`, `defaultWindowHeight`, `infoFlags`, `licenseCRC32`, `timestamp`, `activeTargets`, `functionClassifications`, `steamAppID`, `debuggerPort`, `gms2FPS`, `gms2AllowStatistics`, `roomOrderCount` |

`resources` maps a resource type name (for example `Rooms`) to an object with three lists: `changed` and `new` hold `{"name": ..., "files": {"<label>": "<path in the archive>"}}`, and `deleted` holds resource names.

```jsonc
"resources": {
  "Rooms": {
    "changed": [ { "name": "room_first", "files": { "room.json": "Rooms/room_first/room.json" } } ],
    "new": [],
    "deleted": []
  }
}
```

`applyPlan` has `mode` (`standard`), the flags `requiresCodePipeline`, `requiresTexturePipeline`, `requiresAssetReorder`, `requiresHeavyFinalize`, `supportsDirectResourceApply`, and the lists `simpleResourceTypes` and `heavyResourceTypes`. Applying uses the plan when present and computes it from `resources` when missing.

## Safety checks and limits

G3MLib checks every archive before use.

| Check | Limit | Error |
| --- | --- | --- |
| Entry names | No leading `/` or `\`, no `:`, no `..` segment | `Archive entry '<name>' has an unsafe name.` |
| Manifest | At most 64 MiB, must be present and valid JSON for the manifest type | `Invalid patch: g3mpatch.json not found`, `Invalid patch: g3mpatch.json is too large`, `g3mpatch.json is not a valid patch manifest: <reason>`, `g3mpatch.json is empty.` |
| One entry or one archive file | At most 2,147,483,591 bytes (0x7FFFFFC7) | `Archive entry '<name>' exceeds maximum allowed size.`, `Payload file exceeds maximum allowed size of <n> bytes.` |
| Total declared size of all entries | At most 4 GiB | `Archive entries declare more than the maximum allowed total size.` |
| Number of entries when extracting to disk | At most 500,000 | `Archive holds too many entries.` |
| Sizes while reading | Counted while reading, so a false size in a header does not help | `Archive entries hold more than the maximum allowed total size.` |

Before applying, G3MLib also checks that every `.json` entry parses (`Invalid patch JSON in '<entry>': <reason>`), that every resource type is known (`Unsupported resource type in patch: '<type>'`), and that code entries come with their metadata (`Invalid patch: CodeEntries present without variables_functions.json or code_patch_metadata.json`). After patching, the result is checked again (`Integrity validation failed: <details>`) and the written file must read back (`Saved data validation failed: <details>`). If any resource type fails, no output is written.

`patch validate` checks the archive and manifest only. It warns when the MD5 of `--data` differs from `original.md5`. A patch made for another game release can still validate.

## Resource patches and exact bytes

Applying a resource patch rebuilds DATA from recorded changes. The result is equivalent in resources but need not match the modified file byte for byte. With an embedded `Xdelta/` copy and **--xdelta-fallback**, the exact path is available: it is used only if the DATA file's MD5 equals `original.md5`, and its output is checked against `modified.md5`. The copy makes the patch larger and ties that path to the original bytes. To distribute a binary patch only, use [xpatch](commands/xpatch.md).

## Inspect and distribute

Use [info](commands/info.md) for the manifest summary, [patch validate](commands/patch.md#validate) for validity, and [diff](commands/diff.md) for comparison. Editing archive contents by hand can break the recorded paths or required support files.

G3M's `mod_config.json` is a separate package format. A mod can bundle a `.g3mpatch` with a configured destination. Do not rename a patch to turn it into a Library mod.
