# Command reference

| Command | Purpose |
| --- | --- |
| `patch create` | Create a resource patch or convert a supported input to a patch |
| `patch apply` | Write patched DATA |
| `patch validate` | Check a resource patch, optionally against original DATA |
| `patch merge` | Combine changes from at least two inputs |
| `patch batch create/apply/merge` | Run independent jobs against one original |
| `xpatch create/apply` | Create or apply a binary patch |
| `execute` | Run a CSX script, external program, or raw xdelta arguments |
| `info` | Inspect a DATA file or patch |
| `diff` | Write a Markdown difference report |
| `--version` | Print the CLI version |

Append `--help` to the command or subcommand for its arguments. Paths with spaces require shell quoting. Arguments shown in angle brackets are values you supply; square brackets indicate optional arguments.

## Global options

| Option | Effect |
| --- | --- |
| `--help`, `-h`, `-?` | Print usage |
| `--version`, `-V` | Print version at the root command |
| `--verbose`, `-v` | Request detailed log output |
| `--log <path>`, `-l <path>` | Save a log; use `--log default` for a timestamped file under the executable's `logs/` folder |
| `--json` | Machine-readable output for supported patch, info, and diff commands |
| `--xdelta-path <path>` | Use the specified xdelta executable instead of the bundled one |

`xpatch` and `execute` do not provide the same JSON result contract. External programs can produce their own output. Check command support before parsing stdout as JSON.

Success returns exit code 0. Tool or argument failures return a nonzero code. An external program's exit status is returned by Execute. A successful merge can still report conflicts; inspect the requested merge report.

For shell and Python examples, see [Use G3MTool in scripts](../automation.md).

## Interactive prompt

Starting the CLI without arguments opens its prompt. Type commands without the executable prefix. `help` prints usage, `clear` or `cls` clears the terminal, and `exit` or `quit` closes the prompt.

The prompt supports double-quoted paths. It is not a shell: do not expect shell variables, pipelines, or redirection inside it. Use the system terminal for scripted workflows.
