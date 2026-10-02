# Getting started

Create a .NET 10 application and reference the package:

```text
dotnet new console --framework net10.0 --name DataInspector
cd DataInspector
dotnet add package G3MLib --version 1.0.1
```

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

Use `GameMakerData` for direct resource edits. Use `PatchService` when the result should be a distributable changeset. Use `MergeService` for combining patches against one original. Use `ProjectContext` for a project of exported assets and configured installation actions.

These outputs have different formats. A `.g3mpatch` manifest is not `mod_config.json`, and a resource project is not a G3M profile.

Examples in this section use separate outputs. Keep the original and companion resources until the result has been read back and playtested.
