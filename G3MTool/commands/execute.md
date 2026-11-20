# execute

```text
G3MTool execute <target> [options] [-- arguments...]
```

The target decides the mode: a name ending in `.csx` runs a script, the literal `xdelta` (any letter case) runs xdelta, and anything else starts an external program. Put forwarded arguments after `--` so their flags are not read as G3MTool options.

| Option | Short | Value | Applies to | Meaning |
| --- | --- | --- | --- | --- |
| `--data` | `-d` | DATA file | scripts | Load this file as `Data` |
| `--output` | `-o` | path | scripts | Where results go (table below) |
| `--input` | `-i` | folder | scripts | Becomes the first script argument and `InputDir` |
| `--timeout` | none | whole seconds, greater than 0 | scripts | Stop the script after this time |

Using any of these four options with a program or `xdelta` fails with `--data applies to .csx scripts only.` (the option name changes) and exit code `1`. `--timeout 0` fails with `--timeout must be a positive number of seconds.`

## C# script

```text
G3MTool execute resize-room.csx --data original.win --output test.win
G3MTool execute ExportSprites.csx --data original.win --output exports/
G3MTool execute ImportSprites.csx --data original.win --input exports/Sprites --output imported.win
```

`--output` is read as follows:

| `--output` value | Script exports (`OutputDir`) | DATA file |
| --- | --- | --- |
| An existing folder, a path ending in `/` or `\`, or a path without an extension (and not an existing file) | That folder, created when missing | Not written |
| Any other path | The folder of that path | Written to that path after the script succeeds |
| Omitted | The current folder | Not written. With `--data`, stderr shows `Note: the data file was not saved. Pass --output <file> to save it.` |

The script console output is hidden unless `--verbose` is set. `--verbose` also prints every library log line, which is long for a large DATA file. Questions are answered yes, text inputs return their defaults, and file or folder prompts return null. See [CSX scripting](../csx-scripting.md) for globals, nested scripts, and the timeout.

With `--json` a success prints `{"success":true,"command":"execute","script":<full path>,"data":<path or null>,"saved":<path or null>,"exportDirectory":<folder>,"warnings":[]}`; a failure prints `{"success":false,"command":"execute","error":...}`.

| Message | Cause |
| --- | --- |
| `Script not found: <path>` | Script path does not exist |
| `Data file not found: <path>` | `--data` path does not exist |
| `Compilation error:` followed by compiler lines | The script does not compile |
| `Script timed out after <hh:mm:ss>.` | `--timeout` passed |
| `Script execution failed: <reason>` | The script threw an exception or saving failed |
| the text given to `ScriptError(...)` | The script stopped itself |
| `Cancelled.` (stderr, exit code `130`) | Ctrl+C |

All failures other than cancellation return exit code `1`.

## External program

```text
G3MTool execute ./asset-checker -- --input assets --strict
```

The program starts directly, without a shell and without a console window. Its stdout and stderr are copied to yours as they arrive, and its exit code becomes the exit code of G3MTool. Pipes and redirects belong to the calling shell. If it cannot start: `Error executing <program>: <reason>`, exit code `1`. Ctrl+C kills the program and its child processes.

With `--json`, program output is not echoed. One object is printed after the program ends: `{"success":<exit code is 0>,"command":"execute program","program":...,"exitCode":...,"output":...,"error":...,"warnings":[]}`.

## Raw xdelta

```text
G3MTool execute xdelta --xdelta-path ./xdelta -- -d -s original.win change.xdelta output.win
```

Runs the bundled xdelta (or `--xdelta-path`, which goes before the `--`) with exactly the arguments after `--`. Standard output and error are printed together after xdelta ends. A nonzero xdelta exit code gives `xdelta exited with code <n>` and G3MTool exit code `1`. The 5 minute limit and the other messages are the same as in [xpatch](xpatch.md#errors). With `--json`: `{"success":true,"command":"execute xdelta","output":...,"warnings":[]}`; on failure the xdelta text is inside the `error` field.
