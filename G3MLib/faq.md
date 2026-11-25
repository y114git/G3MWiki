# FAQ

## Is it safe to read and edit one model from several threads?

Not without a lock. Take `GameMakerData.Gate` (`BeginRead` for background jobs, `BeginWrite` for edits) or keep a `ModelLock` next to the model. The library takes the gate itself for scripts, imports, project builds, data file writes and exports. See [Cancellation and threading](cancellation-and-threading.md).

## Why does my save wait or throw `TimeoutException`?

A script or an import holds the gate as a writer. Pass a timeout or a token to `BeginRead` or `BeginWrite` so a UI thread does not block. Do not save or import from a callback that runs on the script's thread while the script waits, such as a click handler during a script dialog: that callback shares the script's scope.

## How do I stop a long operation?

Pass a `CancellationToken` and catch `OperationCanceledException`. `ScriptRunner.RunAsync` returns a result with `Cancelled` set instead of throwing. `ScriptRunner.RunAsync` also takes a `timeout`. A run that times out returns the same result with `Cancelled` set. A cancelled path-based apply or write leaves its target as it was. A cancelled write to a stream can leave partial output. A cancelled resource export removes the files it newly created, but cannot restore a file it overwrote. An in-memory patch apply or import can leave the model partly changed, so discard it. See [Cancellation and threading](cancellation-and-threading.md#cancel-an-operation).

## Does G3MLib launch games or manage mods?

No. It supplies DATA and modding APIs. The host manages games, profiles, downloads, backups around play sessions, and its own user interface. See [Host integration](hosting.md#embed-the-library-in-an-application).

## Can every GameMaker game be edited?

Support depends on the file's format and available resources. A readable metadata header does not guarantee that every resource or compiled-code variant supports editing. Check `LoadWarnings` and `MayLoseDataOnSave` after reading, and test the written result with its game. See [Data integrity](data-integrity.md).

## Are `.csx` inputs accepted by every patch method?

No. G3MTool provides a script execution layer around the library. A direct library host runs its scripts with `ScriptRunner` and supplies the resulting DATA to the patch workflow. See [Scripts and ScriptGlobals](script-globals.md).

## Why does a merge succeed with conflicts?

The service can resolve an overlap by priority and still report it. Inspect TotalConflicts and the report; a merge that succeeds can still lose part of what a mod does.

## Why are dialogs unavailable in scripts?

The default `ScriptGlobals` has no application UI. `ScriptQuestion` returns false, and the dialog, prompt and `ScriptOpenURL` members throw a `ScriptException`. The short `ScriptInputDialog(title, label, defaultInput, allowMultiline, showDialog)` returns the default when `showDialog` is false. Override the members in a `ScriptGlobals` subclass. A host that overrides the prompts returns null when the user cancels one (`PromptLoadFile`, `PromptSaveFile`, `PromptChooseDirectory`, `SimpleTextInput` and `ScriptInputDialog`). Do not rely on G3MTool CLI's prompt return values for a different host.

## Do I need xdelta for ordinary resource patching?

Not for every operation. Binary patch creation/application and embedded fallback paths need a configured executable. The host is responsible for supplying it.

## Why does `GameMakerIO.WriteAsync` fail on a new folder?

The path overload writes a temporary file next to the target and does not create the folder. Create the folder first, or use `PatchInputService.WriteDataFile`, which creates it.

## Why is a resource export folder named `a_b__<hash>`?

A resource name with a character that cannot be part of a file name (such as `a:b`) gets a safe folder name with a 12 character hash suffix, so two names cannot share a folder. The suffix also applies to a name that ends in a dot or space, a reserved device name such as `CON`, a name longer than 96 characters, and a name already shaped like `a_b__` plus 12 hex digits. Import reads the real name from the JSON `name`. Older exports without the suffix still import.

## Can I delete a resource that other resources use?

Clear its uses first with `ReferenceIndex.ClearUsages`. Uses inside code are returned in `Skipped`: change that code first. See [References and renaming](references.md#clear-usages-before-a-delete).

## Can I copy a resource?

Yes: `ResourceCloneService.Clone` copies objects, scripts, sprites, rooms and most other resources inside one data file. It throws `NotSupportedException` for code entries, functions, variables, code locals, audio groups, texture groups, filter effects, and embedded audio, textures and images. The new name must be valid and not in use, or `Clone` throws `RenameException`. See [References and renaming](references.md#clone-a-resource).

## Can I use a macro declared in another code entry?

Yes. Give it to the compile through `CompileGroup.Macros` or `AddMacros`, `CodeImportGroup.Macros`, or `CompileContext.ExternalMacros`. See [GML editing](gml.md#macros).

## Do sprite page layouts depend on the version?

Sprite import packs new sprites in file name order with a 2 pixel border, so layouts can differ from those an older G3MLib produced. Unknown chunks and resource ids outside the table are kept as they were.

## Can I distribute a DATA file or exported sprites?

The library's license does not grant rights to the game's assets. Follow the game's and content owners' redistribution terms. A patch can avoid shipping complete originals, but its payload still needs review.
