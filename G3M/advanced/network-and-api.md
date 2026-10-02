# Network use

| Feature | Connection purpose |
| --- | --- |
| Mods Browser and downloads | GameBanana requests and package downloads |
| Catalog | GitHub plugin/theme indexes, icons, and archives |
| Community | GameBanana feeds and enabled plugin RSS feeds |
| Updates and announcements | G3M online settings and release information |
| Polls | Submitting selected choices with a persistent hashed voting identity |
| Online count | Session presence requests to the G3M service |
| Discord Rich Presence | Activity handoff to the local Discord client |

The online count uses a session identifier. Poll voting uses its own persistent local identity in hashed form. Requests also expose the connection information normally visible to their receiving server. Avoid interpreting a local Library as meaning the whole application makes no network requests.

**Disable Discord Rich Presence** in Appearance prevents that integration from publishing G3M activity through Discord. It does not disable GameBanana, updates, or other internet features.

## Offline use

Installed mods, local import, editing, profile switching, and configured local launches work offline when their tools and game files are available locally. Apply saved themes from **Settings > Appearance**. Catalog discovery and downloads require an internet connection.

A cached online response is not a promise that remote files remain accessible. Check Downloads for transfer errors and retry rate-limited requests later.

Support packages are saved locally and are not uploaded by the build action. Plugins and executable scripts can make their own requests beyond the application features listed here.
