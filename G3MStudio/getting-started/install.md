# Install

G3MStudio ships as a portable zip for six systems. There is no installer: unpack the zip into a folder you own and start the program from there.

## Choose the zip

The file name is `G3MStudio-<OS>-<arch>-<version>.zip`.

| System | Zip name part | Program inside the zip |
| --- | --- | --- |
| Windows, Intel or AMD 64 bit | `Windows-x64` | `G3MStudio.exe` |
| Windows on ARM | `Windows-arm64` | `G3MStudio.exe` |
| Linux, 64 bit Intel or AMD | `Linux-x64` | `G3MStudio` |
| Linux on ARM | `Linux-arm64` | `G3MStudio` |
| macOS, Intel | `macOS-x64` | `G3MStudio.app` |
| macOS, Apple silicon | `macOS-arm64` | `G3MStudio.app` |

Every zip also holds `LICENSE`, `THIRD-PARTY-NOTICES.txt` and a short readme. The program is one file.

## Install steps

1. Unpack the zip into a folder you can write to, for example `Documents\G3MStudio` or `~/Apps/G3MStudio`. Do not use `C:\Program Files`: the in-app updater writes next to the program (see [Updating](updating.md)).
2. Start the program (see the system sections below).
3. Work on a copy of your game. G3MStudio changes game files and can damage them if something goes wrong.

## Windows

The program is not signed. SmartScreen may block the first start. Click **More info**, then **Run anyway**.

## macOS

The app is not signed or notarized, so Gatekeeper blocks it after a download.

1. Right-click `G3MStudio.app` and choose **Open**, then **Open** again.
2. If macOS still refuses, remove the quarantine flag in a terminal:

```sh
xattr -dr com.apple.quarantine G3MStudio.app
```

The app bundle holds `Contents/MacOS/G3MStudio`, `Contents/Info.plist` and the icon. The program declares the data file types, so G3MStudio appears in the Finder **Open With** list. Making it the default program is done in Finder.

## Linux

1. Make the file runnable:

```sh
chmod +x G3MStudio
```

2. The program needs `libfontconfig` and an X11 or Wayland session.
3. Optional: **Open GameMaker data files (.win, .unx, ...) with G3MStudio** in the welcome window (see [First launch](first-launch.md)) registers the data file types through a `.desktop` file and the MIME type `application/x-gamemaker-data` in your user folders.

## What the build checks

The release build starts the program from each zip with `--smoke`, which opens the window, waits 2 seconds and exits with code 0 (see [Command line](command-line.md)). The six systems are built and checked on their own runners.

## Troubleshooting

| Symptom | Fix |
| --- | --- |
| SmartScreen blocks the start | **More info**, then **Run anyway** |
| macOS says the app cannot be opened | Right-click, **Open**; or run the `xattr` command above |
| Linux: `Permission denied` | `chmod +x G3MStudio` |
| Linux: the window does not appear | Install `libfontconfig` and start from an X11 or Wayland session |

## See also

- [First launch](first-launch.md)
- [Updating](updating.md)
- [Platforms](../reference/platforms.md)
- [Troubleshooting index](../reference/troubleshooting.md)
