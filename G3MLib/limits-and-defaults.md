# Limits and defaults

Values the library enforces or assumes when you do not set them.

## Package

| Item | Value |
| --- | --- |
| Package id | `G3MLib` |
| Target framework | `net10.0` |
| License | GPL-3.0-only (see [the repository](https://github.com/y114git/G3MLib) for the notices) |
| Dependencies | `Magick.NET-Q8-AnyCPU` 14.17.2, `Microsoft.CodeAnalysis.CSharp.Scripting` 5.12.0, `Microsoft.CodeAnalysis.Scripting.Common` 5.12.0, `SharpZipLib` 1.4.2 |
| Package contents | The library, `LICENSE`, `README.md`, `CHANGELOG.md`, `THIRD_PARTY_NOTICES.md`, license texts, and the `GameSpecificData` content files, which are copied to the output folder of the referencing project |
| Nullable annotations | `annotations` (reference types are not enforced as non-null) |

The library has no application window, no game launcher and no xdelta executable. The host supplies those.

## Patch archives

| Limit | Value |
| --- | --- |
| `PatchFileSystem.MaxPayloadBytes` | 2,147,483,591 bytes (`0x7FFFFFC7`), the most one array can hold. A larger archive or entry is refused. |
| `PatchFileSystem.MaxTotalPayloadBytes` | 4 GiB by default. Settable and static. `long.MaxValue` removes the limit. Counted on declared sizes when loading and on copied bytes when extracting. |
| `PatchFileSystem.MaxManifestBytes` | 64 MiB for `g3mpatch.json`. |
| `PatchFileSystem.MaxArchiveEntries` | 500,000 entries when an archive is extracted to disk. |
| Entries over 64 MiB | Read in chunks, so a false size in the header cannot force the allocation. |
| Entry names | Refused when rooted, holding a drive or stream colon (`:`), or holding a `..` segment. Unsafe manifest paths are skipped when an archive is extracted for a diff. |
| Manifest | A corrupt `g3mpatch.json`, or a JSON `null` one, throws `InvalidDataException` with the patch path in the message. Apply, diff and merge return an error result that names the patch. |

## Defaults

| Setting | Default |
| --- | --- |
| `PatchService.CreatePatchAsync` `includeXdeltaFallback` | `false` |
| `PatchService.ApplyPatchAsync` `allowXdeltaFallback` | `false` |
| `PatchService.ApplyPatchAsync` `verifyModifiedHash` | `true` (checked only when `allowXdeltaFallback` is `true`) |
| `PatchApplyOptions.ValidateBeforeApply` | `true` |
| `XDeltaService` process timeout | 5 minutes |
| `G3MProjectOptions.Mode` | `Full` |
| `G3MProjectOptions.Name` | `Project` |
| `ProjectContext.AllowScripts` | `true` |
| `TexturePageRepacker` border | 2 pixels |
| `RepackOptions.PowerOfTwo` | `true` |
| `CodeImportGroup.AutoCreateAssets` | `true` |
| `CodeImportGroup.AutoCreateGlobalInitScripts` | `true` |
| `CodeImportGroup.ThrowOnNoOpFindReplace` | `false` |
| `CodeImportGroup.GameMakerEventActions` | `false` |
| `ScriptRunner.RunAsync` timeout | none (`null`) |
| `ScriptGlobals.ExePath` | `AppContext.BaseDirectory` |
| `ScriptGlobals.Verbose` | `false` |
| `GameMakerData.ShortCircuit` | `true` |
| `ModdingMetadata.ToolName` | `G3MLib` |
| `ModdingMetadata.ToolVersion` | the library version |
| Hash for `ChunkDigests` | SHA-256 |
| Hash for `FileHash` | MD5 |

## Timing and file behaviour

| Behaviour | Value |
| --- | --- |
| Merge reference scans | Each pattern is stopped after 2 seconds, and a warning is logged. |
| Temporary file name | `<target>.<id>.g3mtmp` next to the target. Each write has its own name. |
| Stale temporary files | Files of the same target older than one day are removed at the first write of that target in the process. |
| Symlinked target | Written through, so the link stays. |
| Output folder | `GameMakerIO.WriteAsync` to a path does not create a missing folder. `PatchInputService.WriteDataFile`, `G3MProjectService.Build`, patch archive writes and `G3MProjectService.Export` create it. |
| Cache schema | `G3MCacheManifest.CurrentSchema` is 5. |

## Compiler diagnostics

`GmlDiagnostic.Code` values:

| Code | Meaning |
| --- | --- |
| `GML1001` | Lexer error. |
| `GML2001` | Syntax error. |
| `GML3001` | Unknown symbol. |
| `GML3002` | Semantic error. |
| `GML4001` | Unsupported by the game's version. |
| `GML9001` | Internal error. |

## See also

- [Patch API](patches.md)
- [Host integration](hosting.md)
- [API index](api-index.md)
