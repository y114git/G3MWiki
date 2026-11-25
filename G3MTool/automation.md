# Use G3MTool in scripts

Run the CLI with explicit input and output paths, and check its exit code before using a result. Commands take their arguments directly; the [interactive prompt](commands/README.md#interactive-prompt) is not needed. Use `G3MTool.exe` on Windows and `./G3MTool` on Linux and macOS.

## Exit codes

| Code | Meaning |
| --- | --- |
| `0` | Success. A merge with conflicts also returns `0` |
| `1` | Failure of the command, invalid arguments, or a failed batch item |
| `130` | Cancelled with Ctrl+C |
| other | `execute` with a program returns that program's exit code |

## JSON results

Add `--json` to get one JSON object on stdout and no progress text. Informational output is not printed, so stdout can be parsed as is. Keep stderr separate. Do not assume that stdout is JSON from the exit code alone; parse it inside a try block.

| Command | Fields |
| --- | --- |
| `patch create` | `success`, `command`, `original`, `modified`, `output`, `xdeltaFallback`, `statistics`, `warnings`. With `--xdelta`: `success`, `command`, `output`, `details.format`, `warnings` |
| `patch apply` | `success`, `command`, `output`, `details` (`inputKind`, `data`, `patch`, `xdeltaFallback`), `warnings` |
| `patch validate` | `success`, `command`, `patch`, `data`, `tool`, `createdAt`, `original`, `modified`, `statistics`, `applyPlan`, `resourceTypes`, `warnings` |
| `patch merge` | `success`, `command`, `original`, `patches`, `output`, `applied`, `conflicts`, `autoMerged`, `warnings` |
| `patch batch ...` | `success`, `command`, `total`, `completed`, `failed`, `deduplicated`, `items` |
| `xpatch create`, `xpatch apply` | `success`, `command`, `output`, `details.format`, `warnings` |
| `execute` (script) | `success`, `command`, `script`, `data`, `saved`, `exportDirectory`, `warnings` |
| `execute` (program) | `success` (program exit code is 0), `command`, `program`, `exitCode`, `output`, `error`, `warnings` |
| `execute xdelta` | `success`, `command`, `output`, `warnings` |
| `diff` | `success`, `command`, `mode`, `file1`, `file2`, `output`, `differences`, `changed`, `new`, `deleted`, `textDiffs`, `byType`, `warnings` |
| `info` | DATA: `File`, `Size`, `Game`, `BytecodeVersion`, `Version`, `GeneralInfo`, `Resources`. Patch: the manifest. No `success` field |

A failure of G3MTool itself in `patch`, `xpatch`, `execute`, and `diff` (including `execute` failing to start a program), and `info` on a patch without a readable manifest, prints `{"success":false,"command":"<name>","error":"<message>"}` and exits with `1`. A program started by `execute` that exits with a nonzero code is not a G3MTool failure: the result is the program object above with `success` set to `false` and its `exitCode`, and G3MTool exits with that same code. DATA file errors of `info` are text on stderr, also with `--json`. Argument errors (missing argument, unknown option) and failures found before a batch starts are text, not JSON; always check the exit code first.

## PowerShell

```powershell
$text = & .\G3MTool.exe patch create original.win modified.win out\change.g3mpatch --json
if ($LASTEXITCODE -ne 0) { throw "patch create failed: $text" }
$result = ($text -join "`n") | ConvertFrom-Json
"{0} changed, {1} new, {2} deleted" -f $result.statistics.totalChanged, $result.statistics.totalNew, $result.statistics.totalDeleted
```

Check a patch and stop on a hash mismatch:

```powershell
$v = (& .\G3MTool.exe patch validate change.g3mpatch --data original.win --json) | ConvertFrom-Json
if (-not $v.success) { throw "invalid patch" }
if ($v.original.md5 -ne (Get-FileHash original.win -Algorithm MD5).Hash.ToLower()) { throw "wrong original" }
```

## Batch file

```bat
@echo off
G3MTool.exe patch apply original.win change.g3mpatch out\patched.win
if errorlevel 1 (
  echo Apply failed
  exit /b 1
)
```

## Bash

```bash
#!/usr/bin/env bash
set -euo pipefail
./G3MTool patch create original.win modified.win out/change.g3mpatch --json > result.json
./G3MTool patch apply original.win out/change.g3mpatch out/patched.win
./G3MTool info out/patched.win --json | python3 -c 'import json,sys; print(json.load(sys.stdin)["Game"])'
```

With `set -e`, a nonzero exit code stops the script. Handle `130` (Ctrl+C) explicitly if you need to clean up:

```bash
code=0
./G3MTool patch merge original.win a.g3mpatch b.g3mpatch --apply out/merged.win || code=$?
if [ "$code" -eq 130 ]; then
  echo "cancelled"
  # remove files of your own here
fi
exit "$code"
```

## Python

An argument list keeps paths with spaces intact:

```python
import json
import subprocess

run = subprocess.run(
    ["./G3MTool", "patch", "apply", "original.win", "textures.g3mpatch", "output/patched.win", "--json"],
    capture_output=True, text=True,
)
if run.returncode != 0:
    raise RuntimeError(run.stdout or run.stderr)
print(json.loads(run.stdout)["output"])
```

## Continuous integration

A GitHub Actions step on a Linux runner (download the release archive for your platform first, and keep original DATA files outside the repository):

```yaml
- name: Check patch
  run: |
    chmod +x ./G3MTool
    ./G3MTool patch validate patches/change.g3mpatch --json
    ./G3MTool patch apply "$ORIGINAL_DATA" patches/change.g3mpatch out/patched.win
    ./G3MTool diff "$ORIGINAL_DATA" out/patched.win reports --json
- uses: actions/upload-artifact@v4
  with:
    name: diff-report
    path: reports/
```

`patch validate` returning `0` does not prove that the patch applies to a given original; the apply step does. `diff` returns `0` whether or not differences exist, so read `differences` from its JSON when a build should fail on changes.

## Conflicts and batches

A merge that finds conflicts still exits with `0`. Read `conflicts` in the JSON result or the report. A report named `<output>.merge_log.md` is written automatically when conflicts exist; `--report <path>` chooses the location. See [patch merge](commands/patch.md#merge).

`patch batch` runs jobs in order and stops at the first failed item unless `--continue-on-error` is set. With it, later jobs still run, and the exit code is `1` when any item failed. Read `items[].success` and `items[].error` for each job. Jobs that repeat an identical `.g3mpatch` input are copied, not recomputed (`deduplicated`).

## Cancel and timeouts

Ctrl+C returns `130` and leaves existing outputs unchanged. `execute` accepts `--timeout <seconds>` for scripts (exit code `1`, message `Script timed out after <hh:mm:ss>.`). xdelta calls made by G3MTool stop after 5 minutes. A program started with `execute` has no timeout from G3MTool; use the timeout of your CI system.

## Logs

`--log <file>` writes a timestamped log (overwritten each run), also with `--json`. `--log default` writes `logs/<command>_<yyyyMMdd_HHmmss>.log` next to the executable, which can be read-only in some installs; pass an explicit path in CI.
