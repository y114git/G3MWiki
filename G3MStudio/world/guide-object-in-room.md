# Guide: make an object and place it in a room

This guide makes an object `obj_coin` with a sprite and a Create event, then puts two of them in a room. Result: a room with two instances that you can run with **Play This Room**.

You need an open data file and a sprite. To make a sprite first, see [Guide: import a sprite strip](../graphics/guide-import-sprite-strip.md).

## Make the object

1. Choose **File** > **New** > **New Object...**.
2. Type `obj_coin` in `Name of the new Object` and press **Create**. The object opens in a tab.
3. In the **Sprite** row press **Set...**, type part of the sprite name in `Search...`, press `Down` and `Enter`. The preview on the left shows the sprite. Press **Play** to see it animate.
4. Press **Add event...** and choose **Create**. A code tab opens for the empty entry.
5. Type `image_speed = 0.5;` and press **Save changes**.

The event list now shows `Create  ->  gml_Object_obj_coin_Create_0`. See [Objects and events](objects.md) for the other event types.

## Place it in a room

1. Double click a room in the Explorer.
2. In the layers panel on the right make sure an instance layer exists and is selected. If there is none, choose **Instances** in the kind box and press **Add**. A room without one answers `This room has no instance layer. Add one in the layers panel first.`
3. Select `obj_coin` in the Explorer.
4. Choose **Place** in the tool box. The hint reads `Click in the room to place obj_coin.`
5. Click twice in the room. Each click adds an instance. You can also drag `obj_coin` from the Explorer onto the canvas.
6. Choose **Select** in the tool box and click one instance. The Inspector shows `Instance {id} (1 selected)` with **X**, **Y**, **Scale X**, **Scale Y**, **Rotation**, **Frame**, **Speed** and **Object**.
7. Type new numbers in **X** and **Y**, or drag the instance, or press the arrow keys (`Shift` moves 8 pixels). Turn on **Snap** (`Shift+G`) to move on the grid.

## Jump between the pieces

- **Go to Object** (`Ctrl+F12`) opens `obj_coin` from the selected instance.
- **Go to Sprite** (`Alt+Shift+F12`) opens its sprite. With nothing selected it opens the tile set or sprite of the selected layer.
- **Edit Creation Code** in the Inspector opens code that runs for this one instance only.

## Test it

1. Press `Ctrl+S` to save the data file.
2. Choose **Room** > **Play This Room** (`Ctrl+Shift+F5`). The game starts in this room. See [Run the game](../projects/run-game.md).

If the instances do not show in the game, check that the layer is visible (the eye toggle in the layers panel) and that the room is the one the game runs.

## See also

- [Room editor canvas](room-canvas.md)
- [Room instances and elements](room-instances.md)
- [Room layers](room-layers.md)
- [Objects and events](objects.md)
