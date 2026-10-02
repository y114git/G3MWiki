# Theme colors

The **Colors** section in **Settings > Appearance** has seven editable channels. Select a color with the picker or type a hexadecimal RGB value such as `#58B878`.

| Channel | What it controls | Default |
| --- | --- | --- |
| Background | Background layer and panel backdrop. | `#282828` |
| Elements | Buttons, fields, and other control backgrounds. | `#222222` |
| Border | Control and panel outlines. | `#039D5B` |
| Hover | Highlight for hovered controls and items. | `#616B78` |
| Selection | Selected-item appearance. | `#ECEDEF` |
| Main text | Primary labels and content. | `#E8E9EB` |
| Secondary text | Secondary labels and metadata. | `#6DE985` |

Colors apply across the main interface and themed dialogs. An empty stored override uses the default channel color. Use a color's reset control to remove the override rather than trying to reproduce the default by eye.

Selection and hover have different roles. Test both against the main text color, including a selected library card, a checked control, and a focused text field. Check dialogs as well as the main window, because long descriptions and disabled controls can expose unreadable combinations.

Exporting a [theme package](theme-packages.md) stores all seven color overrides. It does not export a custom stylesheet.
