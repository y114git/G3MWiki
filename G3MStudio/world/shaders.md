# Shaders

A shader is a small program that runs on the graphics card. This page covers the source tabs of a shader and the built-in check.

## Source tabs

The shader tab shows one tab for each source the shader holds. The possible tabs are **GLSL ES vertex**, **GLSL ES fragment**, **GLSL vertex**, **GLSL fragment**, **HLSL9 vertex** and **HLSL9 fragment**. A source that is empty is not shown. Each tab is a code view with line numbers and GML-style colouring of comments, numbers and shared keywords. `Ctrl+F` opens find (with replace when the file is open for editing), the same panel as in the code tab. A bar under the view changes its height.

Without a project the sources are read-only. Otherwise two buttons sit above the view:

| Button | Action |
| --- | --- |
| **Save changes** | Stores the typed source as one undo step. `Ctrl+S` does the same while the source has focus. |
| **Discard changes** | Puts the stored source back. |

Both are disabled until the text differs from the stored one. Saving the data file saves every source with typed changes first. Unsaved text survives a language change.

The remaining shader fields are in the [property list](property-grid.md); the six source fields are left out of it.

## Check line

Under each source one line reports a light check. It is not a compiler: it misses errors and never reports a type error.

| Message | Meaning |
| --- | --- |
| `No problems found (a light check, not a compiler)` | None of the checks failed. |
| `Brackets do not match` | `(`, `{` or `[` are not balanced. Comments are ignored. |
| `No main function` | The text has no `main`. |
| `The vertex shader does not set gl_Position` | GLSL vertex source without `gl_Position`. |
| `The fragment shader does not set gl_FragColor` | GLSL fragment source with neither `gl_FragColor` nor `gl_FragData`. |

HLSL9 sources get the brackets and `main` checks only.

## Make a shader

1. Choose **File** > **New** > **New Shader...**, type `sh_tint` and press **Create**. The new shader is a pass-through: it draws the texture times the draw colour. The tabs **GLSL ES vertex**, **GLSL ES fragment**, **GLSL vertex** and **GLSL fragment** hold the same source. The HLSL9 sources start empty, so those tabs are not shown.
2. Open the **GLSL ES fragment** tab. Change the last line of `main`, for example `gl_FragColor = v_vColour * texture2D(gm_BaseTexture, v_vTexcoord) * vec4(1.0, 0.6, 0.6, 1.0);`
3. Read the check line under the source. Press **Save changes**.
4. Make the same change in **GLSL fragment** so both stay alike.
5. Use it in a Draw event: `shader_set(sh_tint); draw_self(); shader_reset();`

## See also

- [Property list](property-grid.md)
- [Resource management](resources.md)
- [Code editor](../code/README.md)
