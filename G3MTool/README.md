# G3MTool

G3MTool creates, applies, combines, and inspects GameMaker resource patches. It also runs binary xdelta operations and C# scripts.

The command-line application is version **1.3.0**. The separate graphical application is version **1.0.0**. Both use G3MLib for resource operations and are distributed under GPL-3.0.

Release packages target Windows, Linux, and macOS on x64 and ARM64. Choose the matching operating system and processor from [GitHub releases](https://github.com/y114git/G3MTool/releases). Self-contained packages include their .NET runtime.

| Task | Guide |
| --- | --- |
| Create and test a first patch | [Getting started](getting-started.md) |
| Use the graphical application | [GUI guide](gui.md) |
| Run commands from scripts | [Automation examples](automation.md) |
| Inspect CLI options | [Command reference](commands/README.md) |
| Process many inputs | [Batch operations](commands/patch.md#batch-operations) |
| Write a C# patch script | [CSX scripting](csx-scripting.md) |
| Inspect patch contents | [Patch format](patch-format.md) |
| Diagnose a failed operation | [FAQ](faq.md) |

G3MTool produces files and reports. It does not manage G3M profiles, launch a game with temporary backups, or decide whether two gameplay changes work together.
