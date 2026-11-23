# UI layers

A UI layer tab edits the tree of a UI layer: flex panels, text items, sprites, objects and sequences, with their layout properties and a small live preview. This page covers every control of that tab.

A UI layer is a GameMaker structure that places user interface elements with a flexible box layout (like web flexbox). The layer is the root. Panels hold other nodes and arrange them by direction, wrap, alignment, padding, gap and size.

Open a UI layer from **UI Layers** in the Explorer.

## Node tree

The list shows the layer first, then every node indented by depth, each as `name  (type)`. Select a node to edit it. New nodes are added inside the selected node (the layer when none is selected).

| Control | Meaning |
|---|---|
| Name box | The name for the next node. Empty uses `panel` or `text` or the asset's name, with `_1`, `_2` added to keep it free. |
| **Add flex panel** | Adds an empty flex panel. |
| **Add text item** | Adds a text item. |
| **Delete node** | Deletes the selected node. The layer itself cannot be deleted. |
| **Add sprite**, **Add object**, **Add sequence** | Each has a list of the assets of that type beside it. Adds an instance of the chosen asset. |

Each change is one undoable step. A refused change shows its reason under the buttons.

## Properties

Fields appear for the selected node in groups. The group names are **Layout**, **Alignment**, **Padding**, **Size**, **Position**, **Margin** and **Display**.

| Node | Fields |
|---|---|
| Layer | **Visible**, **Draw space**, then the container fields. |
| Flex panel | Container fields, size fields, position fields, margins. |
| Sprite, object, sequence or text instance | **Visible**, **Anchor**, **Stretch width**, **Stretch height**, **Tile horizontally**, **Tile vertically**, **Keep aspect**. |

Container fields: **Direction**, **Wrap**, **Gap: Row**, **Gap: Column**, **Justify**, **Align items**, **Align content**, **Layout direction**, and **Padding: Left**, **Right**, **Top**, **Bottom**. Panel fields: **Width**, **Minimum width**, **Maximum width**, **Height**, **Minimum height**, **Maximum height**, **Grow**, **Shrink**, **Position type**, **Align self**, **Clip contents**, **Offset: Left**, **Right**, **Top**, **Bottom**, and **Margin: Left**, **Right**, **Top**, **Bottom**. Choices come from the data file's own value lists. Size, offset, padding and margin accept a number or a value with a unit.

## Preview

A 320 by 180 preview draws the layout of the layer as nested boxes. It is a quick check of sizes and order. It is not the game's renderer.

## See also

- [Image editor](image-editor.md)
- [Sprite editor](sprite-editor.md)
