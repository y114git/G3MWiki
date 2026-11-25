# Cancellation and threading

This page covers how to stop a running operation, what it leaves behind, and how to keep background work from racing with edits to one `GameMakerData`.

## Cancel an operation

Pass a `CancellationToken` and catch `OperationCanceledException` separately from real failures:

```csharp
using G3MLib.Modding.Patching;

using var cancel = new CancellationTokenSource();
try
{
	var applied = await PatchService.ApplyPatchAsync(
		"original.win", "interface.g3mpatch", "output/patched.win",
		cancellationToken: cancel.Token);
	Console.WriteLine(applied.Success);
}
catch (OperationCanceledException)
{
	Console.WriteLine("Cancelled.");
}
```

What a cancelled call leaves behind:

| Operation | After a cancel |
| --- | --- |
| `GameMakerIO.ReadAsync` | Throws. Nothing is written. |
| `GameMakerIO.WriteAsync` to a path | The existing target is untouched. The temporary file is removed. |
| `GameMakerIO.WriteAsync` to a stream | The stream is partly written. |
| `PatchService.ApplyPatchAsync` (paths) | The data file and an existing output are untouched. Temporary files are removed. |
| `PatchService.ApplyPatchAsync` (in memory) | The `GameMakerData` may be partly changed. Discard or reload it. |
| `MergeService.MergePatchesAsync` | Existing outputs are untouched. Cancellation is ignored once the outputs are being committed. |
| `DiffService.CompareAsync` | An existing report is untouched. Temporary files are removed. |
| `ResourceExportService.ExportTypes`, `ExportSelectiveExceptCode`, batch exports | The files this call added to the output folder are removed. A file that was overwritten cannot be restored. |
| `ResourceImportService.Import`, `ImportAssetOrder` | The input folder is never changed. The `GameMakerData` may hold part of the import: discard it. |
| `G3MProjectService.Export`, `Update` | The project folder is as it was. Moving the finished staging folders into place and writing the project file are not interrupted. |
| `CodeImportGroup.Import(mode, progress, token)` in `Apply` mode | The data may hold part of the operations. Discard it. |
| `CompileGroup.Check(token)` | Nothing changed. |
| `CompileGroup.Compile(token)` | The data may hold part of the operations. Discard it. |
| `ScriptRunner.RunAsync` | Returns a result with `Cancelled` set. The same happens when its `timeout` passes. The data may hold part of what the script did. |
| `ResourceCloneService.Clone` | The token only stops the wait for the data gate. |
| `ReferenceIndex.Build`, `RenameService.PlanRenames`, `TexturePageRepacker.Plan`, `ChunkDigests.Compute`, `DebugDataGenerator.Generate`, `G3MProjectService.OpenAsync`, `AudioGroupFile.Open` | Throw. They only read. |
| `AudioGroupFile.Save` | Throws. The file is written through a temporary file, so an existing target stays as it was. |
| `DataRepair.RestoreMissingCodeLocals` | Throws. Code entries repaired before the cancel stay repaired. |
| `RenameService.Apply`, `TexturePageRepacker.Apply` | No cancel point. They are single threaded and short. |

Batch exports and imports check the token once per item, so a large item finishes before the call stops.

Resource export, import and hashing, patch create, validate and apply, merge and diff also read the token from deeper stages that cannot take a parameter. You do not need to pass it again.

## Data access gate

`GameMakerData.Gate` is a `DataAccessGate`: a reader/writer gate that the library and the host share for one data file. Any number of readers can hold it together. A writer holds it alone. Waiting writers go before new readers.

```csharp
using G3MLib.DataFile;

using (data.Gate.BeginWrite(TimeSpan.FromSeconds(5)))
{
	data.Rooms![0].Width += 16;
}
using (data.Gate.BeginRead(cancellationToken: cancellationToken))
{
	await GameMakerIO.WriteAsync("output/copy.win", data);
}
```

Both methods have the signature `(TimeSpan? timeout = null, CancellationToken cancellationToken = default)` and return an `IDisposable` scope.

- A timeout throws `TimeoutException`. A cancelled token throws `OperationCanceledException`. `Timeout.InfiniteTimeSpan` waits without a limit. Other negative timeouts throw `ArgumentOutOfRangeException`.
- Waiting blocks the calling thread. A UI thread should pass a timeout or a token.
- A reader cannot become a writer. `BeginWrite` inside a read scope throws `InvalidOperationException` instead of deadlocking.
- A scope is shared by the code that runs under it, including tasks it starts, so a script that saves the data does not wait on itself.

What the library locks:

| Takes the gate as a writer | Takes the gate as a reader |
| --- | --- |
| `ScriptRunner.RunAsync` (when the globals hold data), `ResourceImportService` (every import), `ResourceImportService.ImportAssetOrder`, `ReferenceIndex.ClearUsages`, `ResourceCloneService.Clone`, `G3MProjectService.Build` | Data file writes (`GameMakerIO.Write`, `WriteAsync`), `ResourceExportService.ExportTypes` and `ExportSelectiveExceptCode`, `G3MProjectService.Export` and `Update` |

Readers share the gate, so two data file writes of one data object can start together. They then take turns inside the write, so each one produces the same bytes. Nothing else uses the gate. `ClearedReferences.Undo` and `ResourceClone.Undo` do not take it: call them inside a write scope when a save or script can run at the same time.

### Script scopes are bound to a thread

The scope that `ScriptRunner` takes is bound to the thread that runs the script and stays bound until the script ends. Code that runs on that thread meanwhile (for example a click handler handled while a script dialog is open) is inside the scope and passes the gate. A host must not save, import or start another script from such a callback while a script runs.

## ModelLock

The gate protects library operations. `ModelLock` is a plain reader/writer lock for a host's own jobs:

```csharp
using G3MLib.DataFile;

using var modelLock = new ModelLock();
using (modelLock.Read())
{
	// decompile, export, search
}
using (modelLock.Write())
{
	data.Rooms![0].Width += 16;
}
```

It is not reentrant: taking it again on the same thread throws `LockRecursionException`. A job that must not block edits works on a snapshot, a second `GameMakerData` read from the same file.

## Rules

- Do not edit one `GameMakerData` from several jobs without the gate or a lock.
- Do not share an output path between jobs.
- Logging scopes and the xdelta executable scope follow the async flow, so concurrent jobs can use separate ones. See [Host integration](hosting.md#logs-and-progress).
- Library callbacks (`LogService`, progress reporters, `ScriptGlobals.MainThreadAction`) may arrive on any thread. Marshal to the UI thread yourself.

## See also

- [Scripts and ScriptGlobals](script-globals.md)
- [DATA files](data-files.md)
- [Host integration](hosting.md)
