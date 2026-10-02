# Package examples

Each example contains a complete `mod_config.json`. Place the files shown beside it, then create a ZIP with the configuration at its root. The [configuration reference](mod-config.md) describes accepted fields and operation types.

## Replace loose files

This package copies the contents of `assets/` into the game's `assets/` folder. Existing files with matching names are replaced; other destination files remain.

```text
asset_pack.zip
├── mod_config.json
├── README.md
└── assets/
    ├── logo.png
    └── music.ogg
```

```json
{
  "config_version": "2.0.0",
  "id": "asset_pack",
  "name": "Asset pack",
  "version": "1.0.0",
  "authors": ["Your name"],
  "game": "undertale",
  "files": [
    {
      "source": "${mod_path}/assets/",
      "target": "${game_path}/assets/",
      "type": "extract"
    },
    {"source": "${mod_path}/README.md", "type": "info"}
  ]
}
```

Replace these example filenames and destinations with files the game actually reads. Many GameMaker assets are inside DATA and require a data patch instead. Creating an `assets/` folder alone does not make the game load its contents.

To supply defaults without replacing existing destination files, use `soft-extract`. To replace a dedicated destination folder's entire contents, use `hard-extract`. Check its [clearing rules](mod-config.md#clear-a-destination) before choosing it.

## Patch two DELTARUNE chapters

The two patches target their own chapter DATA. Each patch uses an unmodified original from that chapter's supported game release.

```text
chapter_edits.zip
├── mod_config.json
├── chapter1.g3mpatch
├── chapter2.g3mpatch
└── README.md
```

```json
{
  "config_version": "2.0.0",
  "id": "chapter_edits",
  "name": "Chapter edits",
  "version": "1.0.0",
  "authors": ["Your name"],
  "game": "deltarune",
  "placeholders": {
    "chapter_one": "${game_path}/chapter1_windows",
    "chapter_two": "${game_path}/chapter2_windows"
  },
  "files": [
    {
      "Chapter 1": [
        {
          "source": "${mod_path}/chapter1.g3mpatch",
          "target": "${chapter_one}/data.win",
          "type": "patch"
        }
      ]
    },
    {
      "Chapter 2": [
        {
          "source": "${mod_path}/chapter2.g3mpatch",
          "target": "${chapter_two}/data.win",
          "type": "patch"
        }
      ]
    },
    {"source": "${mod_path}/README.md", "type": "info"}
  ]
}
```

Groups keep chapter actions together in the editor. They do not depend on the chapter selected for launching: the package contains both actions. G3M adjusts chapter and DATA targets for the game runtime, but patch compatibility still needs testing on each supported release.

## Require a patched base

If an addon patch is created against the output of another mod, give the addon a dependency such as:

```json
{
  "dependencies": ["base_mod:before-step"]
}
```

Add this field to the addon's complete configuration. The player places `base_mod` in an earlier library step and the addon in a later step. Mods in one step are combined against that step's common input; row priority cannot substitute for applying a required base first.

For a loose file used only when another mod leaves it absent, choose a soft copy or extract operation. A dependency declaration alone does not change how files are copied.

## Check the distributed package

Import the ZIP into a testing profile. Check that its icon and information document open, select the intended game, and inspect [Mod Diagnostics](../features/diagnostics.md) before launching. Test the game and restoration after exit. For a multi-chapter package, check every changed chapter.
