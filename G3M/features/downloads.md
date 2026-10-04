# Downloads

Open **Downloads** to view downloaded mods, plugins, themes, and local imports recorded by G3M. Each item reports file transfer and installation status separately.

## Actions

| Action | Purpose |
| --- | --- |
| Install | Imports a completed package |
| Reinstall | Imports a downloaded package again if its file is available |
| Cancel | Stops an active download |
| Retry | Tries a failed transfer again |
| Overwrite | Resolves an import waiting for replacement confirmation |
| Cancel install | Cancels the pending installation decision |
| Continue setup | Opens the manual configuration required by the package |
| Delete | Removes the download record and its associated downloaded file after confirmation |
| Open folder | Opens the downloads directory |
| Clear downloads | Clears completed records through the dialog's cleanup action |

Only actions applicable to the item's state are displayed. A ready file is downloaded but not necessarily installed. An overwrite-pending or manual-setup item needs a decision before it becomes a Library mod.

Drop local files or download URLs into this window to add them. Online files must have an accessible download URL.

## Automatic import

**Settings > Mods Browser > Downloads** has three options:

- **Do not install downloaded files automatically** leaves completed files ready for an explicit Install action.
- **Delete downloaded file after use** removes the downloaded package after successful installation. The installed mod remains in its profile.
- **Save local imports in Downloads** records imports from disk in download history.

History is stored in `downloads/downloads_history.json` under the data directory. Removing an archive outside G3M can leave a record without a usable file; download it again rather than using Reinstall.

## Failed transfers

Read the item's error before retrying. A missing local source, denied write access, HTTP 404, certificate failure, and server rate limit require different fixes. HTTP 429 means the server is limiting requests; repeated immediate retries can prolong the problem.

For a completed archive that needs configuration, use **Continue setup** to open [Manual Mod Installation](../mods/importing.md#files-without-configuration). Configure its actions and choose **Save**, or **Save and configure** to continue in Mod Editor. Cancelling setup leaves the download available for another attempt. Downloading the same file again does not supply a missing `mod_config.json` or infer an unknown destination.
