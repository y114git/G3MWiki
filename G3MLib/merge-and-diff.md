# Merge and diff

## Merge patches

```csharp
using G3MLib.Modding.Merging;

var merged = await MergeService.MergePatchesAsync(
	"original.win",
	new List<string> { "audio.g3mpatch", "interface.g3mpatch" },
	new MergeOptions
	{
		OutputPath = "combined.g3mpatch",
		ApplyPath = "output/combined.win",
		ReportPath = "merge.md"
	});
if (!merged.Success)
{
	throw new InvalidOperationException(merged.Error);
}
Console.WriteLine($"Conflicts: {merged.TotalConflicts}");
```

Supply at least two inputs in **low-to-high priority order**. Each describes changes against the same original. Later inputs win overlaps that are not merged.

| MergeOptions property | Effect |
| --- | --- |
| OutputPath | Retains the merged `.g3mpatch` |
| ApplyPath | Writes the applied DATA result |
| UseCodeMerge | Attempts three-way combination of code edits |
| UsePropertyMerge | Combines supported JSON property changes |
| UseSequentialMerge | Uses lower-memory processing; cannot combine with either merge option above |
| ReportPath | Writes a Markdown merge report |
| CacheOptions | Supplies reusable analysis settings |

Without either output path, a timestamped merged patch is written beside the original. With only ApplyPath, the DATA result is written without retaining the patch. Explicit paths make host behavior easier to review.

A successful MergeResult can have `TotalConflicts > 0`. `AutoMerged` records automatic merges. Inspect the report and test the game; Success does not certify gameplay compatibility.

Sequential processing is a memory strategy, not an addon pipeline. For an addon requiring a base mod's output, first apply the base and then use that DATA as the addon's original.

## Compare

```csharp
using G3MLib.Modding.Diffing;

var comparison = await DiffService.CompareAsync(
	"original.win", "modified.win", "reports/changes.md",
	DiffReportMode.Full);
if (!comparison.Success)
{
	throw new InvalidOperationException(comparison.Error);
}
Console.WriteLine(comparison.OutputPath);
```

Inputs can be DATA or resource patch archives. `Standard` produces resource summaries and changed-file lists. `Full` adds detailed text, GML, JSON, and related resource comparisons.

DiffResult exposes Success, Error, OutputPath, DifferenceCount, mode, change totals, and per-type summaries where available. Check Success before using optional totals. Equal inputs are a successful comparison, rather than an error.

Comparing resources is not the same as comparing serialized bytes. Use an explicit byte comparison if exact file equality is your requirement.
