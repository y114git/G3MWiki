# G3MLib

G3MLib is a .NET library for reading and editing GameMaker DATA, compiling and decompiling GML, exporting resources, creating and applying patches, merging changes, and generating difference reports.

The package id is `G3MLib`, the version is **1.1.7**, and its target framework is **.NET 10**. It has no application window or game launcher. An application using the library supplies its own interface, paths, progress display, and script interaction. See [Limits and defaults](limits-and-defaults.md) for the dependencies and [Host integration](hosting.md#embed-the-library-in-an-application) for the steps an application takes.

| Task | Guide |
| --- | --- |
| Add the package and read a file | [Getting started](getting-started.md) |
| Edit the DATA model and save it | [DATA files](data-files.md) |
| Find usages, unused assets, rename or copy resources | [References and renaming](references.md) |
| Create, validate, or apply a patch | [Patch API](patches.md) |
| Combine patches or compare resources | [Merge and diff](merge-and-diff.md) |
| Export/import resource files | [Resource API](resources.md) |
| Decompile or compile game code | [GML editing](gml.md) |
| Export a game as a project folder, rebuild it, or run a scripted project | [Projects and scripts](projects.md) |
| Look up the `.g3mproj` file and folder layout | [Project format](project-format.md) |
| Run `.csx` scripts and look up what scripts can call | [Scripts and ScriptGlobals](script-globals.md) |
| Check a data file for problems, compare chunks and hashes | [Data integrity and hashing](data-integrity.md) |
| Cancel an operation, share one model between threads | [Cancellation and threading](cancellation-and-threading.md) |
| Connect logs, progress, cache, and xdelta | [Host integration](hosting.md) |
| Find the page for a public type | [API index](api-index.md) |
| Look up limits and default values | [Limits and defaults](limits-and-defaults.md) |
| Check common questions | [FAQ](faq.md) |

Source and license notices are available in [the repository](https://github.com/y114git/G3MLib). G3MLib is GPL-3.0-only and includes code with GPL-3.0 and MPL-2.0 notices. Review the package's license files when distributing an application that includes it.

The repository also holds two command line tools. `G3MLib.Benchmarks` measures reads, writes, resources, patches and merges on a data file you supply (`--full` adds the slow cases, `--compare` runs the comparison with another library, `--markdown` prints table rows). `G3MLib.ScriptCompat` compiles every `.csx` script in a folder against the script host and runs those that need no dialog on a copy of a data file. It takes four arguments: the scripts folder, the data file, a work folder and a Markdown report file. See the repository README for the exact commands.
