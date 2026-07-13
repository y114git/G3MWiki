# DELTARUNE Evidence-First Wiki Design

## Objective

Update `DELTARUNE` into an English-language, evidence-first technical wiki that
is sufficient to understand and modify every meaningful shipped DELTARUNE system
across Chapters 1–5 without reopening the compiled game data for ordinary
questions.

The wiki must explain active gameplay and content as well as legacy, disabled,
test, debug, duplicate, and apparently unused content. It must distinguish
verified facts from uncertain interpretation and avoid undocumented assumptions.

## Verified Source Baseline

The primary source is the locally installed Windows Steam build:

- Steam application: `1671210`
- build: `24122389`
- Chapter 1: `chapter1_windows/data.win`
- Chapter 2: `chapter2_windows/data.win`
- Chapter 3: `chapter3_windows/data.win`
- Chapter 4: `chapter4_windows/data.win`
- Chapter 5: `chapter5_windows/data.win`
- launcher: root `data.win`
- Chapter 5 SHA-256:
  `81F86FB98BDD9F3DE029043ADBB453F276A9FB7E26A4DB1F3EE0AE3675E81C5A`

Official sources may establish release and platform facts. Claims about runtime
behavior must be supported primarily by shipped data, decompiled GML, object
events, rooms, resource metadata, or observable execution.

## Evidence Model

Every documented behavior should identify, where useful:

- owning payload or chapter range;
- relevant scripts, code entries, objects, events, rooms, and resources;
- initialization, update, rendering, persistence, and cleanup flow;
- inputs, global state, flags, arrays, and outputs;
- differences from earlier and later chapters;
- confidence and content status.

Content status uses the following vocabulary:

- `active`: reachable in normal shipped gameplay.
- `conditional`: reachable only under a route, flag, language, platform, or
  special state.
- `legacy`: retained from an earlier implementation but superseded in the
  current payload.
- `unused`: present in shipped data with no verified reachable runtime path.
- `debug/test`: intended for development, testing, diagnostics, or internal
  navigation.
- `removed-reference`: a surviving reference to content whose required
  implementation or resource is absent.
- `uncertain`: evidence is insufficient to classify safely.

`unused` must not be assigned merely because a resource name was not found in
decompiled top-level GML. Dynamic lookup, object inheritance, room placement,
timelines, localization, extension code, and resource IDs must also be
considered.

## Information Architecture

The existing topic-oriented structure remains the primary reading path. Pages
are expanded or added only where a coherent domain requires its own reference.
The wiki must not create one page per script, object, variable, room, or
resource.

Core coverage domains are:

1. Runtime architecture, launcher, chapter boundaries, startup, shutdown, and
   platform behavior.
2. Save formats, configuration, chapter transfer, flags, routes, progression,
   and persistent consequences.
3. Overworld movement, collision, rooms, doors, encounters, interactables,
   cameras, layers, and transitions.
4. Cutscenes, commands, timelines, dialogue, speakers, text effects, choices,
   localization, and video playback.
5. Battles, turn flow, ACTs, mercy, recruits, enemies, attacks, bullets, SOUL
   modes, damage, defense, TP, status effects, victory, defeat, and game over.
6. Characters, party membership, stats, growth, equipment, items, key items,
   spells, fusion, shops, storage, and inventories.
7. Chapter-specific systems such as board and rhythm gameplay, plus all Chapter
   5 systems discovered from the installed payload.
8. Rendering, sprites, animation, particles, shaders, surfaces, lighting, audio,
   music, sound routing, fonts, input, and accessibility-related behavior.
9. Debug, test, legacy, unused, duplicate, deprecated, and partially removed
   content.
10. Practical modding workflows with concrete examples, dependencies,
    chapter-specific caveats, validation steps, and failure modes.

Large catalog pages may enumerate all rooms, items, encounters, enemies,
characters, major events, routes, and other meaningful content. Machine-level
resource inventories remain supporting evidence rather than the main prose.

## Chapter Comparison Rules

Each payload is analyzed independently. Documentation groups identical behavior
only after comparison:

- `Ch1` for Chapter 1-only behavior;
- `Ch2–Ch4` when Chapter 2 introduced behavior retained unchanged through
  Chapter 4;
- separate chapter sections whenever implementation or data changes;
- `Ch5` for new or changed Chapter 5 behavior;
- `Launcher` for the separate chapter-select runtime.

“Same as another chapter” requires structural or code evidence. Similar names
alone are insufficient. Tables should show additions, removals, renamed
resources, changed constants, array/layout changes, and semantic changes.

## Platform Coverage

The installed Windows build is the fully verified baseline. Windows-specific
behavior includes executable architecture, filesystem layout, path handling,
helper DLLs, video files, launch arguments, input branches, save locations, and
graphics/audio backends visible in the payload.

macOS, Nintendo Switch, PlayStation, and other platform differences are
documented only when supported by one of these sources:

- locally available platform payloads;
- explicit platform branches or platform resource variants in shipped code;
- official documentation or release notes;
- reproducible observation with the platform build.

Unsupported assumptions are omitted or labeled `uncertain`; Windows behavior
must not be presented as universal.

## Extraction and Temporary Workspace

All temporary and generated work stays outside `DELTARUNE`. This includes
decompiled GML, assembly, rewritten round-trip payloads, raw
object/room/resource JSON, extracted media, logs, indexes, diffs, caches,
screenshots, and reports.

The working directory will be a dedicated path outside the repository unless an
existing ignored workspace is discovered. It will contain one immutable source
manifest and separate export roots per payload. Generated evidence must never be
linked as a required wiki dependency.

G3MTool and UndertaleModLib exporters will collect:

- code entries and decompiled GML;
- objects and event mappings;
- rooms, layers, instances, tiles, and placements;
- asset order and general metadata;
- sprites, backgrounds, fonts, paths, timelines, tilesets, and textures where
  needed;
- shaders and platform variants;
- sounds, audio groups, and external audio metadata;
- localization files, video metadata, and shipped filesystem resources.

Verbose logs are retained and scanned for partial failures. A successful
exporter exit code is not treated as proof of complete extraction.

## Analysis Workflow

1. Record hashes, sizes, timestamps, asset counts, GameMaker metadata, and
   filesystem manifests for all six payloads.
2. Export all evidence into isolated per-payload directories.
3. Validate extraction completeness and record failures.
4. Build cross-chapter symbol/resource mappings and structural diffs.
5. Trace runtime roots: startup objects, persistent controllers, room creation
   code, save/load entry points, battle controllers, cutscene dispatchers, and
   chapter transfer.
6. Inventory meaningful content and classify reachability/status conservatively.
7. Audit existing pages claim by claim against the current build.
8. Expand canonical topic pages and add only domain-level pages needed for clear
   separation.
9. Add concrete modding examples based on actual call patterns and dependencies.
10. Cross-check every chapter comparison and all numeric tables against
    extracted evidence.

## Modding Examples

Examples must be implementation-oriented rather than generic suggestions. Each
example should state:

- target chapters and payload;
- scripts, objects, rooms, and resources involved;
- required initialization and state;
- code or data change;
- dependent tables, flags, localization, save compatibility, and cleanup;
- how to test the change;
- common breakage such as invalid resource IDs, missing array expansion, wrong
  event ordering, stale save layout, or chapter-specific divergence.

Representative examples include adding an item, spell, enemy, attack pattern,
ACT, encounter, room transition, NPC dialogue branch, cutscene command sequence,
route flag, shop entry, party stat change, localized string, sound,
shader-backed effect, and chapter-select metadata behavior.

Examples must avoid distributing proprietary game assets or large verbatim
decompiled source listings. Short excerpts and pseudocode are used only to
explain behavior.

## Quality Controls

Completion requires:

- no unsupported claim presented as fact;
- all five chapters and the launcher explicitly covered;
- all meaningful rooms, items, equipment, spells, enemies, encounters,
  characters, routes, major events, and systems inventoried;
- active versus legacy/unused/debug status identified where evidence permits;
- chapter differences verified from payloads rather than inferred from
  chronology;
- platform scope stated precisely;
- internal links valid and indexes complete;
- terminology and runtime identifiers consistent across pages;
- duplicated explanations replaced with canonical sections and cross-links where
  this reduces drift;
- every modding example checked against the chapter payloads it claims to
  support;
- every Markdown file checked with the repository's `markdownlint`
  configuration, with every reported warning and error corrected at its source;
- no `markdownlint` rule disabling, inline suppression, exclusion, ignore file,
  or configuration weakening introduced to hide violations;
- lint-driven formatting changes verified not to break tables, heading anchors,
  fenced code blocks, or relative links;
- a final clean-worktree review that preserves pre-existing user changes and
  creates no commits.

## Repository Constraints

- Wiki prose is English only.
- Do not commit changes.
- Preserve the existing user modification in `DELTARUNE/chapter-select.md`.
- Do not create many tiny pages.
- Do not place raw exports or temporary artifacts in `DELTARUNE`.
- Prefer clear, evidence-backed explanations over exhaustive but context-free
  symbol dumps.
- Never ignore or suppress a `markdownlint` diagnostic; fix the Markdown that
  caused it.
- Keep audit methodology, extraction narration, verifier details, source hashes,
  and descriptions of how conclusions were discovered out of `DELTARUNE` pages.
  The wiki states the verified runtime behavior directly; methodology belongs
  only in the external evidence workspace and internal reports.
