# Platforms

G3MStudio runs on Windows, Linux and macOS, on x64 and arm64. This page lists what differs between the systems.

## Supported systems

| System | Architectures | Package |
| --- | --- | --- |
| Windows | x64, arm64 | zip with `G3MStudio.exe` |
| Linux | x64, arm64 | zip with `G3MStudio` |
| macOS | x64, arm64 | zip with `G3MStudio.app` |

The program is built on .NET 10 and Avalonia 12. Each build is tested on its own runner for every system and architecture.

## Differences

| Topic | Windows | Linux | macOS |
| --- | --- | --- | --- |
| First start | SmartScreen: **More info**, **Run anyway** | `chmod +x G3MStudio`; needs `libfontconfig` and X11 or Wayland | Right-click **Open**; `xattr -dr com.apple.quarantine G3MStudio.app` |
| Data folder | `%APPDATA%\G3MStudio` | `~/.config/G3MStudio` | `~/Library/Application Support/G3MStudio` |
| Default program for data files | Offered in the welcome window and Settings | Offered in the welcome window and Settings (`.desktop` and MIME type) | Not offered; set it in Finder. G3MStudio is listed under **Open With**. |
| "Open with" entries | Registry, current user | `g3mstudio.desktop` | `Info.plist` document types |
| Explorer verbs **Run game with G3MStudio** and **Run game with G3MStudio, pick a runtime** | Added with the registration | No | No |
| Updater script | `.cmd` | `.sh` | `.sh` |
| Running a game | Runs the game's executable | Settings > Run: Wine, Proton, umu, PortProton or your own command | Same as Linux |

Running a game on Linux and macOS is tested only with simulated setups, not on real machines.

The program is not signed or notarized on any system.

## See also

- [Install](../getting-started/install.md)
- [Command line](../getting-started/command-line.md)
- [Files and folders](files-and-folders.md)
