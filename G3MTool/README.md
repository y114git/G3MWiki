# G3MTool

G3MTool creates, applies, combines, and inspects GameMaker resource patches. It also runs binary xdelta operations and C# scripts. It has two separate applications: a command-line application (CLI) and a graphical application (GUI).

The command-line application is version **1.3.5**. The graphical application is version **1.0.5**. Both use G3MLib 1.1.5 for resource operations and are distributed under GPL-3.0.

Release packages target Windows, Linux, and macOS on x64 and ARM64. Choose the matching operating system and processor from [GitHub releases](https://github.com/y114git/G3MTool/releases). Each package holds one self-contained executable (it includes the .NET runtime) and the license notices.

| Application | Executable on Windows | Executable on Linux and macOS |
| --- | --- | --- |
| CLI | `G3MTool.exe` | `G3MTool` |
| GUI | `G3MToolGUI.exe` | `G3MToolGUI` |

Supported DATA file extensions are `.win`, `.ios`, `.droid`, and `.unx`.

| Task | Guide |
| --- | --- |
| Create and test a first patch | [Getting started](getting-started.md) |
| Use the graphical application | [GUI guide](gui.md) |
| Run commands from scripts and CI | [Automation](automation.md) |
| Look up an option, exit code, or message | [Command reference](commands/README.md) |
| Process many inputs | [Batch operations](commands/patch.md#batch-operations) |
| Write a C# patch script | [CSX scripting](csx-scripting.md) |
| Inspect patch contents | [Patch format](patch-format.md) |
| Diagnose a failed operation | [FAQ](faq.md) |

G3MTool produces files and reports. It does not manage G3M profiles, launch a game with temporary backups, or decide whether two gameplay changes work together.
