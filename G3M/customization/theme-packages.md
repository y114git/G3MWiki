# Theme packages

A theme ZIP contains `theme_config.json` and optional media files at the package root. G3M uses the configuration to set colors and appearance options, then copies the media into its data directory.

The theme configuration version is `1.0.0`. This is separate from the mod configuration and plugin API versions.

## Package layout

```text
my_theme.zip
├── theme_config.json
├── background.png
├── background_music.ogg
├── startup_sound.wav
├── custom_logo.png
└── custom_font.ttf
```

Only the configuration is required. Use one file per media role. Keep these filenames, with the extension of the actual file format.

| Filename | Role |
| --- | --- |
| `background.<extension>` | Background image, GIF, or video. |
| `background_music.<extension>` | Repeating background music. |
| `startup_sound.<extension>` | Sound played on startup. |
| `custom_logo.<extension>` | Main logo image. |
| `custom_font.ttf` or `custom_font.otf` | Interface font. |

A single enclosing folder is accepted, but putting all package files directly at the ZIP root avoids ambiguity. The media files belong beside the manifest, not in an arbitrary nested assets folder.

## Configuration

```json
{
  "config_version": "1.0.0",
  "custom_background_color": "#202020",
  "custom_elements_color": "#242424",
  "custom_border_color": "#58B878",
  "custom_hover_color": "#404A44",
  "custom_select_color": "#DDE6DF",
  "custom_main_text_color": "#F0F0F0",
  "custom_secondary_text_color": "#9ED8AF",
  "background_disabled": false,
  "disable_animations": false,
  "disable_startup_sound": false,
  "custom_border_radius": 7
}
```

The seven color fields use hexadecimal RGB colors. An empty color string uses G3M's default color for that channel. The three boolean fields control the corresponding Appearance checkboxes. `custom_border_radius` is corner rounding in pixels.

Application language, game paths, installed mods, UI scale, Discord preferences, and the stop-music-on-unfocus preference are not part of the standard theme export. A theme is an appearance package, not a settings backup.

## Export and import

To create a package through the interface:

1. Set the colors and media in **Settings > Appearance**.
2. Open **Im/Export Theme**.
3. Choose **Export** and save the ZIP.
4. Import that ZIP to check its appearance and media.

The export includes the configured background, music, startup sound, logo, and font when their files exist. It does not depend on the files remaining at your original editing locations.

Import applies the package. It also saves a copy in the theme list unless **Do not save theme in list after import** is checked. Applying another theme replaces active custom media; omitted media roles do not keep the previous theme's custom music, logo, or font.

Saved packages can be applied again from Appearance. Deleting a package removes that saved copy but does not automatically reset the active appearance.

## Catalog entry

A theme catalog entry has `id`, `name`, `description`, `author`, `version`, `icon`, `homepage`, `download_link`, and `tags`. This information belongs to the catalog list, not `theme_config.json`.

Supported tags are `minimalist`, `game_inspired`, `animated`, and `with_sound`. Use `animated` for a theme with animated media and `with_sound` when the package contains audio. An icon URL can point to the theme's `custom_logo.png`.

The `themes.json` list has this structure. Replace the example URLs with public URLs for your files:

```json
{
  "themes": [
    {
      "id": "my_theme",
      "name": "My theme",
      "description": "Dark green colors with a still background.",
      "author": "Your name",
      "version": "1.0.0",
      "icon": "https://example.com/my_theme/custom_logo.png",
      "homepage": "https://example.com/my_theme",
      "download_link": "https://example.com/my_theme.zip",
      "tags": ["minimalist"]
    }
  ]
}
```

A catalog download is still a theme ZIP, so test it through local import before submitting it for listing. Check that the public icon and download URLs work independently of a signed-in GitHub session.

## Media compatibility

The [background](background.md), [audio](audio.md), and [font and logo](fonts-and-logo.md) references list usable file formats. A supported extension does not guarantee that every video or audio codec works on every operating system.

Use media and fonts that you have permission to redistribute. Keep a font's license notice when its license requires it.
