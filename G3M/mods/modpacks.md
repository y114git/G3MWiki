# Priority and steps

Select the mods in the Library, then open **Priority & Steps** to arrange how they apply.

The button is available when at least two mods are selected for the current scope. In Chapter Mode, first choose the chapter whose arrangement you want to edit.

## Arrange the list

Click a step to select it. Drag a mod within that step or into another step. **Up** and **Down** move the selected mod within its current step.

**Add step** appends a step. **Remove step** moves that step's mods into the preceding step, or the following one if the first step is removed. It does not uninstall or unuse those mods. **Move step up** and **Move step down** change the order of whole steps.

Click **OK** to keep the arrangement. **Cancel** restores the arrangement from when the dialog opened. Empty steps do not create extra patching stages.

## Priority within a step

Mods in the same step use the same starting GameMaker DATA file. G3MTool combines their changes. A mod higher in the list has higher priority when changes overlap and cannot be combined.

For example, put a preferred interface replacement above another mod that edits the same interface resource. This sets the winner for conflicting changes; it does not guarantee that the resulting game logic works together.

**Code merge** attempts to combine code changes. **Properties merge** combines supported structured properties, including JSON changes. Enable these where appropriate for the selected mods and inspect the result with Diagnostics.

## Separate steps

Steps run from top to bottom. A step starts from the previous step's resulting files.

Use separate steps when an addon requires an already patched base, a version conversion must precede another patch, or a patch explicitly targets another mod's output.

Example:

| Step | Selection |
| --- | --- |
| 1 | Base overhaul |
| 2 | Addon made for that overhaul |

Putting these into one step would compare both patches with the same starting file. It would not supply the patched base the addon expects.

Dependencies and conflicts declared in [mod_config.json](mod-config.md) can describe required mods and their position. Review reported issues rather than guessing an order from the mod names.

## Check a selection

Run [Analyze Actual Launch Result](../features/diagnostics.md) to try the selection on temporary copies. Successful application checks patching; playtesting is still needed to check interactions in the game.

Full DATA replacements and loose-file overwrites can discard another mod's work on the same target. Prefer compatible resource patches when combining edits. Desktop [shortcuts](../features/shortcuts.md) can save both the selection and its steps.

To turn a tested selection into one local package, see [Create a modpack](creating-modpacks.md).
