# FAQ

## Which original file should I use?

Use the game release and platform for which the patch was made. xdelta usually needs exact original bytes. A resource patch can also depend on the original resource state and bytecode format.

## Does merge apply patches one after another?

No. Each input describes changes against the same original. Priority resolves overlapping changes. For a patch built against another mod's output, apply that base first and use the resulting DATA as the addon's original.

## Does sequential merge mean sequential mod steps?

No. It is a memory-use option. It does not change the expected original for each input, and it does not support code/property merging.

## Why does a script show no file picker in the CLI?

CLI prompt helpers return their documented defaults or null. Supply inputs explicitly, adapt the script, or use the GUI's interactive script operation.

## Is a successful output ready to play?

It still needs the matching game runner and companion files. Keep external audio groups and resources together, inspect tool warnings, and test the result in a separate installation.

## Can I clear the cache?

Yes, when no operation is using it. It stores reusable analysis, not your original DATA or mod package. The next operation rebuilds the needed analysis.

## Why is the result different byte-for-byte?

Resource patching reconstructs resources and can serialize them differently. Compare the resource report and test the game. Use xdelta when exact file bytes are required.
