# Catalog

**Settings > Catalog** has separate **Plugins** and **Themes** lists. The catalog downloads package information from GitHub; installing an item downloads its ZIP.

## Filters and details

**Installed only** limits the current list to installed items. Each type has its own tags:

| Plugins | Themes |
| --- | --- |
| Interface | Minimalist |
| Game Experience | Game-inspired |
| Tool | Animated |
| Other | With sound |

An item shows its icon, name, description, author, version, and tags. The available actions depend on installation state. Plugin details also show an API requirement and plugin settings.

## Plugins

Use **Install** to download and install a package. Enable it separately. Use **Update** when an update is available, or **Delete** to remove its installed files. Local plugin packages can also appear in the installed list.

See [Plugins](plugins.md) for API compatibility, dependencies, and the difference between loading and enabling code.

## Themes

Use **Install** to save a theme package, **Apply** to use an installed theme, and **Delete** to remove its saved package. Installation does not require applying the theme immediately.

Installed themes also remain available in **Settings > Appearance**. Deleting a saved theme package does not itself undo the appearance already applied to G3M. Apply another theme or reset the relevant appearance controls to change that appearance.

The catalog includes CLEANG3M, DELTAG3M, PIZZAG3M, and SUGARYG3M. Their packages are downloads, not assets bundled into the application.

## Connection and local use

Catalog discovery and downloads need an internet connection. Installed theme packages and installed plugins are local files and do not need to be downloaded again for each use. An individual plugin can have its own network requirements.

The public package lists are [plugins.json](https://github.com/y114git/G3M/blob/main/catalog/plugins/plugins.json) and [themes.json](https://github.com/y114git/G3M/blob/main/catalog/themes/themes.json). Each is an independent list for its package type.

Theme authors can distribute a ZIP directly without a catalog listing. See [Theme packages](../customization/theme-packages.md) for the format.
