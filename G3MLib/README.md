# G3MLib

G3MLib is a .NET library for reading and editing GameMaker DATA, compiling and decompiling GML, exporting resources, creating and applying patches, merging changes, and generating difference reports.

The package version is **1.1.0** and its target framework is **.NET 10**. It has no application window or game launcher. An application using the library supplies its own interface, paths, progress display, and script interaction.

| Task | Guide |
| --- | --- |
| Add the package and read a file | [Getting started](getting-started.md) |
| Edit the DATA model and save it | [DATA files](data-files.md) |
| Find usages, unused assets, and rename resources | [References and renaming](references.md) |
| Create, validate, or apply a patch | [Patch API](patches.md) |
| Combine patches or compare resources | [Merge and diff](merge-and-diff.md) |
| Export/import resource files | [Resource API](resources.md) |
| Decompile or compile game code | [GML editing](gml.md) |
| Export a game as a project folder, rebuild it, or run a scripted project | [Projects and scripts](projects.md) |
| Connect logs, progress, cancellation, cache, and xdelta | [Host integration](hosting.md) |
| Check common limitations | [FAQ](faq.md) |

Source and license notices are available in [the repository](https://github.com/y114git/G3MLib). G3MLib is GPL-3.0-only and includes code with GPL-3.0 and MPL-2.0 notices. Review the package's license files when distributing an application that includes it.
