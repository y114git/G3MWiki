# Textures and texture pages

A texture page is one large picture inside the data file that holds many smaller pictures: sprite frames, tile sets, fonts and more. This page covers the page tab, page items, export and replace, resizing a page, repacking and texture groups.

Each small picture is a texture page item: a rectangle on a page, plus an offset inside the bounding box the game draws. Studio places the pictures you import when you save.

## Texture page tab

Open a page from **Embedded Textures** in the Explorer. The tab shows the page at its real size in a scroll view, with a chequer pattern behind transparent pixels, and the property list below. Above the picture:

| Control | Meaning |
|---|---|
| `{count} items on this page` | The number of page items on the page. |
| **Export page...** | Saves the page as a PNG (`Save the page as`). Message `Saved {page} to {path}.` |
| **Replace page...** | Asks for a PNG (`Choose a PNG of the page's size`). One undoable step. Message `Replaced the picture of {page}.` |
| **Move to group...** | Opens the Texture Groups window. |
| **Resize page...** | See [Resize and fit](resize-and-fit.md). |
| **Edit Image...** | Opens the [image editor](image-editor.md) on the page. A double click on the picture does the same. |
| Size | The page size, for example `2048 x 2048`. |

Hover the picture to outline the page item under the pointer. The tooltip says `Page item at {x}, {y}, {width} by {height}`. Where items overlap, the smallest one wins. The context menu has **Find Usages**.

The same commands are in the **Tools** menu: **Export Texture Page...**, **Replace Texture Page...**, **Resize Texture Page...**. They act on the page selected in the Explorer. With none they say `Select a texture page first.`

A replacement picture of another size is a warned refusal: `Replace the page with a picture of another size?` The warning says that every sprite frame, glyph and tile on the page points at a rectangle on it, so another size can show the wrong part or nothing. You may overrule it. Undo brings the old picture back until you save.

A failed export or replace shows `'{name}' cannot be used: {reason}`.

![A texture page tab with the page picture and the Export page, Replace page and Resize page buttons](../images/texture-page-tab.png)

## Page items, embedded images and fonts

A font, tile set, **Texture Page Items** entry or **Embedded Images** entry shows its picture in a viewer with a zoom slider (1x to 8x; `Ctrl` + wheel zooms too, at any scroll position) and the buttons **Export PNG**, **Replace PNG** and **Edit Image...**.

Export asks for a file (default `texture.png`). The caption shows `Saved to {path}` or `Could not save: {reason}`. Replace asks for `Picture for {name}` and shows `Picture of {name} replaced.` If the item sits in a larger bounding box, the box is outlined and the picture is drawn where the game draws it.

## Texture groups

**Texture Groups...** (Tools menu) lists groups, their members and the target group. Pick members, choose **Move picked to** a group and press **Move**. The message is `Moved {count} to {group}.` Groups can be made (**New group**) and renamed (**Rename group**). A name that is not plain (letters, digits and underscores, not starting with a digit) asks `Use this name?` because GML code cannot use it. A data file without groups says `This data file has no groups.` A sprite can also change group in the Resize dialog.

## Repack texture pages

**Repack Texture Pages...** (Tools menu) plans a new packing of every texture group. The question shows page counts: `The pages of every texture group would go from {before} to {after}. This cannot be undone: save to keep it, or revert to the saved file. Repack now?` It needs a file without unsaved changes. Afterwards: `Repacked: pages went from {before} to {after}. Save to keep it, or revert to the saved file.` The question can be silenced with **Don't ask again** (see the prompts settings in `../settings/`).

## Unknown formats

`Textures in a format that cannot be shown: {count}. They are kept as they are when you save.` appears for textures the reader does not decode.

## See also

- [Resize and fit](resize-and-fit.md)
- [Image editor](image-editor.md)
- [Sprite frames and import](sprite-frames.md)
- [Image formats and limits](image-formats.md)
