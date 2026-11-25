# Command reference

Every action in G3MStudio is a command with an ID, a name and an optional default shortcut. The menus, the command palette and the shortcut editor all read this one list. This page lists every built-in command.

## How to read the tables

- **ID** is the stable name of the command. Shortcut overrides are stored by ID.
- **Name** is the English label shown in menus and in the command palette (`Ctrl+Shift+P`). Panel commands appear in the palette as "Show {panel}". Image editor commands appear as "Image > {group} > {name}".
- **Default shortcut** is blank when the command has none. You can give any command another shortcut, or a mouse button gesture, in the shortcuts editor (see [Shortcuts](../settings/README.md)). On macOS read `Ctrl` as `Cmd`.
- **Works when** is the condition that must hold. The command is greyed out in menus and missing from the palette while it does not hold. A shortcut for it does nothing.
- Keys written `Num +` are on the number pad.
- A command can have extra keys (aliases) that run it too. The built-in ones are shown after "also".
- Three menu entries are submenus, not commands, so they have no ID: **File > Open Recent**, **Project > Recent Projects** and **Tools > Run Recent Script** (the last 10 scripts run, with **Clear Items** at the end). See [Menus](menus.md).
- Plugins can add commands. They appear in the **Plugins** menu and in the palette, with the title the plugin gives.

Conditions: "data file open" means any data file is loaded. "sound in front or selected" means the active tab, or the Explorer selection, is a sound; the same holds for fonts and picture resources.

## File commands (`file.*`)

| ID | Name | Default shortcut | Works when |
|---|---|---|---|
| `file.newData` | New Data File... | `Ctrl+N` | always |
| `file.open` | Open... | `Ctrl+O` | always |
| `file.save` | Save | `Ctrl+S` | data file open |
| `file.saveAs` | Save As... | `Ctrl+Shift+S` | data file open |
| `file.saveCopy` | Save a Copy... |  | data file open |
| `file.revert` | Revert to Saved... |  | data file open |
| `file.exportSelection` | Export Selected Resources to a Folder... |  | data file open |
| `file.exportAll` | Export All Resources to a Folder... |  | data file open |
| `file.transfer` | Transfer Resources... |  | data file open |
| `file.import` | Import Resources from Another Data File... |  | data file open |
| `file.importPackage` | Import Resources from a Package... |  | data file open |
| `file.importFiles` | Import Pictures, Sounds and Scripts from Files... |  | data file open |
| `file.importSheetNew` | New Sprite from Sheet or Strip... |  | data file open |
| `file.importCodeFolder` | Import Code from a Folder... |  | data file open |
| `file.importCodeFiles` | Import Code from Files... |  | data file open |
| `file.close` | Close | `Ctrl+Alt+W` | data file open |
| `file.exit` | Exit | `Ctrl+Q` | always |
| `file.new.objects` | New Object... |  | data file has a section for the type |
| `file.new.scripts` | New Script... |  | data file has a section for the type |
| `file.new.paths` | New Path... |  | data file has a section for the type |
| `file.new.timelines` | New Timeline... |  | data file has a section for the type |
| `file.new.sounds` | New Sound... |  | data file has a section for the type |
| `file.new.rooms` | New Room... |  | data file has a section for the type |
| `file.new.sprites` | New Sprite... |  | data file has a section for the type |
| `file.new.shaders` | New Shader... |  | data file has a section for the type |
| `file.new.fonts` | New Font... |  | data file has a section for the type |
| `file.new.sequences` | New Sequence... |  | data file has a section for the type |
| `file.new.animationCurves` | New Animation Curve... |  | data file has a section for the type |
| `file.new.particleSystems` | New Particle System... |  | data file has a section for the type |
| `file.new.extensions` | New Extension... |  | data file has a section for the type |

## Help commands (`help.*`)

| ID | Name | Default shortcut | Works when |
|---|---|---|---|
| `help.checkUpdates` | Check for Updates... |  | always |
| `help.gettingStarted` | Getting Started... |  | always |
| `help.tour` | Interface Tour... |  | always |
| `help.about` | About G3MStudio |  | always |
| `help.sendFeedback` | Send Feedback... |  | always |
| `help.reportProblem` | Report a Problem... |  | always |
| `help.openLogsFolder` | Open Logs Folder |  | always |
| `help.copyDiagnostics` | Copy Diagnostics |  | always |
| `help.openDataFolder` | Open Data Folder |  | always |
| `help.documentation` | Documentation | `F1` | always |

## Run commands (`run.*`)

| ID | Name | Default shortcut | Works when |
|---|---|---|---|
| `run.game` | Run Game | `F5` | data file open |
| `run.withRuntime` | Run with Runtime... |  | data file open |
| `run.restart` | Restart Game | `Ctrl+F5` | data file open |
| `run.stop` | Stop Game | `Shift+F5` | always |
| `run.migrateOptions` | Move Run Options to G3MStudio's Folder |  | data file open |

## Edit commands (`edit.*`)

| ID | Name | Default shortcut | Works when |
|---|---|---|---|
| `edit.editAudio` | Edit Audio... |  | data file open |
| `edit.replaceAudio` | Replace Audio... |  | data file open |
| `edit.generateSound` | Generate Sound... |  | data file open |
| `edit.recordSound` | Record Sound... |  | data file open |
| `edit.exportAudio` | Export Audio... |  | data file open |
| `edit.playSound` | Play or Pause Sound |  | sound in front or selected |
| `edit.stopSound` | Stop Sound |  | sound in front or selected |
| `edit.loopSound` | Loop Sound |  | sound in front or selected |
| `edit.replaceFont` | Replace Font File... |  | font in front or selected |
| `edit.editImage` | Edit Image... |  | picture resource in front or selected |
| `edit.replaceFrame` | Replace Sprite Frame... |  | sprite in front |
| `edit.duplicateFrame` | Duplicate Sprite Frame |  | sprite in front |
| `edit.addFrame` | Add Sprite Frame |  | sprite in front |
| `edit.resizeSprite` | Resize Image/Canvas... |  | sprite in front |
| `edit.spritePlay` | Play or Pause Sprite |  | sprite in front |
| `edit.spriteLoop` | Loop Sprite Playback |  | sprite in front |
| `edit.spriteOnion` | Onion Skin |  | sprite in front |
| `edit.spriteLockOrigin` | Lock Origin |  | sprite in front |
| `edit.recalculateMask` | Recalculate Collision Mask |  | sprite in front |
| `edit.importMask` | Import Mask... |  | sprite in front |
| `edit.exportMask` | Export Mask... |  | sprite in front |
| `edit.exportGif` | Export as GIF... |  | sprite in front |
| `edit.importSheet` | Import Sheet or Strip... |  | sprite in front |
| `edit.exportAllAudio` | Export All Audio... |  | data file open |
| `edit.removeFrame` | Delete Sprite Frame |  | sprite in front |
| `edit.undo` | Undo | `Ctrl+Z` | data file open |
| `edit.redo` | Redo | `Ctrl+Y` | data file open |
| `edit.rename` | Rename... | `F2` | resource or folder selected |
| `edit.delete` | Delete Resource... |  | resource or folder selected |
| `edit.duplicate` | Duplicate Resource... |  | resource or folder selected |
| `edit.copyToOtherFile` | Copy to Other Data File... |  | data file open |
| `edit.reveal` | Reveal in Explorer |  | data file open |
| `edit.toggleFavourite` | Add to or Remove from Favourites |  | data file open |
| `edit.addTag` | Add Tag... |  | data file open |
| `edit.moveUp` | Move Up |  | resource or folder selected |
| `edit.moveDown` | Move Down |  | resource or folder selected |
| `edit.copyName` | Copy Name |  | resource or folder selected |
| `edit.findUnreferenced` | Find Unreferenced Assets |  | data file open |
| `edit.findChildren` | Find Child Objects |  | data file open |
| `edit.selectAll` | Select All | `Ctrl+A` | data file open |
| `edit.find` | Find | `Ctrl+F` | data file open |
| `edit.findInCode` | Find in Code... | `Ctrl+Shift+F` | data file open |
| `edit.replaceInCode` | Replace in Code... | `Ctrl+Shift+H` | data file open |
| `edit.assetNumbers` | Replace Numbers with Asset Names in All Code... |  | data file open |
| `edit.findUsages` | Find Usages | `Shift+F12` | data file open |
| `edit.findUses` | Find What It Uses |  | data file open |
| `go.object` | Go to Object | `Ctrl+F12` | one instance selected in the room in front |
| `go.sprite` | Go to Sprite | `Alt+Shift+F12` | the thing in front has a sprite or tile set to go to |

## Mod commands (`mods.*`)

| ID | Name | Default shortcut | Works when |
|---|---|---|---|
| `mods.createPatch` | Create Patch... |  | data file open |
| `mods.applyPatch` | Apply Patch... |  | always |
| `mods.mergePatches` | Merge Patches... |  | always |
| `mods.compareDataFiles` | Compare Data Files... |  | always |
| `mods.exportPack` | Export Mod Pack... |  | data file open |
| `mods.rememberBase` | Remember This File as the Mod Base |  | data file open |
| `mods.clearBase` | Clear the Mod Base Cache |  | always |
| `mods.applyPack` | Apply Mod Pack... |  | data file open |
| `mods.exportMod` | Export as Mod ZIP... |  | data file open |
| `mods.validatePatch` | Validate Patch... |  | always |
| `mods.applyMod` | Apply Mod... |  | always |

## Settings commands (`settings.*`)

| ID | Name | Default shortcut | Works when |
|---|---|---|---|
| `settings.resetPrompts` | Ask Again for All Prompts |  | always |
| `settings.open` | Settings | `Ctrl+,` | always |

## Tools commands (`tools.*`)

| ID | Name | Default shortcut | Works when |
|---|---|---|---|
| `tools.exportPage` | Export Texture Page... |  | data file open |
| `tools.replacePage` | Replace Texture Page... |  | data file open |
| `tools.resizePage` | Resize Texture Page... |  | data file open |
| `tools.repackTextures` | Repack Texture Pages... |  | data file open |
| `tools.plugins` | Plugins... |  | always |
| `tools.strings` | Strings... |  | data file open |
| `tools.roomOrder` | Room Order... |  | data file open |
| `tools.textureGroups` | Texture Groups... |  | data file open |
| `tools.audioGroups` | Audio Groups... |  | data file open |
| `tools.hexView` | Hex View of Data File... |  | data file open |
| `tools.offsetMap` | Write Chunk Offset Map |  | data file open |
| `tools.exportChunk` | Export Raw Chunk... |  | data file open |
| `tools.importChunk` | Replace Chunk from File... |  | data file open |
| `tools.writeDebugData` | Write Debug Data... |  | data file open |
| `tools.disassemble` | Disassembly of Code Entry... |  | data file open |
| `tools.runScript` | Run Script... |  | data file open |
| `tools.runCommand` | Run C# Command... |  | data file open |

## Room commands (`room.*`)

| ID | Name | Default shortcut | Works when |
|---|---|---|---|
| `room.creationCode` | Room Creation Code... |  | room in front |
| `room.creationOrder` | Instance Creation Order... |  | room in front |
| `room.exportTiles` | Export Tiles as CSV... |  | room in front |
| `room.importTiles` | Import Tiles from CSV... |  | room in front |
| `room.imageToTiles` | Convert Image to Tile Layer... |  | room in front |
| `room.play` | Play This Room | `Ctrl+Shift+F5` | room in front |
| `room.savePng` | Save Room as PNG... |  | room in front |
| `room.autoTile` | Toggle Auto-Tile Brush |  | room in front |
| `room.retileSelection` | Re-Tile Selection |  | room in front |
| `room.addLayer` | Add Layer |  | room in front |
| `room.duplicateLayer` | Duplicate Layer |  | room in front |
| `room.toggleGrid` | Show or Hide Grid |  | room in front |
| `room.toggleSnap` | Snap to Grid On or Off |  | room in front |
| `room.zoomFit` | Fit Room to Window |  | room in front |
| `room.zoomActual` | Zoom to 100% |  | room in front |

## Project commands (`project.*`)

| ID | Name | Default shortcut | Works when |
|---|---|---|---|
| `project.open` | Open Project... |  | always |
| `project.new` | New Project... |  | data file open, no project |
| `project.enable` | Enable Project |  | data file open, no project |
| `project.close` | Close Project |  | project linked |
| `project.sync` | Sync to Data File |  | project linked |
| `project.options` | Project Settings... |  | project linked |
| `project.resolve` | Resolve Outside Changes... |  | project linked |
| `project.moveDataFile` | Change Data File Location... |  | project linked |
| `project.disable` | Disable Project... |  | project linked |

## Tab commands (`tabs.*`)

| ID | Name | Default shortcut | Works when |
|---|---|---|---|
| `tabs.next` | Next Tab | `Ctrl+Tab` | data file open |
| `tabs.previous` | Previous Tab | `Ctrl+Shift+Tab` | data file open |
| `tabs.close` | Close Tab | `Ctrl+W` | data file open |
| `tabs.closeOthers` | Close Other Tabs |  | data file open |
| `tabs.closeAll` | Close All Tabs | `Ctrl+Shift+W` | data file open |
| `tabs.reopen` | Reopen Closed Tab | `Ctrl+Shift+T` | data file open |
| `tabs.back` | Back | `Alt+Left` | data file open |
| `tabs.forward` | Forward | `Alt+Right` | data file open |
| `tabs.historyBack` | Previous Tab (History) | `Ctrl+Alt+Left` | tab history has an earlier tab |
| `tabs.historyForward` | Next Tab (History) | `Ctrl+Alt+Right` | tab history has a later tab |
| `tabs.pin` | Pin Tab |  | data file open |
| `tabs.keepOpen` | Keep Tab Open |  | data file open |
| `tabs.splitRight` | Split Right | `Ctrl+\` | data file open |

## View commands (`view.*`)

| ID | Name | Default shortcut | Works when |
|---|---|---|---|
| `view.commandPalette` | Command Palette... | `Ctrl+Shift+P` | always |
| `view.fullscreen` | Toggle Full Screen | `F11` | always |
| `view.nextRegion` | Focus Next Region | `F6` | always |
| `view.prevRegion` | Focus Previous Region | `Shift+F6` | always |
| `view.zoomIn` | Zoom In | `Ctrl++` | always |
| `view.zoomOut` | Zoom Out | `Ctrl+-` | always |
| `view.zoomReset` | Reset Zoom | `Ctrl+0` | always |
| `view.panel.explorer` | Explorer |  | always |
| `view.panel.search` | Search |  | always |
| `view.panel.inspector` | Inspector |  | always |
| `view.panel.problems` | Problems |  | always |
| `view.panel.output` | Output |  | always |
| `view.panel.usages` | Usages |  | always |
| `view.panel.reference` | Function Reference |  | always |
| `view.panel.history` | History |  | always |
| `view.panel.git` | Git |  | always |
| `view.panel.openeditors` | Open Editors |  | always |

## Go To commands (`go.*`)

| ID | Name | Default shortcut | Works when |
|---|---|---|---|
| `go.quickOpen` | Go to Resource... | `Ctrl+P` | data file open |
| `go.goTo` | Go to... | `Ctrl+T` | data file open |

## Layout and window commands (`window.*`)

| ID | Name | Default shortcut | Works when |
|---|---|---|---|
| `window.layout.studio` | G3MStudio Layout |  | data file open |
| `window.layout.code` | Code Layout |  | data file open |
| `window.layout.canvas` | Canvas Layout |  | data file open |
| `window.layout.wide` | Wide Editor Layout |  | data file open |
| `window.layout.minimal` | Minimal Layout |  | data file open |
| `window.layout.save` | Save Layout... |  | data file open |
| `window.layout.reset` | Reset Layout |  | data file open |
| `window.showAll` | Show All Windows |  | data file open |
| `window.closeAll` | Close All Windows |  | data file open |
| `window.layout.saved` | Saved Layouts... |  | data file open |

## Dock commands (`dock.*`)

| ID | Name | Default shortcut | Works when |
|---|---|---|---|
| `dock.toggleAll` | Hide or Show All Docks | `Ctrl+Alt+H` | data file open |
| `dock.toggleLeft` | Fold or Open Left Dock |  | data file open |
| `dock.toggleRight` | Fold or Open Right Dock |  | data file open |
| `dock.toggleBottom` | Fold or Open Bottom Dock |  | data file open |
| `dock.cycleEditorTabs` | Move Editor Tabs |  | data file open |

## Organize commands (`organize.*`)

| ID | Name | Default shortcut | Works when |
|---|---|---|---|
| `organize.newFolder` | New Folder... |  | data file open |
| `organize.moveToFolder` | Move to Folder... |  | data file open |
| `organize.copyToFolder` | Add to Another Folder... |  | data file open |
| `organize.cutToFolder` | Cut |  | data file open |
| `organize.pasteIntoFolder` | Paste into Folder |  | data file open |
| `organize.addToCollection` | Add to Collection... |  | data file open |
| `organize.removeFromGroup` | Remove from Folder, Collection or Tag |  | data file open |
| `organize.colourLabel` | Set Colour... |  | data file open |
| `organize.bulkRename` | Rename with Pattern... |  | data file open |
| `organize.deadResources` | Find Dead Resources... |  | data file open |
| `organize.toggleFolderView` | Switch Folder View |  | data file open |
| `organize.selectByType` | Select All of This Type |  | data file open |
| `organize.selectContents` | Select Everything in the Group |  | data file open |
| `organize.moveToTextureGroup` | Move to Texture Group... |  | data file open |
| `organize.moveToAudioGroup` | Move to Audio Group... |  | data file open |
| `organize.openFolder` | Open Folder |  | data file open |
| `organize.folderUp` | Up One Folder |  | data file open |
| `organize.exportPackage` | Export as Package... |  | data file open |

## Code editor commands (`code.*`)

| ID | Name | Default shortcut | Works when |
|---|---|---|---|
| `code.goToDefinition` | Go to Definition | `F12` | code editor in front |
| `code.peekDefinition` | Peek Definition | `Alt+F12` | code editor in front |
| `code.parameterHints` | Parameter Hints | `Ctrl+Shift+Space` | code editor in front |
| `code.quickFix` | Quick Fix | `Ctrl+.` | code editor in front |
| `code.insertSnippet` | Insert Snippet | `Ctrl+J` | code editor in front |
| `code.toggleComment` | Toggle Line Comment | `Ctrl+/` | code editor in front |
| `code.blockComment` | Toggle Block Comment | `Shift+Alt+A` | code editor in front |
| `code.assetNumbers` | Replace Numbers with Asset Names |  | code editor in front |
| `code.indent` | Indent Selection |  | code editor in front |
| `code.outdent` | Outdent Selection |  | code editor in front |
| `code.duplicateLine` | Duplicate Line | `Shift+Alt+Down` | code editor in front |
| `code.moveLineUp` | Move Line Up | `Alt+Up` | code editor in front |
| `code.moveLineDown` | Move Line Down | `Alt+Down` | code editor in front |
| `code.toggleBookmark` | Toggle Bookmark | `Ctrl+F2` | code editor in front |
| `code.nextBookmark` | Next Bookmark | `Alt+F2` | code editor in front |
| `code.previousBookmark` | Previous Bookmark | `Alt+Shift+F2` | code editor in front |
| `code.nextProblem` | Next Problem | `F8` | code editor in front |
| `code.previousProblem` | Previous Problem | `Shift+F8` | code editor in front |
| `code.outline` | Go to Symbol in File | `Ctrl+Shift+O` | code editor in front |
| `code.toggleDiff` | Show Changes Against Original |  | code editor in front |
| `code.helpOnWord` | Help on Word Under Caret | `Ctrl+F1` | code editor in front |
| `code.openInManual` | Open in Manual |  | code editor in front |
| `code.goToLine` | Go to Line... | `Ctrl+G` | code editor in front |
| `code.goToBracket` | Go to Matching Bracket | `Ctrl+Shift+\` | code editor in front |
| `code.expandSelection` | Expand Selection | `Shift+Alt+Right` | code editor in front |
| `code.shrinkSelection` | Shrink Selection | `Shift+Alt+Left` | code editor in front |
| `code.deleteLine` | Delete Line | `Ctrl+Shift+K` | code editor in front |
| `code.joinLines` | Join Lines | `Ctrl+Alt+J` | code editor in front |
| `code.sortLines` | Sort Selected Lines |  | code editor in front |
| `code.trimWhitespace` | Trim Trailing Whitespace |  | code editor in front |
| `code.upperCase` | Convert to Upper Case |  | code editor in front |
| `code.lowerCase` | Convert to Lower Case |  | code editor in front |
| `code.titleCase` | Convert to Title Case |  | code editor in front |

## Image editor commands (`img.*`)

| ID | Name | Default shortcut | Works when |
|---|---|---|---|
| `img.tool.pencil` | Pencil | `D ; also B, .` | image editor in front |
| `img.tool.eraser` | Eraser | `E` | image editor in front |
| `img.tool.replace` | Replace Colour | `V` | image editor in front |
| `img.tool.remove` | Remove Colour | `H` | image editor in front |
| `img.tool.fill` | Fill | `F` | image editor in front |
| `img.tool.line` | Line | `L` | image editor in front |
| `img.tool.rectangle` | Rectangle | `R` | image editor in front |
| `img.tool.rectangleFilled` | Rectangle (Filled) | `Shift+R` | image editor in front |
| `img.tool.ellipse` | Ellipse | `C` | image editor in front |
| `img.tool.ellipseFilled` | Ellipse (Filled) | `Shift+C` | image editor in front |
| `img.tool.polygon` | Polygon | `P` | image editor in front |
| `img.tool.polygonFilled` | Polygon (Filled) | `Shift+P` | image editor in front |
| `img.tool.curve` | Curve | `A` | image editor in front |
| `img.tool.text` | Text | `T` | image editor in front |
| `img.tool.picker` | Colour Picker | `O ; also I` | image editor in front |
| `img.tool.select` | Rectangle Select | `S` | image editor in front |
| `img.tool.lasso` | Lasso Select | `Q` | image editor in front |
| `img.tool.wand` | Magic Wand | `W` | image editor in front |
| `img.tool.move` | Move Selection | `M` | image editor in front |
| `img.tool.hand` | Hand (Move View) |  | image editor in front |
| `img.brushBigger` | Bigger Brush | `+ ; also Num +` | image editor in front |
| `img.brushSmaller` | Smaller Brush | `- ; also Num -` | image editor in front |
| `img.swapColours` | Swap Primary and Secondary Colour | `X` | image editor in front |
| `img.smooth` | Smooth Drawing |  | image editor in front |
| `img.grid` | Grid | `G` | image editor in front |
| `img.symmetryH` | Mirror Left-Right |  | image editor in front |
| `img.symmetryV` | Mirror Top-Bottom |  | image editor in front |
| `img.tiles` | Tile Preview |  | image editor in front |
| `img.onion` | Onion Skin |  | image editor in front |
| `img.zoomIn` | Zoom In |  | image editor in front |
| `img.zoomOut` | Zoom Out |  | image editor in front |
| `img.zoomFit` | Fit | `Alt+F` | image editor in front |
| `img.zoomActual` | Actual Size (1:1) | `Alt+C` | image editor in front |
| `img.previousFrame` | Previous | `1` | image editor in front |
| `img.nextFrame` | Next | `2` | image editor in front |
| `img.duplicateFrame` | Duplicate Frame | `Ctrl+D` | image editor in front |
| `img.removeFrame` | Delete Frame |  | image editor in front |
| `img.play` | Play |  | image editor in front |
| `img.selectAll` | Select All | `Ctrl+Shift+A` | image editor in front |
| `img.selectNone` | Select None | `Escape` | image editor in front |
| `img.invertSelection` | Invert Selection | `Ctrl+I` | image editor in front |
| `img.expandSelection` | Expand or Shrink Selection... |  | image editor in front |
| `img.copy` | Copy | `Ctrl+C` | image editor in front |
| `img.cut` | Cut | `Ctrl+X` | image editor in front |
| `img.paste` | Paste | `Ctrl+V` | image editor in front |
| `img.clear` | Clear | `Delete` | image editor in front |
| `img.fillSelection` | Fill Selection |  | image editor in front |
| `img.strokeSelection` | Stroke Selection... |  | image editor in front |
| `img.flipH` | Flip Horizontally |  | image editor in front |
| `img.flipV` | Flip Vertically |  | image editor in front |
| `img.rotateCw` | Rotate 90 Degrees Clockwise |  | image editor in front |
| `img.rotateCcw` | Rotate 90 Degrees Anticlockwise |  | image editor in front |
| `img.rotate` | Rotate by Angle... |  | image editor in front |
| `img.scale` | Resize Image... |  | image editor in front |
| `img.skew` | Skew... |  | image editor in front |
| `img.adjust` | Hue, Saturation, Brightness, Contrast... |  | image editor in front |
| `img.invertColours` | Invert Colours |  | image editor in front |
| `img.posterize` | Posterize... |  | image editor in front |
| `img.greyscale` | Greyscale... |  | image editor in front |
| `img.dither` | Dither... |  | image editor in front |
| `img.outline` | Outline with Primary Colour |  | image editor in front |
| `img.blur` | Blur... |  | image editor in front |
| `img.resize` | Resize Canvas... |  | image editor in front |
| `img.crop` | Crop to Selection |  | image editor in front |
| `img.trim` | Trim to Content |  | image editor in front |
| `img.layerAdd` | New Layer | `Ctrl+Shift+N` | image editor in front |
| `img.layerDelete` | Delete Layer |  | image editor in front |
| `img.layerDuplicate` | Duplicate Layer |  | image editor in front |
| `img.layerUp` | Move Layer Up |  | image editor in front |
| `img.layerDown` | Move Layer Down |  | image editor in front |
| `img.layerMergeDown` | Merge Layer Down |  | image editor in front |
| `img.layerVisible` | Show or Hide Layer |  | image editor in front |
| `img.layerLock` | Lock or Unlock Layer |  | image editor in front |
| `img.layerRename` | Rename Layer |  | image editor in front |
| `img.discard` | Discard Image Edits |  | image editor in front |
| `img.goToOriginal` | Go to Original Resource |  | image editor in front |

## See also

- [Menus](menus.md)
- [Command palette and Go To](command-palette-and-go-to.md)
- [Main window](main-window.md)
- [Shortcuts](../settings/README.md)
