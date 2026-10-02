# xpatch

`xpatch` creates and applies binary xdelta patches. These operate on file bytes rather than GameMaker resources and can be used for other file types.

```text
G3MTool xpatch create <original> <modified> [patch-output]
G3MTool xpatch apply <original> <patch> [file-output]
```

Create defaults to the modified input's basename with an `.xdelta` extension next to the executable. Apply defaults to `<original-stem>_patched` next to the executable. It preserves a recognized DATA extension; otherwise its default extension is `.win`. For other file types, always supply the output filename you want.

```text
G3MTool xpatch create original.bin modified.bin change.xdelta
G3MTool xpatch apply original.bin change.xdelta result.bin
```

The original must match what the binary patch requires. A different game release or already modified file commonly fails to decode.

Use `--xdelta-path <executable>` to override the bundled binary. For raw xdelta arguments, use [Execute](execute.md). Global logging options apply, but `--json` does not convert xpatch's result to the patch command's JSON contract.
