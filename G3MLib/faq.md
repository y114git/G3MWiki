# FAQ

## Is it safe to read and edit one model from several threads?

Not without a lock. `GameMakerData` does not lock itself. Keep a `ModelLock` next to the model, hold `Read()` in background jobs, and hold `Write()` for edits. See [DATA files](data-files.md#share-one-model-between-threads).

## Do sprite page layouts match 1.0.1?

No. Sprite import in 1.1.0 packs new sprites in file name order with a 2 pixel border, so the pages differ from the ones 1.0.1 produced. Unknown chunks and resource ids outside the table are kept as they were.

## Does G3MLib launch games or manage mods?

No. It supplies DATA and modding APIs. The host manages games, profiles, downloads, backups around play sessions, and its own user interface.

## Can every GameMaker game be edited?

Support depends on the file's format and available resources. A readable metadata header does not guarantee that every resource or compiled-code variant supports editing. Keep warnings and test the written result with its game.

## Are `.csx` inputs accepted by every patch method?

No. G3MTool provides a script execution layer around the library. A direct library host must execute its scripts through the chosen scripting environment and supply the resulting DATA to the patch workflow.

## Why does a merge succeed with conflicts?

The service can resolve an overlap by priority and still report it. Inspect TotalConflicts and the report; a merge that succeeds can still lose part of what a mod does.

## Why are dialogs unavailable in ProjectContext scripts?

The default library context has no application UI. Connect text callbacks or provide an interactive scripting host. Do not rely on G3MTool CLI's prompt return values for a different host.

## Do I need xdelta for ordinary resource patching?

Not for every operation. Binary patch creation/application and embedded fallback paths need a configured executable. The host is responsible for supplying it.

## Can I distribute a DATA file or exported sprites?

The library's license does not grant rights to the game's assets. Follow the game's and content owners' redistribution terms. A patch can avoid shipping complete originals, but its payload still needs review.
