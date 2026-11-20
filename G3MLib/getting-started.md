# Getting started

Create a .NET 10 application and reference the package:

```text
dotnet new console --framework net10.0 --name DataInspector
cd DataInspector
dotnet add package G3MLib --version 1.1.5
```

The package id is `G3MLib` and it targets `net10.0`. It brings in `Magick.NET-Q8-AnyCPU`, `Microsoft.CodeAnalysis.CSharp.Scripting`, `Microsoft.CodeAnalysis.Scripting.Common` and `SharpZipLib`. See [Limits and defaults](limits-and-defaults.md#package).

## Read a DATA file

Replace the console application's `Program.cs` with:

```csharp
using G3MLib.DataFile;

if (args.Length != 1)
{
	Console.Error.WriteLine("Usage: DataInspector <data-file>");
	return;
}

using var input = File.OpenRead(args[0]);
using var data = GameMakerIO.Read(input,
	warningHandler: (warning, isImportant) => Console.Error.WriteLine(warning));

Console.WriteLine(data.GeneralInfo?.DisplayName?.Content ?? "Unnamed game");
Console.WriteLine($"Rooms: {data.Rooms?.Count ?? 0}");
Console.WriteLine($"Sprites: {data.Sprites?.Count ?? 0}");
```

Run it with an actual GameMaker resource file:

```text
dotnet run -- /path/to/data.win
```

Use the same file you intend to inspect or patch, including its platform and game release. A `.win` extension does not make an arbitrary file a readable DATA file.

## Pick the API level

| Goal | Use |
| --- | --- |
| Edit resources directly | `GameMakerData` ([DATA files](data-files.md)) |
| Produce a distributable changeset | `PatchService` ([Patch API](patches.md)) |
| Combine patches against one original | `MergeService` ([Merge and diff](merge-and-diff.md)) |
| Export a game as a folder of resource files and rebuild a DATA file from it | `G3MProjectService` ([Project format](project-format.md)) |
| Work with a project of exported assets and configured installation actions | `ProjectContext` ([Projects and scripts](projects.md)) |
| Run `.csx` scripts | `ScriptRunner` ([Scripts and ScriptGlobals](script-globals.md)) |
| Check a loaded file for problems | `DataIntegrityService` ([Data integrity](data-integrity.md)) |

Reading and writing also have async forms, `GameMakerIO.ReadAsync` and `WriteAsync`, with progress and a cancellation token. See [DATA files](data-files.md#read-and-write-asynchronously).

These outputs have different formats. A `.g3mpatch` manifest is not `mod_config.json`, and a `.g3mproj` project is not a G3M profile.

Examples in this section use separate outputs. Keep the original and companion resources until the result has been read back and playtested.

## Next steps

- [Host integration](hosting.md#embed-the-library-in-an-application) lists what an application sets up around the library.
- [API index](api-index.md) links every public type to its page.
