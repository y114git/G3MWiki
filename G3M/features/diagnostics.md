# Launch diagnostics

Open **Diagnostics** from the Library after choosing a game, section, profile, and mod selection. The initial inspection reads mod contents; **Analyze Actual Launch Result** additionally applies them to temporary copies. The window is labelled **Mod Diagnostics (BETA)**.

## Inspection views

Use the mod checkboxes to choose which mods to inspect and the resource-type controls to narrow DATA results. Selecting a file, resource, or issue shows its details in the inspector.

| View | Contents |
| --- | --- |
| Overview | Selection summary and counts |
| File Tree | Operations, source files, destinations, and overlaps |
| DATA Impact | Available patch-resource information |
| Preview / Compare | Supported resource previews for comparison |
| Problems | Missing files, invalid targets, relationships, and reported conflicts |

Double-click a supported file or DATA entry to open its inspection view. Audio previews provide Play and Stop controls. Preview availability depends on the resource type and readable patch data; a binary patch cannot list its final resource changes without application.

Two operations targeting the same file are an overlap to investigate. They are not always incompatible: priority, steps, and operation type determine the result.

## Analyze the actual launch result

Click **Analyze Actual Launch Result** to prepare temporary game files and execute the selected operations and priority steps. The action does not start the game or install the result into the configured game folder.

The analysis can invoke G3MTool, xdelta, and bundled scripts. Scripts are executable code, so analyze only content you trust. Temporary output is not a sandbox for arbitrary scripts.

The resulting views are **Actual Steps**, **Actual Resources**, and **Actual Files**. Actual Steps shows completion, duration, selected mods, and errors for each section and step. Actual Resources identifies affected resources and contributing mods. Actual Files lists changed paths with their before and after sizes and checksums.

Use the resource search to find a name, operation, or mod. Select a result to read its details.

**Cancel Analysis** requests cancellation. **Export Exact Report** asks for an HTML destination and writes the HTML report and an accompanying JSON report. Check warnings, failed operations, incomplete scans, and inaccessible paths before treating the analysis as successful.

## What the result establishes

A successful analysis establishes that the inspected files can be processed with that selection and tool configuration. It does not playtest the game or prove that merged scripts behave correctly.

Run the analysis again after changing mod contents, game files, order, steps, or profile. For a launch that fails despite successful application, check game logs and reduce the selected mods to isolate the interaction.
