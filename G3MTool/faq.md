# FAQ

## Which original file should I use?

The game release and platform the patch was made from. A `.g3mpatch` records the original's MD5 (`original.md5`). `patch validate <patch> --data <file>` prints a `[Warning]` line, `Data file hash doesn't match original - patch may not apply correctly`, when it differs. The line is not printed with `--json`. xdelta needs the exact original bytes.

## Does merge apply patches one after another?

No. Each input describes changes against the same original, and the priority order (lowest first) resolves overlaps. For a patch made against another mod's output, apply that mod first and use the result as the original.

## What does `--sequential` (Low-memory merge) do?

It lowers memory use. It does not change which original each input needs. It fails with `Sequential merge does not support code or property merging.` when combined with `--code` or `--properties`.

## Where does the output go when I leave it out?

Next to the CLI executable, not next to the input. The exception is `patch merge` without `--out` and `--apply`, which writes `merged_<timestamp>.g3mpatch` next to the original DATA file. See [Default output locations](commands/README.md#default-output-locations). Pass explicit outputs.

## `execute` prints `Note: the data file was not saved.`

`execute` saves the DATA only when `--output` is a file path. A folder (existing, ending in `/` or `\`, or without an extension) receives exports only. Use `--output result.win`.

## Why does a script show no file picker in the CLI?

CLI prompt helpers return defaults: questions answer yes, text inputs return their default, file and folder pickers return null. Pass paths as arguments (`--input`, `--`) or use **Run a script** in the [GUI](gui.md#scripts-and-dialogs).

## Why is there no script output?

The CLI hides script console output and `ScriptMessage` text unless `--verbose` is set. Verbose also prints many library lines.

## How do I stop a running operation?

CLI: Ctrl+C. The exit code is `130` and temporary files are removed. For patch and DATA output that G3MTool writes, existing outputs stay as they were. A script still keeps files it already wrote itself, and an `execute` program keeps what it already wrote. GUI: **Cancel**; closing the window cancels first. Do not kill the process while an output is being written.

## A batch stopped after the first error

Add `--continue-on-error` (**Continue after errors** in the GUI). The exit code is still `1` when any item failed.

## `Error: Original data file not found` or another `not found` error in a batch command

A file named on the command line does not exist. The message names it. Batch commands check every path before the first job. With `--json` the same text is in the `error` field.

## `xdelta executable not found` or `No xdelta is bundled for <platform>`

Install xdelta3 and pass `--xdelta-path <path>`. The bundled copy is extracted to a temporary folder when needed; if a security product blocks it, use your own copy.

## `xdelta exited with code 1`

The patch does not fit the file it was applied to (different game release, or an already patched file), or an argument is wrong. The `Error:` line printed before this line is xdelta's own error text.

## Is a successful output ready to play?

Not necessarily. Test it in a separate game installation. Keep external audio groups (`audiogroupN.dat`) beside the DATA file; G3MTool copies missing ones next to a written output.

## Why is the result different byte for byte?

Resource patching rebuilds resources and can serialize them differently. Compare with `diff`, and test the game. For exact bytes use xdelta (`xpatch`) or create the patch with `--xdelta-fallback` and apply it with `--xdelta-fallback`.

## Can I clear the cache?

Yes, when no operation uses it. It holds analysis files (`.g3mcache`), not your DATA or patches. A stale entry is ignored when the source file size or MD5 changed.

## Can the CLI read a configuration file or environment variable?

No. All behavior is set by arguments.

## Does `info` show what a patch changes?

It shows counts per resource type (with `-v`). For details compare the patched and original DATA with [diff](commands/diff.md).
