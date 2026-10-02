# Resource patch format

A `.g3mpatch` is a ZIP archive with `g3mpatch.json` and exported changed resources. It describes the difference between original and modified GameMaker DATA.

## Manifest

| Key | Contents |
| --- | --- |
| `createdAt` | Creation timestamp |
| `tool` | Tool name and version |
| `library` | Library information when recorded |
| `original`, `modified` | Filename, size, MD5, bytecode version, GameMaker version, and general information |
| `resources` | Changes grouped by resource type |
| `statistics` | Counts of changed, new, and deleted resources and payload files |
| `applyPlan` | Application information generated for the resource changes |

Resource-type entries describe `changed`, `new`, and `deleted` resources. Payloads can include JSON metadata, GML or assembly code, images, audio, and supporting data needed to reconstruct references and resource order.

Create patches with the tool rather than writing a partial manifest by hand. Resource references, companion files, and ordering can matter even when the visible edit changes one sprite or room.

## Resource patches and exact bytes

Resource application reconstructs DATA from the recorded changes. It is intended for resource-aware editing and merging; equivalent reconstructed resources need not have identical serialized bytes.

`patch create --xdelta-fallback` adds an exact binary alternative under `Xdelta/`. That increases package size and ties the binary path to its original bytes. Use `xpatch` or `patch create --xdelta` when the binary patch itself is your desired output.

## Inspect and distribute

Use `info` for metadata, `patch validate` for validity and original compatibility, and `diff` for comparison. Editing archive contents can invalidate recorded file checks or omit required support files.

G3M's `mod_config.json` is a separate package format. A mod can bundle this patch and a configured destination. Do not rename the patch manifest to turn it into a Library mod.
