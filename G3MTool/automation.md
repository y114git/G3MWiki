# Use G3MTool in scripts

Run the CLI with explicit input and output paths. Check its exit code before consuming a result. Commands accept arguments directly; the interactive prompt is not needed for automation.

## Read a JSON result in PowerShell

This example creates a resource patch and reads the output path from the result:

```powershell
$resultText = & .\G3MTool.exe patch create original.win modified.win output.g3mpatch --json
if ($LASTEXITCODE -ne 0) {
	throw "Patch creation failed. See the command's error output."
}
$result = ($resultText -join "`n") | ConvertFrom-Json
if (-not $result.success) {
	throw "G3MTool reported an unsuccessful result."
}
$result.output
```

Use the executable name and path from your release package. Keep stderr visible or capture it separately; do not merge it into the JSON text.

## Apply a patch from Python

An argument list preserves paths containing spaces without shell quoting:

```python
import json
import subprocess

result = subprocess.run(
	["./G3MTool", "patch", "apply", "original.win",
	 "textures.g3mpatch", "output/patched.win", "--json"],
	check=True,
	capture_output=True,
	text=True,
)
payload = json.loads(result.stdout)
if not payload["success"]:
	raise RuntimeError(payload)
print(payload["output"])
```

On Windows, use `G3MTool.exe`. `check=True` raises `CalledProcessError` on a nonzero exit code; its `stderr` contains the captured error output.

## Command-specific output

`--json` does not impose one schema on every command:

| Command | Result to inspect |
| --- | --- |
| `patch create` | `success`, `output`, `statistics`, and `warnings`. |
| `patch apply` | `success`, `output`, `details`, and `warnings`. |
| `patch validate` | `success`, compatibility data, and `warnings`. Use `--data` to check an original. |
| `patch merge` | `success`, `output`, `applied`, `conflicts`, `autoMerged`, and `warnings`. |
| `patch batch` | `success`, `total`, `completed`, `failed`, `deduplicated`, and per-job `items`. |
| `diff` | `success`, the report's `output`, and counts such as `differences`, `changed`, `new`, and `deleted`. |
| `info` | DATA metadata or the patch manifest itself. These objects do not have the patch-command `success` wrapper. |

DATA metadata from `info` uses keys such as `File` and `Resources`; patch manifests use keys such as `original`, `modified`, and `resources`. Treat the selected command and input type as part of your parser's contract.

`xpatch` and `execute` do not provide these JSON results. An executed program can write its own stdout.

## Failures, conflicts, and batches

A successful command returns exit code `0`; argument and tool failures return a nonzero code. `execute` returns the external program's exit status. Error output is not guaranteed to be a JSON object, so inspect the exit code before parsing stdout.

A successful merge can contain conflicts. Decide whether to accept them by checking `conflicts` and the [merge report](commands/patch.md#merge), rather than treating exit code `0` as proof that the mods agree.

`--continue-on-error` lets a batch run its remaining jobs after a failure. It does not turn the failed batch into a successful one. Keep successful outputs if needed, but report failed jobs to the caller.
