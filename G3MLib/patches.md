# Patch API

Patch services are in `G3MLib.Modding.Patching`. Their results expose `Success` and `Error`; check those values before using a generated output.

## Create

```csharp
using G3MLib.Modding.Patching;

var result = await PatchService.CreatePatchAsync(
	"original.win", "modified.win", "interface.g3mpatch");
if (!result.Success)
{
	throw new InvalidOperationException(result.Error);
}
Console.WriteLine(result.Statistics?.TotalChanged);
```

The first two arguments are readable DATA files. The output is a resource patch archive. Optional `includeXdeltaFallback: true` embeds a binary fallback and requires a configured xdelta executable. Optional `cacheOptions` controls reusable analysis.

Optional precomputed inputs are available for a host that already has the corresponding original-file analysis. They must describe the actual supplied original; omit them in ordinary usage.

## Validate

```csharp
using G3MLib.Modding.Patching;

var validation = await PatchService.ValidatePatchAsync(
	"interface.g3mpatch", dataPath: "original.win");
if (!validation.Success)
{
	throw new InvalidOperationException(validation.Error);
}
Console.WriteLine(validation.Manifest?.Original?.Filename);
```

The optional original adds compatibility checks. Without it, the service validates the package rather than proving compatibility with an arbitrary game file. The successful result provides the parsed Manifest.

## Apply

```csharp
using G3MLib.Modding.Patching;

var applied = await PatchService.ApplyPatchAsync(
	"original.win", "interface.g3mpatch", "output/patched.win");
if (!applied.Success)
{
	throw new InvalidOperationException(applied.Error);
}
```

`allowXdeltaFallback` defaults to false and controls trying the embedded binary alternative first. `verifyModifiedHash` defaults to true; retain normal checking unless the host has a specific, documented reason to change it. Resource reconstruction and binary reproduction have different guarantees.

The output can include external audio groups. Test it with the corresponding runner and resources rather than assuming the main `.win` file is the whole game.

## Normalize another input

`EnsureG3MPatchAsync(originalPath, inputPath, tempDir, ..., cacheOptions)` returns a resource-patch path. `PatchInputService.MaterializeDataAsync(originalPath, inputPath, tempDir)` returns derived DATA for supported DATA/resource/binary patch inputs.

Provide a dedicated temporary directory and retain it until the returned file has been consumed. These methods do not provide the G3MTool CLI's CSX execution layer. A library host must execute scripts through its selected script environment before passing their DATA result to ordinary patch services.

The [patch format](../G3MTool/patch-format.md) describes the archive's user-relevant contents. It is separate from a G3M mod package.
