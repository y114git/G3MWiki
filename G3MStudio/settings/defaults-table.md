# Default values

This page lists every setting of `settings.json` with its default and its allowed range. Keys are camelCase. Values outside a range are clamped when the file loads. See [Settings file](settings-file.md) for the format.

## Interface

| Key | Label | Default | Allowed |
| --- | --- | --- | --- |
| `language` | **Language** | `en` | installed language codes |
| `uiScale` | **Interface scale** | 0 (**Auto**) | 0, or 50 to 200 |
| `density` | **Density** | `Comfortable` | `Compact`, `Comfortable`, `Spacious` |
| `fontFamily` | **Font family** | empty (theme font) | any font name |
| `fontWeight` | **Font weight** | empty (theme weight, Bold) | `Normal`, `Medium`, `SemiBold`, `Bold` |
| `sizes` | **Shape and size** | empty (density preset) | see [Appearance settings](appearance.md#density-and-shape-and-size) |
| `animations` | **Animations** | empty (system default) | `Full`, `Medium`, `Minimal`, `Off` |
| `tabButtons` | **Previous and next tab buttons** | `MenuLeft` | `MenuLeft`, `MenuRight`, `Hidden` |
| `editorTabs` | **Editor tabs** | `Top` | `Top`, `Left`, `Right` |
| `editorTabListWidth` | **Width of the tab list** | `170` | 80 to 400 |
| `editorTabIcons` | **Show a picture in editor tabs** | Off | On, off |
| `helpLevel` | **Explanatory text** | `All` | `All`, `Hints`, `Details`, `Off` |
| `hoverPreviews` | **Show previews and IDs on hover** | true | true, false |
| `showNullEntries` | **Show null entries in the resource tree** | false | true, false |
| `colors` | **Colors** | empty | keys of [Colors and themes](colors-and-themes.md), `#RRGGBB` |
| `overrides` | advanced theme tokens | empty | token name to text |

## Explorer

| Key | Default | Allowed |
| --- | --- | --- |
| `explorerFolderView` | false | true, false |
| `explorerShowCounts` | true | true, false |
| `explorerFoldersOpen` | true | true, false |
| `explorerHideUndecompilable` | false | true, false |
| `explorerHideBroken` | false | true, false |
| `explorerOpenDataOnSearch` | false | true, false |
| `explorerHideData` | false | true, false |
| `explorerAnonymousFunctions` | `Dim` | `Show`, `Dim`, `Hide` |
| `explorerStructConstructors` | `Dim` | `Show`, `Dim`, `Hide` |
| `explorerInnerFunctions` | `Dim` | `Show`, `Dim`, `Hide` |
| `explorerSortOrder` | `ListOrder` | `ListOrder`, `NameAscending`, `NameDescending` |
| `spritePreviewFirstVisible` | true | true, false |
| `explorerThumbnailSize` | 22 | 16 to 96 |
| `explorerZoom` | 100 | 50 to 250 |
| `spriteZoom` | 0 (not set) | 0 to 32 |
| `explorerOptionsFolded` | false | true, false |

## Code editor

| Key | Default | Allowed |
| --- | --- | --- |
| `codeFontFamily` | empty (theme code font) | any font name |
| `codeFontSize` | 13 | 8 to 32 |
| `codeWordWrap` | false | true, false |
| `codeLineNumbers` | true | true, false |
| `codeAutoComplete` | true | true, false |
| `codeIndentGuides` | true | true, false |
| `codeBracketColors` | true | true, false |
| `codeLigatures` | false | true, false |
| `codeLiveCheck` | true | true, false |
| `newScriptGlobal` | true | true, false |
| `codeAssetNumbers` | `Manual` | `Off`, `Manual`, `Auto` |
| `codeAssetNumbersGameFunctions` | true | true, false |
| `codeAssetNumberTypes` | all ten types | `sprites`, `objects`, `rooms`, `sounds`, `paths`, `fonts`, `shaders`, `scripts`, `tileSets`, `particleSystems` |
| `codeTabWidth` | 4 | 2 to 8 |
| `codeHoverDelayMs` | 400 | 300 to 3000 |
| `searchLive` | true | true, false |
| `searchContextLines` | 1 | 0 to 3 |
| `searchMaxResults` | 5000 | 500 to 100000 |
| `decompiler` | empty (library defaults) | see [Decompiler settings](decompiler.md) |

## Image, sound, comparison, rooms

| Key | Default | Allowed |
| --- | --- | --- |
| `image.brushSize` | 1 | 1 to 256 |
| `image.brushRound` | false | true, false |
| `image.smooth` | false | true, false |
| `image.tolerance` | 0 | 0 to 255 |
| `image.contiguous` | true | true, false |
| `image.textSize` | 16 | 4 to 256 |
| `image.textFont` | empty | font name |
| `image.applyImmediately` | false | true, false |
| `image.layerAutosave` | true | true, false |
| `image.showGrid` | true | true, false |
| `image.gridCell` | 16 | 0 to 1024 |
| `image.gridColour`, `checkerLight`, `checkerDark`, `canvasBackground` | empty (theme) | `#RRGGBB` or `#RRGGBBAA` |
| `image.onionFrames` | 1 | 1 to 8 |
| `image.onionOpacity` | 35 | 5 to 100 |
| `image.handleSize` | 3 | 1 to 12 |
| `audio.backend` | `openal` | `openal`, `system`, `off` |
| `audio.device` | empty (system default) | device name |
| `audio.volume` | 100 | 0 to 100 |
| `audio.bufferMs` | 100 | 20 to 500 |
| `audio.captureDevice` | empty | device name |
| `audio.visualizer` | true | true, false |
| `audio.waveformHeight` | 0 | 0, or 40 to 800 |
| `diff.ignoreWhitespace` | false | true, false |
| `diff.unified` | false | true, false |
| `diff.contextLines` | 3 | 0 to 50 |
| `diff.pictureMode` | `overlay` | `overlay`, `swipe`, `difference` |
| `roomGridOverride` | false | true, false |
| `roomGridWidth`, `roomGridHeight` | 16, 16 | 1 to 1024 |
| `autoTile.outsideIsTerrain` | false | true, false |
| `autoTile.startOn` | false | true, false |
| `autoTile.defaultLayout` | `Blob47` | 0 to 2 in the box (47 tiles, 16 tiles, 256 masks) |

The other `image` keys are the remembered values of the Resize and Import Sheet dialogs. See [Image editor settings](image.md).

## Files, startup and projects

| Key | Default | Allowed |
| --- | --- | --- |
| `backupCount` | 3 | 1 to 100 |
| `askOnClose` | true | true, false |
| `rememberWindow` | true | true, false |
| `startup` | `Restore` | `Restore`, `Home`, `Ask` |
| `lostFileSearch` | true | true, false |
| `lostFileAutoUse` | true | true, false |
| `hashMismatch` | `Ask` | `Ask`, `KeepLayout`, `FreshLayout`, `Refuse` |
| `lostFileForgetOld` | true | true, false |
| `project.autoSave` | `AfterDelay` | `Off`, `AfterDelay`, `OnFocusLoss`, `OnSave` |
| `project.autoSaveSeconds` | 2 | 1 to 3600 |
| `project.autoSync` | `OnSave` | `Off`, `AfterDelay`, `OnFocusLoss`, `OnSave` |
| `project.autoSyncSeconds` | 30 | 1 to 3600 |
| `modBaseEnabled` | true | true, false |
| `modBaseMaxMb` | 2048 | 64 to 1048576 |

## Run

| Key | Default | Allowed |
| --- | --- | --- |
| `runRunner`, `runArguments`, `runWorkingDir`, `runLinks`, `runEnvironment`, `runtimeFolders` | empty | text |
| `runStopSeconds` | 5 | 1 to 60 |
| `runHandover` | `Wait` | `Wait`, `DoNotWait` |
| `runDebugOutput` | false | true, false |
| `runSaveInPlace` | false | true, false |
| `runAutoRestart` | false | true, false |
| `warnTempRun` | true | true, false |
| `runCompat` | `Auto` | `Auto`, `Native`, `Wine`, `Proton`, `Umu`, `PortProton`, `Custom` |
| `runCompatProgram`, `runWinePrefix`, `runCompatCommand` | empty | text |
| `runOptionsLocation` | `StudioFolder` | `StudioFolder`, `BesideDataFile` |
| `runProfiles`, `activeRunProfile`, `fileRunProfiles`, `trustedRunFiles` | empty | see [Run settings](run.md) |

## Advanced, privacy, prompts, plugins

| Key | Default | Allowed |
| --- | --- | --- |
| `askRiskyActions` | true | true, false |
| `allowUnsafeSilently` | false | true, false |
| `memoryLimitMb` | 0 (no limit) | 0, or 256 to 1048576 |
| `undoLimitMb` | 128 | 16 to 1048576 |
| `cacheLimitMb` | 128 | 16 to 65536 |
| `pluginsEnabled` | true | true, false |
| `pluginsHotReload` | true | true, false |
| `crashReports` | true | true, false |
| `usageStats` | true | true, false |
| `installId` | random, made at first start | text |
| `prompts` | empty (every prompt asks) | prompt id to `Yes` or `No` |
| `shortcuts` | empty (command defaults) | command id to gesture |
| `shortcutAliases` | empty (command defaults) | command id to list of gestures |

## See also

- [Settings file](settings-file.md)
- [Settings page](settings-overview.md)
- [Privacy and crash reports](privacy.md)
