# execute

```text
G3MTool execute <target> [--data <file>] [--output <file>]
    [--input <directory>] -- [arguments...]
```

Target can be a `.csx` script, an external program, or the literal `xdelta`. Put forwarded arguments after `--` so their flags are not parsed as G3MTool options.

## C# script

| Option | Alias | Effect |
| --- | --- | --- |
| `--data` | `-d` | Load GameMaker DATA for the script |
| `--output` | `-o` | Write resulting DATA to this file; also supplies its containing output directory |
| `--input` | `-i` | Supplies the first script input argument and InputDir |

```text
G3MTool execute resize-room.csx --data original.win --output test.win
```

Provide Output explicitly with Data. Although the CLI can fall back to the original basename beside its executable, that path may be unsuitable or overlap an input. A script without Data can still perform its own file operations; DATA-dependent code must call `EnsureDataLoaded()`.

See [CSX scripting](../csx-scripting.md) for globals, nested scripts, and CLI interaction limits.

## External program

```text
G3MTool execute ./asset-checker -- --input assets --strict
```

The target is started directly with the supplied arguments, without shell interpretation. Its stdout and stderr are forwarded, and its exit status is returned. Shell pipelines and redirects belong in the calling shell, not the forwarded argument list.

## Raw xdelta

```text
G3MTool execute xdelta --xdelta-path ./xdelta -- -d -s original.bin change.xdelta output.bin
```

The literal xdelta target resolves the configured or bundled xdelta executable. Execute does not guarantee JSON output from a script or external process.
