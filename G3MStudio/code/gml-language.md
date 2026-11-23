# GML Language Support

This page lists what the code editor handles of GML, and where it depends on the GameMaker version of the data file. GML is GameMaker's scripting language. Behavior is read from the open data file, so one G3MStudio build works with old and new games.

## What the editor reads

- Names come from the open data file: resource names, scripts, extension functions, and everything the compiled code already calls or sets.
- Built-in functions, variables and constants come from the compiler's list for the version of the data file, plus every name of the bundled reference, so newer functions are completed and hovered too.
- Declarations such as `function`, `constructor`, `static`, `enum`, `#macro`, `#region` and `globalvar` feed completion, outline, rename and the checks.

## Differences by version

| Area | Older data | Newer data |
|---|---|---|
| Functions in code | Scripts are plain scripts. | GameMaker 2.3 and later: methods, constructors and anonymous functions get child entries ([Code Entries](code-entries.md#child-entries)). |
| Asset references | Before GameMaker 2023.8, code names every asset by number, so decompiled code reads `draw_sprite(5, ...)`. Use [Replace Numbers with Asset Names](asset-numbers.md). | From 2023.8 assets are named in code. |
| Dead resources | In data older than 2023.8 no resource counts as safe, because numbers hide uses ([Dead Resources](dead-resources.md)). | Uses are found by the reference index. |
| String escapes | Data before GameMaker Studio 2: rule G3M109 is off. | GameMaker Studio 2 and later: G3M109 checks escapes. |
| Format functions | | `string`, `show_debug_message`, `string_ext` and `show_debug_message_ext` accept extra arguments. |

G3MStudio reads the version from the general info of the data file (major, minor, release, build), not from the release channel. An LTS release and a regular release with the same version number behave the same.

## What is not checked

There is no type system. Member completion after a dot is a guess from the text, and the checks report only what they are sure of. Names that exist only at run time (set by another event, found with `asset_get_index`) can be reported by the checks or listed as dead. See [Code Checks and Lint Rules](lint-rules.md).

## See also

- [GML Intelligence](gml-intelligence.md)
- [Code Entries](code-entries.md)
- [Code Editor](code-editor.md)
