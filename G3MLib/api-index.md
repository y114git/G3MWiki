# API index

Every public type of G3MLib 1.1.5 with the page that explains it. Types that are plain data holders appear in groups. Namespaces start with `G3MLib.`.

## DataFile: reading and writing

| Type | Purpose | Page |
| --- | --- | --- |
| `DataFile.GameMakerIO` | Read and write data files, sync and async | [DATA files](data-files.md) |
| `DataFile.GameMakerReader`, `GameMakerWriter` | Low-level stream reader and writer behind `GameMakerIO` | [DATA files](data-files.md) |
| `DataFile.GameMakerProgress` | Fraction and chunk name for read and write progress | [DATA files](data-files.md#read-and-write-asynchronously) |
| `DataFile.LoadWarning` | One warning raised while reading | [Data integrity](data-integrity.md#read-warnings) |
| `DataFile.GameMakerData` | The loaded model: typed resource lists, `Gate`, `LoadWarnings`, `MayLoseDataOnSave`, version helpers | [DATA files](data-files.md) |
| `DataFile.GameMakerDataExtensionMethods`, `ListExtensions` | Helper extensions for lists and lookups | [DATA files](data-files.md) |
| `DataFile.ToolInfo` | Decompiler settings and instance id prefix held by `GameMakerData.ToolInfo` | [GML editing](gml.md) |
| `DataFile.GameMakerSerializationException` | Thrown by the reader and writer for malformed data | [DATA files](data-files.md) |
| `DataFile.ResourceType` | Enum of resource kinds | [DATA files](data-files.md) |
| `DataFile.GameMakerObject`, `GameMakerResource`, `GameMakerNamedResource`, `GameMakerResourceRef`, `GameMakerResourceById<T, ChunkT>`, `GameMakerObjectWithBlobs`, `PaddedObject`, `PrePaddedObject`, `ISearchable`, `IStaticChildObjCount`, `IStaticChildObjectsSize` | Base contracts of model objects | [DATA files](data-files.md#model-types) |
| `DataFile.GameMakerObservableList`, `GameMakerSimpleList`, `GameMakerSimpleListString`, `GameMakerSimpleListShort`, `GameMakerPointerList`, `GameMakerSimpleResourcesList`, `IVersionedList` | List types of the model | [DATA files](data-files.md#model-types) |
| `DataFile.GameMakerChunk` and the `GameMakerChunk<XXXX>` types (`FORM`, `GEN8`, `OPTN`, `LANG`, `EXTN`, `SOND`, `AGRP`, `SPRT`, `BGND`, `PATH`, `SCPT`, `GLOB`, `GMEN`, `SHDR`, `FONT`, `TMLN`, `OBJT`, `ROOM`, `UILR`, `DAFL`, `TPAG`, `CODE`, `VARI`, `FUNC`, `STRG`, `TXTR`, `AUDO`, `EMBI`, `TGIN`, `ACRV`, `SEQN`, `TAGS`, `FEDS`, `FEAT`, `PSEM`, `PSYS`, `2023List`), `GameMakerSingleChunk`, `GameMakerListChunk`, `GameMakerAlignUpdatedListChunk`, `GameMakerSimpleListChunk`, `GameMakerEmptyChunk`, `GameMakerUnsupportedChunk`, `GameMakerRawChunk`, `IGameMakerSingleChunk`, `IGameMakerListChunk`, `IGameMakerSimpleListChunk` | One type per chunk of the file format | [DATA files](data-files.md#model-types) |
| `DataFile.GameMakerDebugData`, `GameMakerDebugIO`, `GameMakerDebugFORM`, `GameMakerDebugChunkDBGI`, `GameMakerDebugChunkINST`, `GameMakerDebugChunkLOCL`, `GameMakerDebugChunkSCPT`, `GameMakerDebugChunkSTRG` | Debugger data model and its reader and writer | [GML editing](gml.md#debugger-data) |
| `DataFile.ModelsDebug.DebugDataGenerator`, `DebugDataMode` | Builds debugger data from a data file | [GML editing](gml.md#debugger-data) |
| `DataFile.DataAccessGate` | Reader/writer gate for one data file | [Cancellation and threading](cancellation-and-threading.md#data-access-gate) |
| `DataFile.ModelLock` | Host-side reader/writer lock | [Cancellation and threading](cancellation-and-threading.md#modellock) |
| `DataFile.DataLookup` | Hash indexes over strings and named resources | [References and renaming](references.md#lookups-and-repair) |
| `DataFile.DataRepair`, `VariableCounters` | Rebuild code locals and variable counters | [References and renaming](references.md#lookups-and-repair) |

## DataFile.Models

The typed object model. Every resource list of `GameMakerData` holds these types.

| Type group | Page |
| --- | --- |
| Sprites and sprite data: `GameMakerSprite`, `SpriteType`, `SepMaskType`, `TextureEntry`, `MaskEntry`, `NineSlice`, `TileMode`, `GameMakerSpineTextureEntry`, the `GameMakerYYSWF*` and `GameMakerVector*` types | [DATA files](data-files.md#model-types) |
| Textures: `GameMakerTexturePageItem`, `GameMakerEmbeddedTexture`, `TexData`, `GameMakerEmbeddedImage`, `GameMakerTextureGroupInfo`, `TextureGroupLoadType` | [Resource export and import](resources.md#sprite-import-and-texture-pages) |
| Audio: `GameMakerSound`, `AudioEntryFlags`, `AnimSpeedType`, `GameMakerAudioGroup`, `GameMakerEmbeddedAudio` | [Resource export and import](resources.md#audio-group-files) |
| Objects, rooms and scripts: `GameMakerGameObject`, `Event`, `EventAction`, `EventType`, the `EventSubtype*` enums, `GameMakerPhysicsVertex`, `CollisionShapeFlags`, `GameMakerRoom` and its `Background`, `View`, `GameObject`, `Tile`, `Layer`, `Layer*Data`, `*Instance` types, `GameMakerScript`, `GameMakerGlobalInit`, `GameMakerTimeline`, `GameMakerTimelineMoment` | [DATA files](data-files.md#model-types) |
| Other assets: `GameMakerBackground`, `TileID`, `GameMakerFont`, `Glyph`, `GlyphKerning`, `GameMakerPath`, `PathPoint`, `GameMakerShader`, `ShaderType`, `VertexShaderAttribute`, `GameMakerRawShaderData`, `GameMakerAnimationCurve`, `Channel`, `Point`, `CurveType`, `GameMakerSequence` and its keyframe and track types, `GameMakerParticleSystem`, `GameMakerParticleSystemEmitter` and their enums, `GameMakerExtension` and its `GameMakerExtension*` types, `GameMakerFilterEffect` | [DATA files](data-files.md#model-types) |
| Code: `GameMakerCode`, `GameMakerInstruction`, `Opcode`, `InstructionType`, `DataType`, `InstanceType`, `VariableType`, `ComparisonType`, `IReferencedObject`, `GameMakerInstructionUtil`, `GameMakerVariable`, `GameMakerFunction`, `GameMakerCodeLocals`, `LocalVar` | [GML editing](gml.md) |
| Strings and info: `GameMakerString`, `GameMakerGeneralInfo` (with `BranchSource`, `VersionSource`, `BranchType`, `RuntimeVersion`, `InfoFlags`, `FunctionClassification`), `GameMakerOptions`, `OptionsFlags`, `Constant`, `GameMakerLanguage`, `LanguageData`, `GameMakerTags`, `GameMakerFeatureFlags` | [DATA files](data-files.md#model-types) |
| UI layers: `GameMakerUINode`, `GameMakerUIRootNode`, `GameMakerUIContainer`, `GameMakerUILayer`, `GameMakerUIFlexPanel`, `GameMakerUIGameObject`, `GameMakerUISequenceInstance`, `GameMakerUISpriteInstance`, `GameMakerUITextItemInstance`, `GameMakerUIEffectLayer`, `GameMakerFlexValue` and the flex enums and interfaces | [DATA files](data-files.md#model-types) |

## DataFile: references, projects, scripts

| Type | Purpose | Page |
| --- | --- | --- |
| `DataFile.References.ReferenceIndex`, `Reference`, `StringUse`, `UnreferencedKind`, `TileUse`, `CodeScan`, `CodeScanResult`, `AudioIndex` | Find usages and unreferenced items | [References and renaming](references.md) |
| `DataFile.References.ClearedReferences` | Result of `ClearUsages` with `Undo` | [References and renaming](references.md#clear-usages-before-a-delete) |
| `DataFile.References.RenameService`, `RenameRequest`, `RenameKind`, `RenamePlan`, `RenameConflict`, `RenameChange`, `RenameChangeSet`, `RenameFailure`, `RenameException` | Rename resources, variables and functions | [References and renaming](references.md#rename) |
| `DataFile.References.ResourceCloneService`, `ResourceClone` | Copy a resource in memory | [References and renaming](references.md#clone-a-resource) |
| `DataFile.Project.G3MProjectService`, `G3MProjectOptions`, `G3MProjectResult`, `G3MProject`, `G3MProjectResource`, `G3MProjectBaseFile` | The `.g3mproj` project format | [Project format](project-format.md) |
| `DataFile.Project.ProjectExportMode` | Full or delta project | [Project format](project-format.md#export-and-update) |
| `DataFile.Project.ProjectContext`, `PathPair`, `ProjectException`, `GameFileBackupException`, `IGameFileBackup`, `GameFileBackup`, `GameFileNoOpBackup` | Scripted resource project and backups | [Projects and scripts](projects.md) |
| `DataFile.Project.IProjectAsset`, `ISerializableProjectAsset`, `ISerializableTextureProjectAsset`, `SerializableAssetType`, `SerializableAssetTypeExtensions`, `SerializableProjectAssetBridge` and the `Project.SerializableAssets` types (`SerializableRoom`, `SerializableSprite`, `SerializableSequence`, and the rest) | Serialized assets of a `ProjectContext` project | [Projects and scripts](projects.md#scripted-resource-project-projectcontext) |
| `DataFile.Scripting.ScriptRunner`, `ScriptRunResult` | Compile and run `.csx` scripts | [Scripts and ScriptGlobals](script-globals.md) |
| `DataFile.Scripting.ScriptGlobals`, `IScriptInterface`, `ScriptException`, `ScriptCancelledException` | What scripts can call | [Scripts and ScriptGlobals](script-globals.md#scriptglobals-members) |
| `DataFile.Legacy.UTMTScriptTranslator` | Translates UndertaleModTool scripts before compiling | [Scripts and ScriptGlobals](script-globals.md) |
| `DataFile.Interop.YyFile`, `YyFormat`, `YySprite`, `YySound`, `YyScript`, `YyObject`, `YyRoom` | GameMaker `.yy` readers and writers | [Resource export and import](resources.md#gamemaker-project-files) |

## DataFile.Util

| Type | Purpose | Page |
| --- | --- | --- |
| `TexturePageRepacker`, `RepackOptions`, `RepackPlan`, `RepackPagePlan`, `PreparedFrame` | Add or replace frames, repack pages | [Resource export and import](resources.md#sprite-import-and-texture-pages) |
| `PageEditor`, `PreparedTextureReplace`, `TextureWorker`, `TextureGroupPacker`, `BorderFlags`, `MaskBuilder`, `GMImage`, `ImageFormat`, `QoiConverter` | Texture page and image helpers | [Resource export and import](resources.md#sprite-import-and-texture-pages) |
| `BPS` | BPS patch apply | [Projects and scripts](projects.md) |
| `AdaptiveBinaryReader`, `IBinaryReader`, `BufferBinaryReader`, `BufferBinaryWriter`, `FileBinaryReader`, `FileBinaryWriter`, `StringBuilderHelper` | Binary I/O helpers behind the reader and writer | [DATA files](data-files.md) |
| `AssetReferenceTypes`, `RefType`, `Paths`, `DebugUtil`, `GitVersion`, `ScriptingUtil` | Internal-facing helpers that are public | [DATA files](data-files.md) |

## DataFile.Compiler and DataFile.Decompiler

| Type | Purpose | Page |
| --- | --- | --- |
| `DataFile.Compiler.CodeImportGroup`, `CodeImportMode`, `CodeImportResult`, `CodeOperationResult`, `CodeOperationStatus` | Queue and run code edits | [GML editing](gml.md#queue-code-changes) |
| `DataFile.Compiler.CompileGroup`, `CompileResult`, `CompileError` | Replace-only compile, macros | [GML editing](gml.md#macros) |
| `DataFile.Compiler.BuiltinList`, `FunctionInfo`, `VariableInfo` | Built-in function and variable tables | [GML editing](gml.md) |
| `DataFile.Decompiler.GlobalDecompileContext`, `GameSpecificResolver`, `Assembler`, `Disassembler`, `DisassemblyLine` | Decompile and disassemble | [GML editing](gml.md#decompile-a-code-entry) |

## Analyzer

| Type | Purpose | Page |
| --- | --- | --- |
| `Analyzer.Compiler.CompileContext`, `CompileScriptKind`, `CompilerException`, `FunctionScope` | One compile of one code entry, with `ExternalMacros` | [GML editing](gml.md#macros) |
| `Analyzer.Compiler.GmlTokenizer`, `GmlToken`, `GmlTokenKind` | Tokenizer for editors | [GML editing](gml.md#diagnostics-and-tokens) |
| `Analyzer.Compiler.GmlDiagnostic`, `DiagnosticSeverity`, `SourceSpan`, `LineMap` | Structured compiler diagnostics | [GML editing](gml.md#diagnostics-and-tokens) |
| `Analyzer.Compiler.IBuiltins`, `IBuiltinFunction`, `IBuiltinVariable`, `ICodeBuilder`, `Errors.ICompileError`, `Errors.LexerError`, `Errors.ParserError` | Compiler extension points and error types | [GML editing](gml.md) |
| `Analyzer.Compiler.Nodes.*`, `Lexer.*`, `Parser.*`, `Bytecode.*` | Parse tree, lexer, parser and bytecode generator internals | [GML editing](gml.md) |
| `Analyzer.Decompiler.DecompileContext`, `DecompileSettings`, `IDecompileSettings`, `IDecompileWarning`, `DecompileDataLeftoverWarning`, `DecompilerException`, `GlobalFunctions`, `IGlobalFunctions` | Decompiler entry points and settings | [GML editing](gml.md#decompile-a-code-entry) |
| `Analyzer.Decompiler.DecompiledSource`, `SourceMap`, `SourceMapEntry` | Decompiled text with a source map | [GML editing](gml.md#decompile-a-code-entry) |
| `Analyzer.Decompiler.AST.*`, `ControlFlow.*`, `GameSpecific.*` | Decompiler syntax tree, control flow graph and per-game rules | [GML editing](gml.md) |
| `Analyzer.AssetType`, `GMEnum`, `GMEnumValue`, `IGMCode`, `IGMFunction`, `IGMInstruction`, `IGMString`, `IGMVariable`, `IGameContext`, `Analyzer.Mock.*` | Game context contracts and a mock for tests | [GML editing](gml.md) |

## Modding

| Type | Purpose | Page |
| --- | --- | --- |
| `Modding.Patching.PatchService` | Create, validate and apply patches | [Patch API](patches.md) |
| `Modding.Patching.PatchCreateResult`, `PatchValidateResult`, `PatchApplyResult`, `PatchApplyOptions`, `PatchStatistics` | Patch results and options | [Patch API](patches.md) |
| `Modding.Patching.G3MPatchManifest`, `DataFileInfo`, `GeneralInfoData`, `ToolInfo`, `ResourceTypeChanges`, `ResourceChange`, `PatchApplyPlan` | Contents of `g3mpatch.json` | [Patch API](patches.md#manifest) |
| `Modding.Patching.PatchFileSystem` | Reads and writes a patch archive | [Patch API](patches.md#read-a-patch-archive) |
| `Modding.Patching.PatchInputService` | Write data files, copy audio groups, normalize inputs | [Patch API](patches.md#normalize-another-input) |
| `Modding.Merging.MergeService`, `MergeOptions`, `MergeResult` | Merge patches | [Merge and diff](merge-and-diff.md) |
| `Modding.Diffing.DiffService`, `DiffReportMode`, `DiffResult`, `DiffTypeSummary`, `DiffResourceTypeChanges`, `ResourceDiffSummary`, `TextFileDiff` | Compare files and write reports | [Merge and diff](merge-and-diff.md#compare) |
| `Modding.Resources.ResourceExportService` | Export every resource type | [Resource export and import](resources.md) |
| `Modding.Resources.ResourceImportService` | Import every resource type | [Resource export and import](resources.md#import-a-type) |
| `Modding.Resources.ItemResult`, `ResultKind`, `BatchProgress` | Batch results and progress | [Resource export and import](resources.md#batch-export-with-progress-and-cancellation) |
| `Modding.Resources.FrameExporter`, `FrameJob` | PNG frame export | [Resource export and import](resources.md#batch-export-with-progress-and-cancellation) |
| `Modding.Resources.AudioGroupFile`, `AudioGroupReader`, `AudioSource`, `SoundIdRemap` | Audio group files and audio lookup | [Resource export and import](resources.md#audio-group-files) |
| `Modding.Resources.ResourceHashService` | Hash every resource | [Data integrity](data-integrity.md#hashing) |
| `Modding.Integrity.DataIntegrityService`, `DataIntegrityResult`, `IntegrityIssue`, `IntegritySeverity` | Validate and repair | [Data integrity](data-integrity.md#integrity-check) |
| `Modding.Hashing.ChunkDigests`, `ChunkDigest`, `FileHash`, `HashService` | Chunk and file hashes | [Data integrity](data-integrity.md#hashing) |
| `Modding.Logging.LogService`, `ModdingLogMessage`, `ModdingLogLevel`, `ModdingProgress` | Log and progress routing | [Host integration](hosting.md#logs-and-progress) |
| `Modding.Caching.G3MCacheOptions` | Analysis cache settings | [Host integration](hosting.md#analysis-cache) |
| `Modding.Caching.G3MCacheService`, `G3MCacheManifest`, `G3MDataAnalysisCache`, `G3MDataInfoSnapshot` | Cache reading, writing and contents | [Host integration](hosting.md#analysis-cache) |
| `Modding.XDelta.XDeltaService`, `XDeltaResult` | Run the xdelta executable | [Host integration](hosting.md#xdelta-executable) |
| `Modding.ModdingMetadata` | Tool name and version written into patch manifests | [Host integration](hosting.md#tool-name-and-version) |
| `Modding.Internal.ResourceTypeRegistry`, `DataFileExtensionUtil`, `GeneralInfoUtil` | Resource type names, import order, file helpers | [Resource export and import](resources.md#resource-type-names) |

## See also

- [Getting started](getting-started.md)
- [Limits and defaults](limits-and-defaults.md)
