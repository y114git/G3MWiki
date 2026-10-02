# Blocklist

Blocklist hides matching entries from filtered mod results. It does not uninstall a mod or remove it from a saved launch selection.

Open the Blocklist control, select a game or **Global**, choose a rule type, enter its value, and click **Add**. Select a rule and use **Remove** to delete it.

| Rule | Matching behavior | Example |
| --- | --- | --- |
| ID | Exact ID, ignoring case; recognized GameBanana-backed IDs can match the submission number | `123456` |
| Name | Case-insensitive substring of the mod name | `texture experiment` |
| Category | Exact category label, ignoring case | The category text shown for the upload |

Global rules combine with the selected game's rules. Any matching rule hides the entry. A category rule matches category metadata, rather than every custom tag with similar wording.

Rules save to `settings/blocklist.json` in the data directory. If a wanted result disappears, check both its game rules and Global rules before retrying the search.
