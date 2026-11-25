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

A successful MergeResult can have `TotalConflicts > 0`. `AutoMerged` records automatic merges. Inspect the report and test the game; Success does not mean the game plays correctly.

`MergePatchesAsync` and `DiffService.CompareAsync` accept an optional `cancellationToken`; see [Cancellation and threading](cancellation-and-threading.md).

| MergeResult property | Content |
| --- | --- |
| `Success`, `Error` | Outcome. A missing original, a missing patch, fewer than two patches, or `UseSequentialMerge` combined with a merge option returns `Success = false` with a message. A malformed `g3mpatch.json` in an input returns an error that names the patch path. |
| `OutputPath` | The retained `.g3mpatch`, when one was written. |
| `TotalConflicts` | Overlaps that were resolved by priority. |
| `AutoMerged` | Overlaps merged automatically. |

### Merge by name

With `UsePropertyMerge` on, lists that hold plain names are merged by name against the original list instead of by position: global init scripts and game end scripts (`GlobalScripts/global_scripts.json`), tags (`Tags/tags.json`) and feature flags (`FeatureFlags/feature_flags.json`). Additions and removals from every patch are kept, and the earlier patch's additions come first at a shared spot. A list with repeated names is merged as before. Two patches that make the same edit merge cleanly. A resource filter finds resources in patch archives and exports made before the folder names carried a hash suffix, in both folder name forms.

Reference scans during a merge stop after 2 seconds per pattern and log a warning when one does.

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

A resource counts as changed when its content hash changes. The hash covers what a resource is, not where it sits: a sprite frame that only moved to another spot or page of the texture atlas, and a sound that only moved to another audio slot, hash the same while their pixels or audio match. A real pixel or audio change still shows. Hashes that older versions cached are rebuilt once (cache schema 4).

Inputs can be DATA or resource patch archives. `Standard` produces resource summaries and changed-file lists. `Full` adds detailed text, GML, JSON, and related resource comparisons.

`DiffResult` exposes `Success`, `Error`, `OutputPath`, `DifferenceCount`, `Mode`, the nullable totals `TotalChanged`, `TotalNew`, `TotalDeleted` and `TextDiffCount`, and `ByType`, a dictionary of resource type to `DiffTypeSummary` (`Changed`, `New`, `Deleted`). Check `Success` before using optional totals. Equal inputs are a successful comparison, rather than an error. `DiffReportMode` is `Standard` or `Full`; the cache parameter is `G3MCacheOptions`.

Text diffs in the report use a longest common subsequence with context hunks and a size guard for very large files. Event names of keyboard, trigger and gesture events are parsed to readable names. A patch archive given as an input is extracted for the comparison with the same name and size checks as an apply: unsafe manifest paths are skipped, and the extraction can be cancelled between entries and chunks. A malformed manifest returns an error result.

The report data types are public: `ResourceDiffSummary` (`ByType`, `TextDiffs`, totals), `DiffResourceTypeChanges` (`Changed`, `New`, `Deleted` name lists) and `TextFileDiff` (`ResourceType`, `ResourceName`, `FileName`, `UnifiedDiff`).

Comparing resources is not the same as comparing serialized bytes. Use an explicit byte comparison if exact file equality is your requirement.
