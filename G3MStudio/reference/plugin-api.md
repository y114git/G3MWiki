# Writing a plugin

This page is for people who write plugins. It describes the plugin folder and `plugin.json`, then each part of the C# interface a plugin gets. To install and manage plugins, see [Plugins](../settings/plugins.md).

A plugin is plain C# source. G3MStudio compiles it when it starts the plugin, so there is nothing to build by hand. A plugin has full access to the computer, like a script. Treat a plugin from someone else the way you treat a program.

## Make a plugin

1. Open **Tools** > **Plugins...** and press **New Plugin**. This makes a folder such as `plugins/newplugin` with `plugin.json` and `src/Main.cs`, with one command (**New Plugin: Say Hello**) that shows a notice.
2. Press **Open Folder** and edit `src/Main.cs`.
3. Save. With **Reload a plugin when its files are saved** on (see [Advanced settings](../settings/advanced.md)), the plugin restarts at once. Otherwise press **Reload**.

A compile error shows after **Error:** in the details box. Only the first five errors are listed. Plugins are compiled with Roslyn on a background thread when they reload, so the window keeps responding.

## Folder layout

```text
plugins/
  my-plugin/
    plugin.json
    src/
      Main.cs
    lib/            optional, extra .dll files
```

The `plugins` folder is in the [data folder](../getting-started/user-data.md). All `.cs` files under `src` are compiled together, in name order. Every `.dll` in `lib` is added as a reference and can be loaded by the plugin. The compiled result is cached in `plugin-cache` and is rebuilt when a source file, a library or the program itself changes.

The compiler sees the .NET libraries and all assemblies of G3MStudio and G3MLib. The usings `System`, `System.Collections.Generic`, `System.IO`, `System.Linq`, `System.Threading` and `System.Threading.Tasks` are on for every file. Nullable checks are on.

## plugin.json

```json
{
  "id": "word-count",
  "name": "Word Count",
  "version": "0.1.0",
  "description": "Counts the words of the open entry.",
  "entry": "src/Main.cs",
  "authors": ["Your Name"],
  "permissions": ["Reads code"],
  "homepage": "https://github.com/owner/repository"
}
```

| Field | Required | Default | Meaning |
| --- | --- | --- | --- |
| `id` | yes | | Unique id. It names the options file, the cache and the command ids. Installing a plugin with the same id replaces the old one. |
| `name` | yes | | Shown name. |
| `version` | yes | | Shown version. |
| `description` | no | empty | One line for the details box. |
| `entry` | no | `src/Main.cs` | File with the plugin class. |
| `authors` | no | empty | Shown in the install question. |
| `permissions` | no | empty | Text shown in the install question and in the details box. It is not enforced. |
| `homepage` | no | empty | Shown in the details box. |

Comments and trailing commas are allowed. The names are not case sensitive.

## The plugin class

The compiled assembly needs a class that implements `IStudioPlugin`. G3MStudio creates the first one it finds, so it needs a parameterless constructor. If none exists the plugin fails with "No class in the plugin implements IStudioPlugin."

```csharp
using G3MStudio.Core.Notifications;
using G3MStudio.Core.Plugins;

public sealed class Main : IStudioPlugin
{
    public void Activate(IStudioContext context) =>
        context.AddCommand("hello", "Word Count: Say Hello",
            () => context.Notify(NotificationLevel.Info, "Hello."));

    public void Deactivate()
    {
    }
}
```

`Activate` runs when the plugin starts or reloads. `Deactivate` runs before it stops. Everything added through the context is removed when the plugin stops, so `Deactivate` is only for things the plugin made itself. Each plugin is loaded in its own load context, which is dropped when the plugin stops. Static fields do not survive a reload.

## The context

| Member | What it does |
| --- | --- |
| `AddCommand(id, title, run)` | Adds a command. It is listed in the **Plugins** menu and in the command palette under the title you give. The full command id is `plugin.<plugin id>.<id>`, so you can bind a key to it in [Keyboard shortcuts](../settings/shortcuts-editor.md). The id only has to be unique inside the plugin. |
| `AddPanel(id, title, create)` | Adds a command that opens a window with a control you build. `create` returns an Avalonia control and runs each time the window opens. The command id is `plugin.<plugin id>.panel.<id>`. |
| `AddEditor(typeKey, create)` | Shows a control above the property list in the tab of a resource. `typeKey` is the Explorer type key, such as `sprites` or `objects`. `create` gets the resource model. Return `null` to show nothing for that resource. The first active plugin that returns a control wins. |
| `AddCodeHints(provider)` | Adds gray notes at the end of code lines. See below. |
| `AddCodeAction(id, title, run)` | Adds an entry to the code editor context menu. See below. |
| `AddSetting(setting)` and `Settings` | Declares options and reads them. See below. |
| `On(eventName, handler)` | Calls the handler when something happens. See below. |
| `Notify(level, message)` | Shows a notice. The level is a `NotificationLevel`. |
| `Log(message)` | Writes a line to the plugin log. |
| `Workspace` | The open data file, or `null`. Edits go through `Workspace.Edit` so Undo works. |
| `CodeEntries` | The names of the code entries that have text of their own. Child entries belong to their parent and are not listed. |
| `GetCode(entry)` | The decompiled text of an entry as the editor shows it, or `null` when it cannot be decompiled. |
| `SetCodeAsync(entry, text)` | Replaces the code of an entry. It compiles first. It returns `null` on success, or the error text. One undo step. A failed compile changes nothing. |
| `GameFolder` | The folder of the open data file, where the game's own files such as `lang` live, or `null`. |

A command that throws is logged, and a notice names the plugin and the message. The program keeps running.

## Options

Declare an option in `Activate` and the **Plugins** window shows a control for it under **Options**.

```csharp
context.AddSetting(new PluginSetting("show", "Show notes", PluginSettingKind.Bool, "true",
    "Turn the gray notes on or off."));
```

The constructor is `PluginSetting(key, label, kind, default, description = "", choices = null, min = 0, max = 1000)`. `label` is the text next to the control and `description` is the gray line under it. Declaring the same key again replaces the definition.

| Kind | Control | Default text |
| --- | --- | --- |
| `Bool` | Check box | `"true"` or `"false"` |
| `Text` | Text box | The text |
| `Number` | Number box between `Min` and `Max` (0 and 1000 unless set) | The number written out |
| `Choice` | List of `Choices` | One of the choices |

Read a value with `context.Settings.Get(key)`, `GetBool(key)` or `GetNumber(key)`. `Set` and `Reset` change it. A change from the window is saved at once, and `Settings.Changed` fires. Code hints are asked again after every change. Values are kept in `plugin-settings/<plugin id>.json` in the data folder, so they survive a reload and an update. A file that cannot be read is ignored and the reason goes to the log.

## Events

`On("workspace.opened", handler)` runs when a data file is opened or switched to. The handler gets a `PluginEvent` with the event name. A handler that throws is logged and skipped. This is the only event for now.

## Code hints

A hint is a gray note shown after the end of a line, for example the text behind a key. It is not part of the code: it is never saved and never compiled.

```csharp
context.AddCodeHints(request => request.Text.Split('\n')
    .Select((line, i) => (line, i))
    .Where(x => x.line.Contains("TODO"))
    .Select(x => new CodeHint(x.i + 1, "open item", "Found a TODO on this line.")));
```

`CodeRequest` holds the entry name and the text as shown in the editor. `CodeHint` has a one-based line number, the note and an optional tooltip. The provider runs on the interface thread shortly after the text changes, when the plugin reloads and when an option changes. Keep it quick: read files once and cache them. A provider that throws is logged as `Code hints:` and skipped.

## Code actions

A code action is an entry in the code editor context menu. Its `run` function gets a `CodeRequest` with the entry, the whole text and the selection (length 0 when nothing is selected). It returns the whole new text, or `null` to change nothing. The change is one undo step.

## The plugin log

Each plugin has a log. `Log(message)` adds a line, and so does every exception a plugin throws. Read it in **Tools** > **Plugins...**: select the plugin and look at the details box. The log is kept in memory and is empty again after a restart. It is not a file.

## Failures

A plugin that fails to compile or start is turned off. The notice reads "'{name}' failed to start and was turned off: {error}". A broken editor, hint provider, code action or event handler is logged and skipped, and the others keep working. If **Run plugins** is off, no plugin starts.

## See also

- [Plugins](../settings/plugins.md)
- [Advanced settings](../settings/advanced.md)
- [Code editor](../code/code-editor.md)
- [Data folder](../getting-started/user-data.md)
