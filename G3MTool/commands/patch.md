# patch

`patch` works with resource patches. Inputs can be GameMaker DATA files, `.g3mpatch` files, patch ZIPs that contain `g3mpatch.json`, `.xdelta`, `.vcdiff`, and `.csx` scripts. A script runs while G3MTool derives its modified DATA; run trusted scripts only.

How each input kind is turned into DATA:

| Input | Handling |
| --- | --- |
| DATA file (`.win`, `.ios`, `.droid`, `.unx`) | Read and validated as is |
| `.xdelta`, `.vcdiff` | Applied to the original with xdelta, then validated |
| `.g3mpatch`, `.zip` | Applied to the original, then validated |
| `.csx` | Run against the original with `Data` loaded, then the saved result is validated. See [CSX scripting](../csx-scripting.md) |
| anything else | Error: `Unsupported patch input '<name>' (<ext>). Supported: .g3mpatch, .xdelta, .vcdiff, .csx and GameMaker data files.` |

External audio groups (`audiogroupN.dat`) listed in the DATA are copied next to a written output when they are missing there.

## Create

```text
G3MTool patch create <original> <modified> [output] [--xdelta] [--xdelta-fallback] [--cache <dir>]
```

| Argument or option | Default | Meaning |
| --- | --- | --- |
| `original` | required | Original DATA file |
| `modified` | required | DATA file, `.g3mpatch`, `.xdelta`, `.vcdiff`, or `.csx` input |
| `output` | `patch_<yyyyMMdd_HHmmss>.g3mpatch` next to the executable | Output patch. An existing file is replaced |
| `--xdelta` | off | Write an xdelta patch instead of a `.g3mpatch` (default name ends in `.xdelta`) |
| `--xdelta-fallback` | off | Embed an exact xdelta copy in the `.g3mpatch`. Larger file. Cannot be combined with `--xdelta` |
| `--cache <dir>` | none | Read and write `.g3mcache` analysis files in this folder. A cache entry is ignored when the source file size or MD5 no longer matches |

```text
G3MTool patch create original.win modified.win interface.g3mpatch
G3MTool patch create original.win tweak.csx tweak.g3mpatch
G3MTool patch create original.win modified.win exact.g3mpatch --xdelta-fallback
```

Text output:

```text
Patch created successfully: C:\mods\interface.g3mpatch
  Changed: 1 (1 files)
  New:     0
  Deleted: 0
```

With `--json` the result is `{"success":true,"command":"patch create","original":...,"modified":...,"output":...,"xdeltaFallback":false,"statistics":{"totalChanged":1,"totalNew":0,"totalDeleted":0,"totalChangedFiles":1,"totalNewFiles":0},"warnings":[]}`. With `--xdelta`, the result has `output` and `details.format` set to `xdelta`, and the text output is `Patch created successfully: <output>`.

Messages:

| Message | Cause |
| --- | --- |
| `--xdelta and --xdelta-fallback are mutually exclusive.` | Both flags given |
| `Original data file not found` | The original path does not exist |
| `Failed to create patch: <reason>` | The library failed |

The library prints a warning, and still creates the patch, when the two DATA files differ in bytecode version, display name, or (substantially) room order count.

## Apply

```text
G3MTool patch apply <data> <patch> [output] [--xdelta-fallback] [--cache <dir>]
```

| Argument or option | Default | Meaning |
| --- | --- | --- |
| `data` | required | Original DATA file |
| `patch` | required | `.g3mpatch`, `.zip`, `.xdelta`, `.vcdiff`, `.csx`, or a DATA file |
| `output` | file with the DATA file's name next to the executable | Output DATA. Written through a temporary file, so a failure keeps an existing output |
| `--xdelta-fallback` | off | Use the embedded xdelta copy of a `.g3mpatch` (see below) |
| `--cache <dir>` | none | Reuse `.g3mcache` analysis when the patch input must be converted first |

Without `--xdelta-fallback` the embedded xdelta copy is not read. With it, G3MTool tries the embedded copy first (it is skipped with a warning when the DATA file's MD5 differs from the recorded original). If that fails it continues with the normal resource apply. After a normal apply it compares the output MD5 with the recorded modified MD5 and, on a difference, tries the embedded copy.

```text
G3MTool patch apply original.win interface.g3mpatch test/patched.win
G3MTool patch apply original.win delta.xdelta test/patched.win
```

Text output ends with `Patch applied successfully: <output>`. The JSON result is `{"success":true,"command":"patch apply","output":...,"details":{"inputKind":"g3mpatch","data":...,"patch":...,"xdeltaFallback":false},"warnings":[]}`. `inputKind` is `xdelta` or `csx` for those inputs.

Messages:

| Message | Cause |
| --- | --- |
| `Data file not found: <path>` | Original missing |
| `Patch file not found: <path>` | Patch missing |
| `Invalid patch: g3mpatch.json not found or failed to parse manifest` | Archive without a usable manifest |
| `Invalid patch JSON in '<entry>': <reason>` | A JSON entry in the archive is malformed |
| `Unsupported resource type in patch: '<type>'` | The manifest names an unknown resource type |
| `Invalid patch: CodeEntries present without variables_functions.json or code_patch_metadata.json` | Code entries without their metadata |
| `Patch apply failed: <n> resource type(s) failed during import; output was not written` | Import of some resource types failed |
| `No resources were applied successfully` | Nothing could be applied |
| `Integrity validation failed: <details>` | The patched data failed its checks |
| `Saved data validation failed: <details>` | The written file did not read back |
| `Failed to apply patch: <reason>` | Any other failure |

Warning `Data file hash doesn't match original - patch may not apply correctly` appears in `patch validate` when `--data` is not the original the patch was made from.

## Validate

```text
G3MTool patch validate <patch> [--data <file>] [--cache <dir>]
```

`--data` (short `-d`) is an optional DATA file. Validation reads the manifest, and with `--data` compares the file's MD5 with the recorded original and prints the warning above on a mismatch. It does not apply the patch. Exit code `0` means the archive and manifest are valid, not that the patch applies.

```text
G3MTool patch validate interface.g3mpatch --data original.win
```

```text
Validating G3M patch: C:\mods\interface.g3mpatch
Patch is valid.
  Tool: G3MTool v1.3.6
  Created: 2026-11-20T04:31:37.6076751Z
  Resources: 1 changed, 0 new, 0 deleted
```

The JSON result has `success`, `command`, `patch`, `data`, `tool`, `createdAt`, `original`, `modified`, `statistics`, `applyPlan`, `resourceTypes` (counts per resource type), and `warnings`.

Messages: `Patch file not found: <path>`, `Invalid patch: g3mpatch.json not found`, `Invalid patch: g3mpatch.json is too large`, `Invalid patch: failed to parse manifest`, `Failed to read patch: <reason>`.

## Merge

```text
G3MTool patch merge <original> <patch1> <patch2> [patch3 ...] [options]
```

Two to 100 inputs, lowest priority first. Every input is derived from the same original, then merged. Inputs can be `.g3mpatch`, `.xdelta`, `.vcdiff`, `.csx`, or DATA files.

| Option | Short | Meaning |
| --- | --- | --- |
| `--out <path>` | `-o` | Write the merged `.g3mpatch` here |
| `--apply <path>` | `-a` | Also write the merged DATA file here |
| `--code` | none | Git-style 3-way merge for GML code files |
| `--properties` | none | Deep merge for JSON property files |
| `--sequential` | none | Low-memory pipeline. Error `Sequential merge does not support code or property merging.` with `--code` or `--properties` |
| `--report <path>` | `-r` | Write a Markdown merge report |
| `--cache <dir>` | none | Reuse `.g3mcache` analysis files |

With neither `--out` nor `--apply`, the patch is written to `merged_<yyyyMMdd_HHmmss>.g3mpatch` in the folder of the original DATA file. Without `--report`, a report named `<output>.merge_log.md` is still written when conflicts are found (next to the DATA output if `--apply` is given, otherwise next to the patch).

```text
G3MTool patch merge original.win base.g3mpatch ui.g3mpatch --apply merged.win --out merged.g3mpatch --report merge.md
```

```text
Merged 2 patches.
  Patch:  merged.g3mpatch
  Data:   merged.win
  Conflicts: 0, auto-merged: 0
```

The library also prints progress and a line such as `Merge complete in 0.6s: 3 files + 0 GML + 0 ASM, 0 conflicts, 0 auto-merged`. The JSON result has `success`, `command`, `original`, `patches`, `output`, `applied`, `conflicts`, `autoMerged`, and `warnings` (one entry when conflicts exist). `output` and `applied` repeat the paths as given.

Messages: `Required argument 'patches' missing for command: 'merge'.` (fewer than two inputs), `Original file not found: <path>`, `Patch not found: <path>`, `Merge failed: <reason>`, `Merge succeeded but apply failed: <reason>`.

## Batch operations

Batch commands run independent jobs one after another against one original. All input files are hashed first. A job whose inputs are all `.g3mpatch` files and that repeats an earlier identical job is not run again: its outputs are copied and the item is marked `dedup`. Each command accepts one to 1000 inputs.

Shared options:

| Option | Meaning |
| --- | --- |
| `--out-dir <dir>` | Required for `create` and `apply`. Output folder, created when missing |
| `--cache <dir>` | Reuse `.g3mcache` analysis files |
| `--continue-on-error` | Run the remaining jobs after a failure. Without it the batch stops at the first failed job |
| `--xdelta-fallback` | `apply`: try the embedded xdelta copy (same rule as [apply](#apply)). `create`: embed one |

```text
G3MTool patch batch create <original> <modified...> --out-dir <dir> [--xdelta] [--xdelta-fallback] [--cache <dir>] [--continue-on-error]
G3MTool patch batch apply <original> <patches...> --out-dir <dir> [--xdelta-fallback] [--cache <dir>] [--continue-on-error]
G3MTool patch batch merge <original> <sets...> [--apply <dir>] [--out <dir>] [--code] [--properties] [--report] [--cache <dir>] [--continue-on-error]
```

Output names (a number suffix `_2`, `_3`, ... is added when a name is taken, so earlier outputs are never replaced):

| Command | Output |
| --- | --- |
| `batch create` | `<input-name>.g3mpatch`, or `.xdelta` with `--xdelta` in `--out-dir` |
| `batch apply` | `<original-name>_<input-name><original-extension>` in `--out-dir` (`.win` if the original has no extension) |
| `batch merge` | `merge_<nnn>_<first five input names joined by _>` with the original's extension in `--apply` (default: current folder); the same name with `.g3mpatch` in `--out` when given; with `--report`, `<name>.merge_log.md` next to the last output |

`--xdelta` and `--xdelta-fallback` together fail with `--xdelta and --xdelta-fallback are mutually exclusive.`

A merge set is one quoted argument: a comma-separated list, lowest priority first, at least two paths. Semicolons are rejected (`Merge sets use comma separators only; semicolons are not supported.`). Batch merge always writes the merged DATA.

```text
G3MTool patch batch merge game.win "base.xdelta,ui.g3mpatch" "a.win,b.xdelta,c.g3mpatch" --apply data --out patches --report
```

Text output:

```text
Batch complete: 2/2 succeeded, 0 failed, 1 deduplicated
  [1] apply OK (0.7s)
      C:\work\out\orig_p1.win
  [2] apply OK dedup (0.0s)
      C:\work\out\orig_p1_2.win
```

A failed item prints `FAIL` and `error: <message>`. The exit code is `1` when any item failed. JSON: `success`, `command`, `total`, `completed`, `failed`, `deduplicated`, and `items` with `index`, `kind`, `inputs`, `outputs`, `success`, `deduplicated`, `error`, `seconds`.

Problems found before any job starts (missing original, missing input file, bad merge set) stop the command with `Error: <message>` (a JSON object with `success: false` and `error` under `--json`) and exit code `1`. The text names the file or rule: `Original data file not found: <path>`, `patch file not found: <path>`, `modified file not found: <path>`, `merge patch file not found: <path>`, `Each merge set must contain at least two paths.`

Ctrl+C stops the batch (exit code `130`).

See also: [Automation](../automation.md), [Patch format](../patch-format.md).
