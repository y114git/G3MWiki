# diff

```text
G3MTool diff <file1> <file2> [output-dir] [--full] [--cache <dir>] [--json]
```

Compares two DATA files, or two `.g3mpatch` files, and writes a Markdown report.

| Argument or option | Default | Meaning |
| --- | --- | --- |
| `file1`, `file2` | required | The two inputs |
| `output-dir` | `diff/` next to the executable | Folder for the report. The file is `diff_<yyyyMMdd_HHmmss>.md` |
| `--full` | off | Add unified text, GML, and JSON differences and deeper texture-page, reference, and asset-order detail. Slower and much larger |
| `--cache <dir>` | none | For DATA against DATA, reuse `.g3mcache` resource hashes when they exist |

```text
G3MTool diff original.win modified.win reports --full --cache cache
```

```text
Comparing files...
  File 1: C:\work\original.win
  File 2: C:\work\modified.win
  Output: C:\work\reports\diff_20261120_132128.md
Diff report created: C:\work\reports\diff_20261120_132128.md
  Differences: 1
  Mode: standard (use --full for unified text/code/JSON diffs)
```

A sprite whose frames only moved on the texture atlas, or a sound that only moved to another audio slot, is not listed as changed. Only a change of pixels, audio, or properties counts.

The standard report holds resource-level changes, changed text-file counts, resource counts, asset-order and index differences, sprite frame differences, and selected reference checks. A successful comparison returns `0` whether or not differences exist, so `diff` is not an equality test.

With `--json` the object is `{"success":true,"command":"diff","mode":"full","file1":...,"file2":...,"output":...,"differences":1,"changed":1,"new":0,"deleted":0,"textDiffs":1,"byType":{"Rooms":{"changed":1,"new":0,"deleted":0}},"warnings":[]}`. `mode` is `standard` or `full`. Standard mode with differences adds a warning that unified hunks are omitted. On failure: `{"success":false,"command":"diff","error":...}`.

Text-mode errors go to stderr with exit code `1`: `Error: File not found: <path>` and other `Error: <reason>` lines. With `--json` the error is the JSON object above. Ctrl+C cancels (exit code `130`).

Patch-to-patch comparison lists the declared and packaged resource changes. It does not show whether either patch applies to a given original.
