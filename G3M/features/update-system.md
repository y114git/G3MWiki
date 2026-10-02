# Application and mod updates

G3M checks online update information during startup and offers an available application update. The package is chosen for the current operating system and architecture: Windows, Linux, or macOS, with x64 or ARM64 variants.

## Application update

Review the offered version and release information before starting the update. G3M downloads the package and hands over to the platform's installation or replacement process. Allow the application to close when required, and avoid updating during an active game or file operation.

On Windows, the update can start the supplied installer. On Linux and macOS, it stages the release contents for replacement and restart. Write permission to the installation is required.

**Settings > General > Beta Updates** selects the beta update channel. Enable it only if you want packages from that channel. A package unavailable for your platform or a failed download is reported rather than substituted with another architecture.

If automatic update fails, download the appropriate release package from [GitHub](https://github.com/y114git/G3M/releases). Keep the separate G3M data directory when replacing application files.

## Mod updates

GameBanana-backed mods can be checked for available updates. **Settings > Library > Update Mods** controls automatic checks, their scope, and whether the update control is visible.

The scope can cover the selected game, active profile, or all profiles. **Update current versions directly** installs updates without creating the usual saved fallback version. Export or save a version first if you need a copy of the current mod.

An updated mod may target a different game release. Read its requirements and recheck dependent addons before launching a combined selection.
