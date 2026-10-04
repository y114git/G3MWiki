# Mod editor

The editor creates and edits local mods. It writes `mod_config.json` and keeps the files needed by the package. Open it from **Add Mod** when creating a mod, or from an installed mod's edit action.

The five tabs are **Metadata**, **Compatibility**, **Files**, **Placeholders**, and **Help**. The [configuration reference](mod-config.md) defines the values these controls write.

## Metadata

| Control | Use |
| --- | --- |
| Game | Select a built-in game or a visible custom game. The mod belongs to that game's library. |
| Mod name | Name displayed in the library. |
| Authors | Author names, separated by commas in the desktop editor. |
| Short description | Description of the mod. |
| Homepage | HTTP or HTTPS project or download page. |
| Icon | Bundled image path or HTTP or HTTPS image URL. The browse button selects a local image. The preview updates when you edit the path or URL. |
| Tags | Mark text edits, customization, gameplay changes, or other content. |
| Overall mod version | Version label for this package. |
| Game version | Label describing the intended game release. |

A mod's ID is its stable identity in the library and in relationships. Editing an existing mod preserves that identity. A game-version label alone does not prevent a patch from running on a different data file. Use a target hash when an exact file is required.

## Compatibility

The **Dependencies** and **Conflicts** lists each have **Add** and **Remove** controls. Select a row to edit its mod ID and **Order**.

| Order choice | Configuration value |
| --- | --- |
| No order | Bare mod ID. |
| Before | `id:before` |
| After | `id:after` |
| Earlier step | `id:before-step` |
| Later step | `id:after-step` |
| Earlier in this step | `id:before-priority` |
| Later in this step | `id:after-priority` |

The ID names the other mod. Dependency conditions require that mod to be selected in the specified position. Conflict conditions report a problem when the other mod is selected in the specified position. See [relationship rules](mod-config.md#dependencies-and-conflicts) for the precise position checks.

For an addon that requires an already applied base mod, use **Earlier step**. Priority within one step chooses which conflicting changes win; it does not create a separate patched base for an addon.

## Files

The left pane lists operations in processing order. The right pane edits the selected operation or group.

| Control | Action |
| --- | --- |
| Add | Add an operation. Configure its type and paths before saving. |
| Add group | Create a named group of operations. |
| Remove | Remove the selected operation or group. Removing a group removes its contained operations. |
| Drag a row | Move an operation or group in the sequence, including into another group. |
| Up and down arrows | Change the selected item's position without dragging. |
| Type | Choose patching, copying, extraction, a soft or hard variant, or information. |
| Source | Select the input file or folder. A source inside the mod is stored as a `${mod_path}` path. |
| Target | Select the destination. A trailing `/` identifies a directory. |
| Include source hash | Store a checksum of the resolved source. |
| Include target hash | Store a checksum of the resolved existing destination. |

Groups organize the operation sequence. They are not library mod steps. An `info` operation has no destination, so target controls do not apply to it.

Hash controls resolve the path against the selected game's settings and the mod folder. A source or target that cannot be read cannot produce a valid checksum. Hashes can check files, folders, or supported archive members.

Choose **overwrite** to copy a source folder itself, or **extract** to copy its contents. The hard variants clear destination content; review the [clearing rules](mod-config.md#clear-a-destination) before using them.

## Placeholders

Use this tab to name paths that several operations share. **Add** creates a row, **Remove** deletes it, and selecting a row opens its name and path fields.

For example, a placeholder named `chapter_assets` with the value `${game_path}/chapter1_windows/assets` lets operations use `${chapter_assets}/logo.png`.

The value begins with a built-in root and a path suffix. Names cannot duplicate built-in roots or other names without regard to case. Custom placeholders cannot contain other custom placeholders. Removing a placeholder does not rewrite operation paths that use it; fix those references before saving.

## Help and validation

**Help** explains metadata, file operations, placeholders, and compatibility. Path examples use the selected game and local folders where available.

Saving checks the configuration and required paths. Invalid values receive an explanation so you can correct the affected operation or field. A valid package still needs a real test in the intended game, particularly when a patch depends on a particular game build.

Saving a local mod writes it to the current profile. Use [Export](exporting.md) to share a ZIP. The browser editor has different save controls because it cannot write directly into a desktop library; see [Web editor](web-editor.md).
