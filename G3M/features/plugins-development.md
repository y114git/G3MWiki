# Create a plugin

Plugins add Python code and PyQt6 interfaces to G3M. The plugin API version is `1.3.0`. A plugin package has a manifest and a Python entry file with a `create_plugin()` factory.

This guide describes the extension interface. For installing and managing a plugin, see [Plugins](plugins.md).

## Minimal package

```text
example_plugin/
├── plugin_config.json
└── plugin.py
```

`plugin_config.json`:

```json
{
  "config_version": 1,
  "id": "example_plugin",
  "name": "Example plugin",
  "description": "Shows a panel with a saved message.",
  "author": "Your name",
  "version": "1.0.0",
  "api_version": ">=1.3.0",
  "entry": "plugin.py",
  "tags": ["interface"],
  "hooks": ["main_view"],
  "settings_schema": {
    "fields": [
      {
        "key": "message",
        "type": "string",
        "label": "Message",
        "default": "Hello from a plugin"
      }
    ]
  }
}
```

`plugin.py`:

```python
from PyQt6.QtWidgets import QLabel, QVBoxLayout, QWidget


class ExamplePlugin:
	def create_main_widget(self, ui_context, parent):
		widget = QWidget(parent)
		layout = QVBoxLayout(widget)
		settings = ui_context.host_context.plugin_settings
		message = settings.get("message", "Hello from a plugin")
		layout.addWidget(QLabel(message, widget))
		return widget


def create_plugin():
	return ExamplePlugin()
```

Install the folder or a ZIP containing it through G3M's local plugin import, then enable it in **Settings > Catalog > Plugins**. Its main panel uses the saved message. The plugin details dialog exposes the text setting. Reopening or rebuilding the panel reads the saved value; this example does not update an existing label as you type.

## Manifest reference

| Field | Required | Accepted value |
| --- | --- | --- |
| `config_version` | Yes | Integer `1`. |
| `id` | Yes | Nonempty lowercase ASCII letters, digits, and underscores. |
| `name` | Yes | Display text or a plugin translation key. |
| `description` | Yes | Description text or a plugin translation key. |
| `author` | Yes | Author name. |
| `version` | Yes | Three numeric components, such as `1.0.0`. |
| `api_version` | Yes | Exact API version or one supported version requirement. |
| `entry` | Yes | Existing Python file within the package. |
| `icon` | No | Existing image file within the package. |
| `homepage` | No | Project page shown by the homepage action. |
| `tags` | No | `interface`, `game_experience`, `tool`, or `other`. |
| `relations` | No | Map of other plugin IDs to `require` or `conflict`. |
| `hooks` | No | Array of supported hook names or interface contributions. |
| `settings_schema` | No | Object with a `fields` array for automatically generated settings. |

Entry and icon paths cannot escape the plugin folder. Plugin archive limits are 5,000 entries and 200 MiB uncompressed.

Supported API requirement prefixes are `>=`, `>`, `<=`, `<`, `^`, and `~`. Exact `1.3.0` requires that API version. `>=1.3.0` accepts that version or a higher one. `~1.3.0` accepts patch versions within `1.3`. `^1.3.0` accepts compatible versions within major version `1`. Compound ranges and wildcards are not supported. The plugin's own version is always an exact three-component number.

A dependency such as `"relations": {"helper_plugin": "require"}` requires that plugin to be enabled. A `conflict` entry identifies a plugin that cannot remain enabled alongside it. G3M checks these relationships when enabling plugins.

## Loading and settings

The factory takes no arguments. G3M calls these optional instance methods:

| Method | When it runs |
| --- | --- |
| `on_load(context)` | When G3M loads the instance, including when it needs a settings widget. |
| `on_enable(context)` | When G3M enables the instance. |
| `on_disable(context)` | When G3M disables the instance. |

These lifecycle method names do not belong in `hooks`.

`PluginContext` contains `plugin_id`, `app_state`, `feedback_service`, `settings_service`, `profile_service`, `game_registry_service`, `localization_service`, `plugin_settings`, and an optional `task_runtime`.

Use the plugin settings accessor for the plugin's own saved values:

```python
value = context.plugin_settings.get("confirm_before_use", True)
context.plugin_settings.set("confirm_before_use", False)
settings = context.plugin_settings.all()
```

These settings belong to the plugin, not automatically to the selected profile. A plugin that needs per-profile values must key them by profile itself. A schema default determines the initial displayed value; pass the same default when reading a setting that has not been saved.

### Generated settings fields

Each field has a `key`, a `type`, and optional `label`, `description`, and `default` values. Labels and descriptions can be translation keys.

| Type | Interface and additional fields |
| --- | --- |
| `bool` | Checkbox. |
| `int` | Integer spin box, with `min` and `max`. Defaults are 0 and 999999. |
| `choice` | Combo box. `options` contains objects with `value` and `label`. |
| `string` | Text field, saved when editing finishes. |
| `action` or `button` | Button. `button_text` changes its caption. |

A choice field looks like this:

```json
{
  "key": "mode",
  "type": "choice",
  "label": "Mode",
  "default": "manual",
  "options": [
    {"value": "manual", "label": "Manual"},
    {"value": "automatic", "label": "Automatic"}
  ]
}
```

For an action field, implement `handle_settings_action(action_id, ui_context, parent)`. The action ID is the field's key. An action is not a stored boolean setting.

## Interface contributions

`PluginUiContext` contains `plugin_id`, `app_state`, and `host_context`. Its `host_context` is the regular `PluginContext`. Build and update Qt widgets on the GUI thread.

| Manifest declaration | Instance method |
| --- | --- |
| `main_view` | `create_main_widget(ui_context, parent)` returns a widget for the main plugin panel. |
| `settings_view` | `create_settings_widget(ui_context, parent)` returns a custom settings widget. |
| `launch_action` | `get_launch_actions(ui_context)` supplies launch-menu actions. |
| `launch_option_changed` | `get_launch_options(ui_context)` supplies checkable launch-menu options. |
| `community_feeds` | `get_community_feeds(ui_context)` supplies community RSS feeds. |

A custom settings widget takes precedence over generated schema fields. Give returned widgets the supplied parent. Keep long work outside widget construction so opening a view does not freeze G3M.

### Launch action

```python
from models.plugin_models import PluginLaunchAction


def get_launch_actions(self, ui_context):
	return [
		PluginLaunchAction(
			id="prepare_files",
			label="Prepare files",
			description="Run the plugin's file preparation task.",
			requires_confirmation=True,
		)
	]


def on_launch_action(self, context, action_id):
	if action_id != "prepare_files":
		return False
	if context.task_runtime:
		context.task_runtime.raise_if_cancelled()
		context.task_runtime.set_status("Preparing files")
	return True
```

These are methods of your plugin class. Action IDs contain lowercase letters, digits, and underscores. G3M prefixes the selected menu identifier with the plugin ID; the handler receives the local action ID. The action replaces the selected built-in launch action. It does not launch a game unless the plugin implements that behavior.

### Checkable launch option

`get_launch_options(ui_context)` returns `PluginLaunchOption` objects or corresponding dictionaries. Each option has `id`, `label`, `description`, `checked`, `enabled`, and `disabled_reason`. G3M calls `on_launch_option_changed(ui_context, option_id, checked)` when the user changes it. Returning `False` rejects the change. Save the accepted value through `ui_context.host_context.plugin_settings`.

### Community feed

`get_community_feeds(ui_context)` returns `PluginCommunityFeed` objects or dictionaries with `id`, `label`, and `url`. Feed URLs must use HTTPS. The selected feed appears in the [Community](community.md) dialog.

## Operation hooks

G3M calls `on_<hook_name>(context, ...)` for operation and notification hooks. Extra arguments depend on the hook. Accept unused arguments with `*_args` when your implementation needs only the context.

| Hook | Extra arguments or use |
| --- | --- |
| `before_mod_apply` | Current selections, before G3M applies mods. |
| `after_mod_apply_before_launch` | Selections and the multi-mod flag, after application and before launching or committing changes. |
| `after_mod_apply_committed` | Payload with `mode`, after a keep-changes or patching-only operation commits its changes. |
| `mod_apply_cancelled` | Cleanup payload identifying a hook or operation and its cancellation or failure reason. |
| `after_game_started` | Vanilla-mode flag. |
| `before_restore_after_exit` | Vanilla-mode flag, before managed changes are restored. |
| `after_restore_after_exit` | Vanilla-mode flag, after restoration. |
| `shortcut_dialog` | Shortcut context while the user configures a shortcut. |
| `before_mod_apply_shortcut` | Shortcut context and shortcut configuration. |
| `after_mod_apply_before_launch_shortcut` | Shortcut context and shortcut configuration. |
| `before_restore_after_exit_shortcut` | Shortcut context and shortcut configuration. |
| `after_restore_after_exit_shortcut` | Shortcut context and shortcut configuration. |
| `language_changed` | No extra arguments. Refresh translated plugin content. |
| `theme_changed` | No extra arguments. Refresh plugin styling. |
| `profile_changed` | No extra arguments. Refresh profile-dependent content. |
| `launch_action` | Local action ID for the selected plugin action. |
| `launch_action_cancelled` | Action ID and cancellation payload. |

Operation hooks that run as tasks receive `context.task_runtime`. Returning `False` from a task hook stops that operation; an exception reports a failure. Cancellation cleanup must undo changes made by the plugin itself. G3M's own game backups do not cover arbitrary plugin writes.

```python
def on_before_mod_apply(self, context, *_args):
	runtime = context.task_runtime
	if runtime:
		runtime.raise_if_cancelled()
		runtime.set_progress(20, "Checking plugin files")
	return True
```

`task_runtime.set_progress(percent, message)` updates progress. `set_status(message, status_type)` sends a status message. `is_cancelled()` checks cancellation, and `raise_if_cancelled()` raises `InterruptedError`. Register a subprocess with `track_process(process, cancel_callback)` so cancellation can stop it. Do not directly modify Qt widgets from a worker hook.

Shortcut hooks run through the shortcut runner rather than a visible main window. Do not assume that a main-window widget exists or that a task runtime is present.

## Localization

Put translations in `lang/lang_en.json`, `lang/lang_ru.json`, or another supported language file. A plugin language file contains its own relative keys:

```json
{
  "name": "Example plugin",
  "description": "Shows a panel with a saved message.",
  "message_label": "Message"
}
```

Manifest keys can use `plugins.example_plugin.name`. In plugin code, use `context.localization_service.get_plugin_tr(context.plugin_id)` and call the returned translator with `"message_label"`. This keeps keys in the plugin's namespace.

## Distribution and testing

A ZIP must contain one identifiable plugin package with its manifest and entry file. Test local installation, enabling, disabling, reopening settings, switching profiles, changing language and theme, and any shortcut behavior you claim to support.

Also test cancellation and failures after the plugin changes files. For a plugin with a launch action, confirm that its action is still understandable when another launch mode is selected afterward.

To publish a catalog entry, supply the plugin ID, name, description, author, version, API requirement, icon URL, homepage, download link, tags, and relations. The archive's manifest and catalog entry must describe the same plugin. See [Catalog](catalog.md).

The `plugins.json` list uses the following structure. Here `icon` is a public URL, while the package manifest's `icon` refers to a bundled file. Replace the example URLs with your hosted files:

```json
{
  "plugins": [
    {
      "id": "example_plugin",
      "name": "Example plugin",
      "description": "Shows a panel with a saved message.",
      "author": "Your name",
      "version": "1.0.0",
      "api_version": ">=1.3.0",
      "icon": "https://example.com/example_plugin/icon.png",
      "homepage": "https://example.com/example_plugin",
      "download_link": "https://example.com/example_plugin.zip",
      "tags": ["interface"],
      "relations": {}
    }
  ]
}
```

A plugin executes with G3M's process permissions. Loading a settings widget can execute plugin code even if its operation hooks are disabled. Do not describe a plugin as sandboxed or rely on disabling it as a security boundary.
