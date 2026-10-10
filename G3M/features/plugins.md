# Plugins

Plugins extend G3M with Python code, settings, panels, and launch actions. Manage them in **Settings > Catalog > Plugins**.

## Install and enable

Select a catalog entry to read its description, author, version, API requirement, and homepage. **Install** downloads its package through [Downloads](downloads.md). Installation and enabling are separate actions.

Enable a plugin to use its operation hooks and main interface. G3M checks its API requirement and declared plugin dependencies. A required plugin must be available and enabled. Conflicting plugins cannot remain enabled together.

A local plugin can be imported from a ZIP or folder through the plugin import workflow. The package needs `plugin_config.json`; a mod ZIP or theme ZIP is not a plugin package. Local plugins also appear in the installed list.

## Settings, updates, and deletion

The plugin details dialog shows generated settings or the plugin's own settings interface. Plugin settings persist independently of the current game profile unless the plugin implements per-profile settings itself.

**Update** replaces the installed package when the catalog offers a higher plugin version. **Disable** stops its enabled hooks. **Delete** removes the installed plugin package. Review any plugin-specific data or backups before deleting a plugin that manages saves or files.

G3M marks a plugin incompatible when it cannot use the plugin's API requirement. A broken entry includes an error rather than functioning as an enabled plugin. Check [Logs](../advanced/logging.md) for import or hook failures.

## Included catalog plugins

| Plugin | Package version | Purpose |
| --- | --- | --- |
| [DR Save Manager](deltarune-save-manager.md) | 1.2.3 | Manage save collections and edit DELTARUNE save data from G3M. |
| [Custom Save Folders](custom-save-folders.md) | 1.1.5 | Choose separate save folders and assign save-folder rules to mods. |

These are optional downloads from the catalog. Their settings and panels are supplied by the plugins, so they can have controls specific to save editing or folder management.

## Trust

A plugin runs Python code with G3M's process permissions. Opening a custom settings interface can load code even when the plugin's operation hooks are disabled. Install plugins from authors you trust, and keep separate backups of important saves.

For authoring and the extension interface, see [Create a plugin](plugins-development.md).
