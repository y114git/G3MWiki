# patch

`patch` works with resource patches and compatible inputs: GameMaker DATA, `.g3mpatch`, patch ZIPs with `g3mpatch.json`, `.xdelta`, `.vcdiff`, and `.csx`. A script is executed while deriving its modified DATA; run trusted scripts only.

## Create

```text
G3MTool patch create <original> <modified-or-input> [output]
    [--xdelta] [--xdelta-fallback] [--cache <directory>]
```

The original supplies the starting DATA. A modified DATA input is compared directly; another supported patch or script input is first applied against that original.

| Option | Effect |
| --- | --- |
| `--xdelta` | Create a binary xdelta patch instead of `.g3mpatch` |
| `--xdelta-fallback` | Embed an exact binary fallback in a resource patch |
| `--cache <directory>` | Read and write reusable analysis data |

`--xdelta` and `--xdelta-fallback` cannot be combined. Without an output argument, the result is `patch_<timestamp>.g3mpatch` or `.xdelta` next to the executable.

```text
G3MTool patch create original.win texture-edit.win textures.g3mpatch --cache cache
G3MTool patch create original.win existing.xdelta converted.g3mpatch
```

## Apply

```text
G3MTool patch apply <original> <input> [output]
    [--xdelta-fallback] [--cache <directory>]
```

The result is a DATA file. Direct xdelta and CSX inputs are applied to the original; other supported inputs follow resource-patch application.

`--xdelta-fallback` tries an embedded binary copy first and falls back to resource application if that attempt fails. Resource application can also use an available binary fallback after its own failure. Neither route makes an arbitrary original compatible with the binary patch.

The default output is next to the executable with the original's basename. Specify a separate output explicitly if that default could overlap an input.

```text
G3MTool patch apply original.win textures.g3mpatch patched.win
```

## Validate

```text
G3MTool patch validate <patch> [--data <original>] [--cache <directory>]
```

Validation checks the patch's structure and contents. `--data`, also `-d`, adds original-file compatibility checks. A valid archive without an original-file check does not establish that it can be applied to your game release.

## Merge

```text
G3MTool patch merge <original> <low-priority-input> <higher-priority-input>...
    [--out <patch>] [--apply <data>] [--code] [--properties]
    [--sequential] [--report <markdown>] [--cache <directory>]
```

Supply at least two inputs, **lowest priority first**. Each describes changes against the same original. A later input wins an overlapping change that is not combined. For an addon targeting an already patched game, separate application steps are required instead.

| Option | Effect |
| --- | --- |
| `--out`, `-o` | Save the merged resource patch |
| `--apply`, `-a` | Apply the merged result and write DATA to this path |
| `--code` | Attempt three-way merging of GML code changes |
| `--properties` | Merge supported JSON property changes |
| `--sequential` | Use lower-memory processing; incompatible with `--code` and `--properties` |
| `--report`, `-r` | Write a Markdown merge report |
| `--cache` | Reuse analysis files in the supplied directory |

`--sequential` controls memory use; it does not make each mod target the preceding mod's patched DATA.

With neither output option, the command saves `merged_<timestamp>.g3mpatch` beside the original. With only Apply, it writes DATA without retaining a merged patch. With both options, it produces both outputs.

```text
G3MTool patch merge original.win audio.g3mpatch interface.g3mpatch --out combined.g3mpatch --apply combined.win --report merge.md
```

## Batch operations

Each job uses the same original independently. It does not turn a list into a sequential mod stack.

```text
G3MTool patch batch create <original> <inputs...> --out-dir <directory>
G3MTool patch batch apply <original> <inputs...> --out-dir <directory>
G3MTool patch batch merge <original> "low,high" "other-low,other-high"
    [--apply <data-directory>] [--out <patch-directory>]
```

Create accepts `--xdelta` or `--xdelta-fallback`; Apply accepts `--xdelta-fallback`. Both require `--out-dir`. Merge accepts `--code`, `--properties`, and the boolean `--report`, which writes a report beside each merged patch. Batch merge does not accept the single-merge `--sequential` flag.

All three accept `--cache <directory>` and `--continue-on-error`. Without Continue on error, a failed job stops the remaining queue. With it, remaining jobs run, but failed jobs still make the overall operation fail.

For batch merge, each quoted argument is one comma-separated set in low-to-high priority order. Paths in a merge set cannot contain commas. The DATA output directory defaults to the current working directory. `--out` optionally retains merged patch packages.

```text
G3MTool patch batch create original.win edit-a.win edit-b.win --out-dir patches
G3MTool patch batch apply original.win a.g3mpatch b.xdelta --out-dir builds --continue-on-error
G3MTool patch batch merge original.win "audio.g3mpatch,ui.g3mpatch" "audio.g3mpatch,colours.g3mpatch" --apply builds --out packages --report
```

Batch outputs use generated names based on their jobs. Check the reported output paths rather than assuming a job overwrites its source. Repeated identical jobs may reuse a generated result.
