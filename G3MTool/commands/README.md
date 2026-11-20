# Command reference

This page lists every command, the options shared by all commands, exit codes, and the interactive prompt. Each command has its own page with arguments, options, and messages.

| Command | Purpose |
| --- | --- |
| [`patch create`](patch.md#create) | Create a resource patch (or an xdelta patch) from a supported input |
| [`patch apply`](patch.md#apply) | Write patched DATA |
| [`patch validate`](patch.md#validate) | Check a resource patch, optionally against original DATA |
| [`patch merge`](patch.md#merge) | Combine changes from two to 100 inputs |
| [`patch batch create`, `apply`, `merge`](patch.md#batch-operations) | Run independent jobs against one original |
| [`xpatch create`, `apply`](xpatch.md) | Create or apply a binary xdelta patch |
| [`execute`](execute.md) | Run a CSX script, an external program, or raw xdelta arguments |
| [`info`](info.md) | Inspect a DATA file or patch |
| [`diff`](diff.md) | Write a Markdown difference report |
| [`--version`](version.md) | Print the CLI version |

Add `--help` (`-h`, `-?`) to a command or subcommand to print its usage. Paths with spaces need shell quoting. In usage lines, `<value>` is required and `[value]` is optional.

## Global options

These options are accepted by every command, before or after the command name.

| Option | Short | Value | Effect |
| --- | --- | --- | --- |
| `--verbose` | `-v` | none | Print detailed output. Also prints script console output and library messages |
| `--log <path>` | `-l` | file path | Write a log file. The value `default` (any letter case) writes `logs/<command>_<yyyyMMdd_HHmmss>.log` next to the executable, where `<command>` is the innermost command name (for example `create`) |
| `--json` | none | none | Print one JSON object instead of text. See [Automation](../automation.md#json-results) |
| `--xdelta-path <path>` | none | file path | Use this xdelta executable instead of the bundled one |
| `--help` | `-h`, `-?` | none | Print usage |
| `--version` | `-V` | none | Print the version (root command only) |

`info` also defines its own `-v`/`--verbose` (full per-resource listing) with the same spelling.

A log file is overwritten on each run. Every line starts with `[yyyy-MM-dd HH:mm:ss.fff]`. If the log file cannot be opened, the CLI prints `Warning: Could not open log file '<path>': <reason>` and continues without a log.

With `--json`, informational output, progress lines, and warnings are not printed. Errors of the `patch`, `xpatch`, `execute`, and `diff` commands are reported as JSON on stdout. `info` writes its errors as text to stderr.

There are no environment variables and no configuration files. Defaults depend only on the arguments and on the folder of the executable.

## Exit codes

| Code | Meaning |
| --- | --- |
| `0` | Success. A merge with conflicts also returns `0`; check the conflict count or the report |
| `1` | Command failure, invalid arguments (missing argument, unknown option, `--timeout` not positive), missing required option such as `--out-dir`, or an error no command handles (printed as `Error: <message>`, as JSON under `--json`) |
| `130` | Cancelled with Ctrl+C. The CLI prints `Cancelled.` to stderr |
| any other | `execute` with an external program returns that program's exit code |

`execute` with the target `xdelta` returns `1` when xdelta fails.

## Cancellation

Press Ctrl+C once to cancel. The running operation stops at its next check, temporary files are removed, and the exit code is `130`. For operations that stage their output and then replace the target (such as `patch` and `xpatch`), an output that existed before the run is not replaced. This does not apply to a program started by `execute`: stopping it does not undo what it has already written. A second Ctrl+C while the first is being handled uses the system default and ends the process at once. A killed `execute` program is stopped together with its child processes.

## Default output locations

When an output argument is omitted, these defaults apply. They are next to the executable, not next to the input.

| Command | Default |
| --- | --- |
| `patch create` | `patch_<yyyyMMdd_HHmmss>.g3mpatch` (`.xdelta` with `--xdelta`) |
| `patch apply` | A file with the DATA file's name |
| `patch merge` | `merged_<yyyyMMdd_HHmmss>.g3mpatch` in the folder of the original DATA file (only when neither `--out` nor `--apply` is given) |
| `xpatch create` | The modified file's name with `.xdelta` |
| `xpatch apply` | `<original-name>_patched<ext>` where `<ext>` is the DATA extension, or `.win` |
| `diff` | `diff/diff_<yyyyMMdd_HHmmss>.md` |

Always pass explicit outputs in scripts.

## Interactive prompt

Start the CLI without arguments. It prints `G3MTool (CLI) - 1.3.5 - by Y114` and `Type 'help' for available commands or 'exit' to quit`, then the prompt `(G3MTool) `.

| Input | Effect |
| --- | --- |
| a command line | Runs it, without the executable name, for example `info original.win` |
| `help` | Prints the root usage |
| `clear`, `cls` | Clears the terminal |
| `exit`, `quit` | Leaves the prompt (case-insensitive) |
| end of input (Ctrl+Z then Enter on Windows, Ctrl+D on Linux and macOS) | Leaves the prompt, exit code `0` |

Double quotes group a path with spaces, and `""` passes an empty argument. The prompt is not a shell: it has no variables, pipelines, or redirection, and backslash is not an escape character. Each command starts with default options, so `--verbose` or `--json` must be repeated on every line. Ctrl+C cancels the running command and returns to the prompt.

For scripts, see [Automation](../automation.md).
