# Support packages

Open **Windows > Support Packager** to save diagnostic information to a local ZIP. Building a package does not upload it.

## Choose contents

With **Custom package settings** disabled, every available item is included. Enable it to select individual items, especially when collecting a report to share.

| Group | Available selections |
| --- | --- |
| Application | G3M version; selected game, chapter, and profile; launch, patching, and installation state; background-operation counts; current launch state. |
| System | Operating system and version; architecture and processor; Python runtime; memory usage; boot and collection time; process names without command lines; network counters; CPU, disk, and network samples; UI responsiveness samples. |
| Configuration and metadata | Settings with recognized secrets removed, installed-mod metadata, and G3M folder structure with file sizes. |
| G3M JSON, Markdown, and text files | Individual files found under the G3M data directory. |
| G3MPatch manifests | Metadata from available resource patches. |
| Logs | Individual available logs, restricted by the chosen log range. |
| Game folder structures | Names and sizes under configured game folders. |
| Supported AppData folder structures | Names and sizes under recognized game user-data locations. |
| Each installed mod | Configuration, folder structure, or full bundled files. |

File and mod selections depend on what exists in the data directory. An empty group reports that no available items were found.

The full-mod-files option includes payload files. Folder-structure options describe names and sizes rather than copying an entire game installation.

Choose the log range: all available logs, the last 24 hours, 7 days, or 30 days. Select **Build**, choose the ZIP destination, and wait for completion. Cancel closes or stops the collection as available in the dialog.

## Share a report

G3M redacts recognized account names and secret values in diagnostic text. Review the ZIP yourself before sharing: arbitrary mod files and unfamiliar secret formats can contain information the redaction does not identify.

Include the steps that reproduce the issue and the expected result alongside the package. For a patch-order problem, an exported [Diagnostics report](diagnostics.md) can show the affected resources more directly than a general system report.
