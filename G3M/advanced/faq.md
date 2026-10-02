# FAQ

## Can I select several mods?

Yes. Put compatible changes in one step or place addons in a later step when they require another mod's output. [Priority & Steps](../mods/modpacks.md) describes the distinction. A patch applying successfully does not prove that every gameplay interaction is compatible.

## Why is a supported game absent from Mods Browser?

Browsing requires a GameBanana ID, while local Library support does not. Check the [supported-games table](../games/README.md) or the custom entry's ID.

## Does changing profiles separate game saves?

No. Profiles separate mods and selection state. Game paths and user-data paths are shared settings. Use a save-management plugin or separate game/user-data locations where the game supports them.

## Why did a mod remain applied after closing the game?

Check the launch mode. Launch and keep changes and Patching only retain changes deliberately. Normal Launch can also report a restoration conflict if files changed externally. Preserve recovery data and inspect the log.

## Does a downloaded mod immediately become active?

No. Import puts it in the Library; **Use** includes it in the selection. **Do not install downloaded files automatically** leaves the archive waiting for you to install it from Downloads.

## Why does an imported duplicate not replace the current mod?

Choosing Merge stores the incoming package in Mod Versions. Open that list and Switch to the saved package. Replace has a different effect: it installs over the current mod.

## Can I use a theme offline?

An installed theme can be applied locally. Loading the hosted theme list and downloading a package require internet access. Install and Apply are separate Catalog actions.

## Does deleting a theme reset its appearance?

No. Deletion removes the saved package. Appearance already copied into the current settings remains until you change or reset it.

## Can the web editor install into G3M automatically?

It can save a ZIP and open a `g3m://` local-file import link after you provide that ZIP's full local path. The browser cannot determine an arbitrary Downloads path or write directly into G3M's profile folders.

## Which backup do I need?

Use Mod Versions for one mod, profile export for one Library setup, Game Versions for game files, and a data-directory copy for the whole G3M setup. Back up saves outside those locations separately.
