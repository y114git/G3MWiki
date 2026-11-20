# Version

```text
G3MTool --version
G3MTool -V
```

Either prints the CLI version (currently `1.3.5`) and exits with code `0`. There is no `version` subcommand. With other arguments present, `-V` is rewritten to `--version` unless it follows a `--` separator.

The GUI shows its version at the top left of the window as `G3MTool (GUI) - 1.0.5`. The CLI and GUI numbers belong to separate applications. Include the number in a bug report.

Patches record the tool in their manifest (`tool` field, for example `G3MTool` `1.3.5`) and the G3MLib version (`library` field). `G3MTool info <patch>` prints the tool.
