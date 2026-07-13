# DELTARUNE Evidence Wiki Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use
> superpowers:subagent-driven-development or superpowers:executing-plans to
> implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for
> tracking.

**Goal:** Produce an English, evidence-first technical wiki covering every
meaningful player-facing system and content category in the installed Steam
build of DELTARUNE Chapters 1–5 and its launcher.

**Architecture:** Extract each GameMaker payload into an external temporary
workspace, derive comparable manifests and runtime traces, and use them to audit
and rewrite topic-oriented Markdown. Keep raw exports out of the wiki; publish
only clear explanations, useful catalogs, chapter differences, status
classifications, and verified modding examples.

**Tech Stack:** PowerShell, G3MToolCLI, UndertaleModLib, Underanalyzer,
Markdown, `markdownlint-cli2`, Git read-only inspection.

## Global Constraints

- Wiki prose is English only.
- Do not create commits.
- Preserve the user's existing edit to `DELTARUNE/chapter-select.md`.
- Keep exports, caches, logs, and rewritten payloads outside the repository.
- Do not create one page per script, object, room, or variable.
- Document active, conditional, legacy, unused, debug/test, removed-reference,
  and uncertain content without overstating reachability.
- Correct every `markdownlint` diagnostic at its source. Never suppress,
  disable, exclude, or ignore a rule or file.
- Treat the installed Windows Steam build `24122389` as the primary runtime
  source; label other platform claims by their actual evidence.
- `DELTARUNE` pages contain runtime facts, relationships, chapter differences,
  and modding guidance only. Audit narration, extraction methods, verifier
  implementation, source hashes, and discovery process stay in external evidence
  or internal reports.

---

### Task 1: Freeze the Runtime Evidence Baseline

**Files:**

- Modify: `DELTARUNE/README.md`
- Modify: `DELTARUNE/chapter-differences.md`
- Create externally: payload manifest and extraction logs

**Interfaces:**

- Consumes: six installed GameMaker payloads and the Steam manifest.
- Produces: immutable hashes, sizes, timestamps, versions, asset counts, and
  extraction roots used by every later task.

- [ ] Record SHA-256, byte size, timestamp, relative path, and GameMaker
      metadata for Chapters 1–5 and the launcher.
- [ ] Record the Steam app, build, depot, and manifest identifiers.
- [ ] Create a dedicated temporary workspace outside the repository with one
      subdirectory per payload and a logs directory.
- [ ] Export general information and asset order for all payloads.
- [ ] Verify each exporter log for partial failures and record any limitations.
- [ ] Replace stale target counts and four-chapter statements in the wiki.
- [ ] Run `markdownlint-cli2` on every modified Markdown file and fix all
      diagnostics.

### Task 2: Export Comparable Runtime Data

**Files:**

- Create externally: code, object, room, shader, sound, sprite, font,
  background, path, timeline, tileset, and texture metadata exports

**Interfaces:**

- Consumes: Task 1 payload manifest and extraction roots.
- Produces: validated per-payload resource trees and decompiled code searchable
  by later tasks.

- [ ] Export code entries with GML and assembly fallbacks for each payload.
- [ ] Export objects with event/action mappings and inheritance metadata.
- [ ] Export rooms with creation code, instances, layers, views, and tiles.
- [ ] Export shaders and all shipped platform variants.
- [ ] Export sound and audio-group metadata while preserving external audio
      group resolution.
- [ ] Export visual resource metadata needed to trace animation and rendering.
- [ ] Capture verbose logs and search them for decompiler failures, warnings,
      missing resources, and serialization errors.
- [ ] Re-run failed or incomplete exports and explicitly quarantine unresolved
      failures from factual claims.

### Task 3: Build Cross-Chapter Inventories

**Files:**

- Create externally: normalized manifests, symbol maps, resource diffs, and
  reachability notes
- Modify: `DELTARUNE/chapter-differences.md`
- Modify: `DELTARUNE/global-variables.md`

**Interfaces:**

- Consumes: validated exports from Task 2.
- Produces: chapter ranges, additions/removals, changed constants, renamed
  resources, and status classifications.

- [ ] Normalize scripts, code entries, objects, rooms, sprites, sounds, shaders,
      fonts, paths, timelines, items, spells, enemies, and flags.
- [ ] Compare resource presence and code hashes across every adjacent chapter.
- [ ] Trace dynamic resource lookup, inheritance, room placement, initialization
      roots, persistent controllers, and save/load paths before marking content
      unused.
- [ ] Classify content conservatively using the design status vocabulary.
- [ ] Document data-layout changes and global-variable ownership by chapter.
- [ ] Verify every comparison table directly against the normalized manifests.
- [ ] Run full lint on modified files and fix every diagnostic.

### Task 4: Audit and Rewrite Runtime Architecture

**Files:**

- Modify: `DELTARUNE/README.md`
- Modify: `DELTARUNE/chapter-select.md`
- Modify: `DELTARUNE/save-load-system.md`
- Modify: `DELTARUNE/localization.md`
- Create if required: `DELTARUNE/runtime-architecture.md`
- Create if required: `DELTARUNE/platform-differences.md`

**Interfaces:**

- Consumes: startup, configuration, launcher, localization, save, and platform
  traces from Tasks 2–3.
- Produces: canonical architecture and platform references used by all system
  pages.

- [ ] Trace startup through launcher handoff and chapter initialization.
- [ ] Document save naming, layouts, transfer rules, configuration files, slot
      metadata, corruption handling, and compatibility differences.
- [ ] Document language selection, string lookup, fallback, fonts, videos, and
      chapter-specific localization behavior.
- [ ] Document Windows paths, x64 helpers, launch arguments, filesystem and
      backend assumptions.
- [ ] Add only verified macOS/console branches, clearly labeling evidence and
      uncertainty.
- [ ] Preserve the user's existing formatting edit in `chapter-select.md`.
- [ ] Validate links and run full Markdown lint.

### Task 5: Rewrite Overworld, Rooms, Events, and Cutscenes

**Files:**

- Modify: `DELTARUNE/overworld.md`
- Modify: `DELTARUNE/cutscene-system.md`
- Modify: `DELTARUNE/dialogue-system.md`
- Modify: `DELTARUNE/speaker-system.md`
- Modify: `DELTARUNE/funnytext-system.md`
- Create: `DELTARUNE/rooms-events-and-progression.md`

**Interfaces:**

- Consumes: room/object placement, movement, collision, cutscene command,
  dialogue, choice, and flag traces.
- Produces: complete player-facing world/event reference and room catalog.

- [ ] Trace movement, collision, interactables, doors, cameras, layers, room
      transitions, encounters, followers, and special movement modes.
- [ ] Inventory every player-reachable room by chapter with purpose, important
      controllers, transitions, and conditional state.
- [ ] Document major events and cutscenes, command dispatch, blocking, cleanup,
      skip behavior, and save consequences.
- [ ] Document dialogue storage, speakers, portraits, typing effects, choices,
      localization, and route branches.
- [ ] Identify test, duplicate, legacy, and unreachable rooms with evidence.
- [ ] Add modding examples for a room transition, NPC branch, choice, cutscene,
      route flag, and localized line.
- [ ] Validate all identifiers and run full Markdown lint.

### Task 6: Rewrite Battle and SOUL Systems

**Files:**

- Modify: `DELTARUNE/battle-system.md`
- Modify: `DELTARUNE/damage-system.md`
- Modify: `DELTARUNE/tension-system.md`
- Modify: `DELTARUNE/heart-mechanics.md`
- Modify: `DELTARUNE/monster-runtime.md`
- Modify: `DELTARUNE/recruit-system.md`
- Modify: `DELTARUNE/game-over-system.md`
- Create: `DELTARUNE/enemies-encounters-and-attacks.md`

**Interfaces:**

- Consumes: battle controllers, enemy definitions, attack objects, bullet
  patterns, SOUL modes, ACTs, mercy, damage, TP, recruit, and defeat traces.
- Produces: canonical battle lifecycle and complete player-facing enemy catalog.

- [ ] Trace battle creation, party/enemy turns, menus, target selection, ACT,
      magic, items, defense, bullets, mercy, victory, rewards, and cleanup.
- [ ] Document all SOUL colors and special modes actually encountered across
      Chapters 1–5.
- [ ] Derive damage, defense, element, grazing, TP, status, mercy, recruit, and
      game-over formulas per chapter range.
- [ ] Inventory every player-facing enemy, encounter, boss, ACT set, attack
      family, reward, and route-dependent outcome.
- [ ] Distinguish active attacks from prototypes, duplicates, debug battles, and
      unreachable definitions.
- [ ] Add modding examples for an enemy, encounter, ACT, bullet pattern, SOUL
      mode interaction, and reward.
- [ ] Validate formulas and numeric tables with code evidence and run full lint.

### Task 7: Rewrite Characters, Stats, Items, and Economy

**Files:**

- Modify: `DELTARUNE/party-management.md`
- Modify: `DELTARUNE/level-up-system.md`
- Modify: `DELTARUNE/items-equipment.md`
- Modify: `DELTARUNE/spells-abilities.md`
- Modify: `DELTARUNE/fusion-system.md`
- Create: `DELTARUNE/characters-stats-and-progression.md`
- Create if required: `DELTARUNE/shops-and-economy.md`

**Interfaces:**

- Consumes: character arrays, party logic, item/spell info functions,
  inventories, equipment, shops, storage, fusion, and save serialization.
- Produces: complete catalogs and modification workflows for progression data.

- [ ] Inventory player-facing characters, party states, base stats, growth,
      equipment slots, spells, and chapter transitions.
- [ ] Inventory every obtainable or encounterable item, weapon, armor, key item,
      pocket item, consumable, spell, fusion recipe, shop entry, and storage
      rule.
- [ ] Mark unobtainable, legacy, debug, placeholder, removed-reference, and
      chapter-local entries using reachability evidence.
- [ ] Correct duplicated or conflicting party and stat explanations.
- [ ] Add modding examples for items, equipment, spells, recipes, shops, party
      members, and save-compatible stat changes.
- [ ] Verify all IDs, values, effects, prices, and chapter ranges.
- [ ] Run full Markdown lint.

### Task 8: Document Chapter-Specific and Audiovisual Systems

**Files:**

- Modify: `DELTARUNE/board-system.md`
- Modify: `DELTARUNE/rhythm-system.md`
- Create: `DELTARUNE/chapter-5-systems.md`
- Create: `DELTARUNE/rendering-animation-audio.md`

**Interfaces:**

- Consumes: Chapter 3–5 controllers, shaders, surfaces, sprites, timelines,
  particles, sounds, music, videos, and special gameplay traces.
- Produces: complete explanations of unique chapter mechanics and presentation
  pipelines.

- [ ] Re-verify board and rhythm systems across every payload that retains their
      code or uses them at runtime.
- [ ] Identify and trace every Chapter 5-specific player-facing system, event
      family, route interaction, battle extension, and presentation feature.
- [ ] Document animation state, sprites, layers, surfaces, particles, shaders,
      lighting, transitions, music, sound routing, audio groups, and video
      playback.
- [ ] Explain platform shader variants and helper DLL behavior only to the
      extent supported by shipped evidence.
- [ ] Add practical examples for animation, sound, music, shader effects, and
      Chapter 5-specific extension points.
- [ ] Run full Markdown lint.

### Task 9: Consolidate Debug, Legacy, and Unused Content

**Files:**

- Create: `DELTARUNE/debug-legacy-and-unused-content.md`
- Modify: all domain pages containing status classifications

**Interfaces:**

- Consumes: reachability work and unresolved evidence from all earlier tasks.
- Produces: one canonical status reference with domain-page cross-links.

- [ ] Collect test rooms, debug navigation, diagnostic scripts, unused
      encounters, duplicate resources, retained old systems, and removed
      references.
- [ ] Document why each entry receives its status and what evidence could
      invalidate the classification.
- [ ] Keep player-facing context in domain pages and avoid duplicating entire
      explanations in the status reference.
- [ ] Change unsupported `unused` claims to `uncertain`.
- [ ] Run full Markdown lint.

### Task 10: Final Consistency and Completeness Audit

**Files:**

- Modify: `DELTARUNE/README.md`
- Modify: `DELTARUNE/SUMMARY.md`
- Modify: any page failing the audit

**Interfaces:**

- Consumes: the entire updated wiki and validated evidence workspace.
- Produces: a navigable, consistent, lint-clean final documentation set.

- [ ] Reconcile every page against the installed payload manifest and remove
      stale build/count statements.
- [ ] Check all room, item, spell, enemy, character, route, major event, and
      chapter inventories for omissions against normalized evidence.
- [ ] Search for contradictory values, duplicate canonical explanations,
      unsupported absolutes, vague claims, and identifiers absent from evidence.
- [ ] Verify all relative links, heading anchors, and SUMMARY navigation.
- [ ] Run `markdownlint-cli2 "**/*.md"` and fix every error and warning without
      exclusions or rule changes.
- [ ] Re-run lint after formatting to catch `MD060` and table regressions.
- [ ] Inspect `git diff --check`, the complete diff, and `git status` to confirm
      there are no temporary files, accidental binaries, commits, or overwritten
      user edits.
- [ ] Report remaining uncertainty explicitly; do not claim completeness while
      any required category lacks authoritative evidence.
