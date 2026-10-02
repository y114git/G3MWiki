# Projects and scripts

`G3MLib.DataFile.Project.ProjectContext` works with serialized assets and a project JSON file. It is a library resource-project format, separate from G3M's `mod_config.json` and G3MTool's `.g3mpatch`.

## Create a resource project

```csharp
using G3MLib.DataFile;
using G3MLib.DataFile.Project;

using var input = File.OpenRead("original.win");
using var data = GameMakerIO.Read(input);
var project = new ProjectContext(data, "original.win", "output/edited.win",
	"project/project.json", "Interface test");
project.ScriptMessageHandler = Console.WriteLine;
project.ScriptWarningHandler = message => Console.Error.WriteLine(message);

var room = data.Rooms?.FirstOrDefault()
	?? throw new InvalidDataException("Expected a room to export.");
room.Width += 16;
project.MarkAssetForExport(room);
project.Export(clearMarkedAssets: true);
```

The new project's directory must be empty and its main file must not exist. Use an existing-project factory to open a prepared project instead of calling this constructor over one.

Mark selected IProjectAsset resources with MarkAssetForExport, then call `Export(clearMarkedAssets: true)` to write them. UnmarkAssetForExport, IsAssetMarkedForExport, and EnumerateUnexportedAssets manage the pending set.

Code-source edits use TryGetCodeSource and UpdateCodeSource. RecompileAllCodeSources compiles the project's edited source into its DATA context. Inspect compilation errors before writing game output.

The example writes a project containing the changed room. It does not write `output/edited.win` during export.

## Import a project

CreateWithDataFilePaths supplies explicit load/save DATA paths. CreateWithDirectories supplies load/save directories for the project's configured path choices. Both accept the main project JSON path.

Call Import once on that context. It applies the project's declared resource and file operations; an imported context cannot be imported a second time. The host can supply a current DATA model, backup implementation, and synchronous main-thread callback.

To import the example's assets into an explicitly loaded model:

```csharp
using G3MLib.DataFile;
using G3MLib.DataFile.Project;
using G3MLib.Modding.Patching;

using var input = File.OpenRead("original.win");
using var data = GameMakerIO.Read(input);
var project = ProjectContext.CreateWithDataFilePaths(
	"original.win", "output/project-edited.win", "project/project.json");
project.Import(currentData: data);
input.Dispose();
PatchInputService.WriteDataFile(data, "output/project-edited.win", "original.win");
```

When you supply currentData, Import changes that model and the host saves it. When the context loads its own model, it also writes the configured DATA output during import.

The main JSON uses case-sensitive property names such as Name, Flags, AssetsDataFilePath, ErrorOnWarnings, ExcludeDirectories, SubProjects, Patches, ExternalFiles, FileCopies, FileDeletes, PreImportScripts, PreAssetImportScripts, and PostImportScripts.

| Property | Purpose |
| --- | --- |
| Name | Project label. |
| Flags | Custom flags start with `_`. Other flag names are unsupported and stop import. |
| AssetsDataFilePath | Candidate source/destination DATA paths for asset import |
| VerifyStrings | Required strings used to check the intended input |
| ErrorOnWarnings | Treats DATA read/write warnings as errors; defaults to true |
| ExcludeDirectories | Root directories omitted from project asset scanning |
| SubProjects | Relative paths to additional project JSON files |
| Patches | BPS patch paths and candidate DATA targets. These are not `.g3mpatch` or xdelta inputs. |
| ExternalFiles | Project files/directories copied into the save directory |
| FileCopies, FileDeletes | Declared file actions within the configured game directories |
| PreImportScripts, PreAssetImportScripts, PostImportScripts | Relative scripts at the named import phases |

A path choice can be a string using the same source/destination, or an object with Source and Destination. An array offers alternatives. Prefer explicit path pairs when input and output names differ.

Source and destination choices are relative to LoadDirectory and SaveDirectory respectively. ExternalFiles sources and patch/script paths are relative to the project directory. Paths cannot escape their corresponding directory.

Example metadata fragment:

```json
{
  "Name": "Interface test",
  "AssetsDataFilePath": {
    "Source": "original.win",
    "Destination": "edited.win"
  },
  "ErrorOnWarnings": true,
  "ExcludeDirectories": ["reference"]
}
```

This metadata alone does not contain edited assets. Use exported project assets or existing project examples for the payload's schema; ordinary resource-service exports and serialized project assets are not interchangeable.

### File operations and optional inputs

FileCopies and FileDeletes accept path strings, arrays of alternatives, or an object with Paths and Required. Required defaults to true. For an optional copy:

```json
{
  "FileCopies": [
    {
      "Paths": {"Source": "options.ini", "Destination": "options.ini"},
      "Required": false
    }
  ]
}
```

This is a fragment of the project's main JSON. If `options.ini` is absent from the load directory, that copy is omitted. Required copies stop import when no candidate source exists. FileDeletes also selects by source-file existence, then deletes the corresponding destination.

An ExternalFiles entry uses a direct path pair, for example `{"Source": "extras/logo.png", "Destination": "assets/logo.png"}`. It copies a bundled project file or directory into the save directory.

## Script interaction

ProjectContext provides ScriptMessageHandler and ScriptWarningHandler for text output. Its default script interface does not open URLs or application dialogs. ScriptQuestion returns false; interactive prompts and ScriptOpenURL report that UI is unavailable.

A host needing dialogs must supply an appropriate IScriptInterface scripting environment. Do not assume G3MTool CLI defaults apply to ProjectContext: they are different hosts.

Scripts execute local code. Relative-path validation of project files does not sandbox arbitrary C# or make project imports safe to run from an untrusted source.
