# Go To and Project Symbols

Go To is one fuzzy search over every kind of entry: resources, code, functions, variables, strings, commands, tabs and settings pages. This page also explains the project symbol index that lets the code editor know names declared in other code entries.

## Go To

Press `Ctrl+T` or choose **View > Go to...**. The window is titled **Go To**, with a results list and a preview of the selected entry on the right. With no text it lists recent entries. Without a prefix, the big lists (strings, functions, variables) stay out until you type two letters.

Placeholder: "Type a name. o: objects, @ symbols, # strings, > commands, t: tabs". A prefix narrows the search:

| Prefix | Searches |
|---|---|
| `o:` | objects |
| `s:` | sprites |
| `r:` | rooms |
| `x:` | scripts |
| `a:` | sounds |
| `n:` | fonts |
| `spr:` `snd:` `scr:` `shd:` `fnt:` `tl:` `obj:` `room:` `seq:` `curve:` `ps:` `path:` `bg:` `ext:` | the matching resource type (same as the Explorer filter) |
| `c:` | code entries |
| `f:` | functions and the function reference |
| `v:` | variables |
| `@` | functions and variables |
| `#` | strings |
| `>` | commands (the command palette) |
| `t:` | open tabs |
| `p:` | Settings pages |

Result kinds shown in the list: Resource, Code, String, Function, Variable, Command, Settings page, Tab, Reference. The best match comes first; ties keep the recent order. `Ctrl+P` (Quick Open) finds a resource by name the same way.

## Project symbols

A name such as a `#macro`, an enum, a `globalvar`, or a variable made with `variable_instance_set` and a string literal is declared in one code entry and used by all. G3MStudio reads these declarations from every entry into an index in the background.

The index is used for:

- **Code checks.** A name found in the index is not reported as unknown (see [Code Checks and Lint Rules](lint-rules.md)). Until the first index is built, the check does not report unknown names. Code checked while the index was missing is checked again when it is ready.
- **Hover.** A macro or enum of another entry shows its declaring line and "defined in {entry}".
- **Expanding macros.** The live parse expands macros other entries declare, so they are not reported as errors.

After an edit the index is built again, and the last one answers meanwhile.

## See also

- [GML Intelligence](gml-intelligence.md)
- [Find Usages](find-usages.md)
- [Code Editor](code-editor.md)
