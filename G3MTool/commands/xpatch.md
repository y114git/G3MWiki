# xpatch

`xpatch` creates and applies binary xdelta patches. They work on file bytes, not GameMaker resources, so they also suit other file types.

```text
G3MTool xpatch create <original> <modified> [output]
G3MTool xpatch apply <original> <patch> [output]
```

| Argument | Default | Meaning |
| --- | --- | --- |
| `original` | required | Original file |
| `modified` / `patch` | required | Modified file (create) or xdelta patch (apply) |
| `output` | create: the modified file's name with `.xdelta`; apply: `<original-name>_patched<ext>`, both next to the executable | Output file. Its folder is created when missing. An output that this run created is deleted again on failure or cancel |

For apply, `<ext>` is the original's extension when it is `.win`, `.ios`, `.droid`, or `.unx`, otherwise `.win`. For other file types, always pass the output name.

```text
G3MTool xpatch create original.bin modified.bin change.xdelta
G3MTool xpatch apply original.bin change.xdelta result.bin
```

Text output starts with `Creating xdelta patch...` or `Applying xdelta patch...`, then lists the original, the modified file or patch, and the output. It ends with `Patch created successfully: <output>` or `Patch applied successfully: <output>`.

With `--json` each command prints one object, success or failure:

```jsonc
{"success":true,"command":"xpatch create","output":"C:\\work\\change.xdelta","details":{"format":"xdelta"},"warnings":[]}
{"success":false,"command":"xpatch apply","error":"Patch file not found: C:\\work\\missing.xdelta"}
```

`--xdelta-path <executable>` replaces the bundled xdelta.

## Errors

Every failure returns exit code `1`.

| Message | Cause |
| --- | --- |
| `Original file not found: <path>` | Missing original (create or apply) |
| `Modified file not found: <path>` | Missing modified file |
| `Patch file not found: <path>` | Missing patch |
| `xdelta exited with code <n>` | xdelta rejected the input. Its error text is printed first, with an `Error:` prefix in text mode, then this line. A patch applied to a different original usually fails this way |
| `xdelta timed out after 300s and was terminated` | xdelta ran longer than 5 minutes |
| `xdelta executable not found: <path>` | Bad `--xdelta-path` |
| `No xdelta is bundled for <platform>. Install xdelta3 and pass --xdelta-path <path>.` | The build has no bundled xdelta for this platform |

The bundled xdelta is extracted to a temporary folder (`g3mtool-xdelta-*`) on first use and removed when the process exits. For raw xdelta arguments, use [execute](execute.md#raw-xdelta).
