# Audio

**Settings > Appearance > Audio** has separate controls for background music and the startup sound. Each control selects a file or removes the active custom file.

Supported extensions are `.mp3`, `.wav`, `.ogg`, `.flac`, `.m4a`, and `.aac`. The actual file must use a codec available to the platform's media playback.

## Startup sound

The startup sound plays when G3M starts. **Disable startup sound** suppresses it without removing the custom file. Removing the file and disabling playback are separate actions.

A theme names this file `startup_sound.<extension>`. It is included in theme export when present.

## Background music

Background music repeats while G3M is running. **Stop music when unfocused** stops it when G3M loses focus or is minimized. Returning to the application starts the track from its beginning; it does not resume the previous playback position.

Removing the custom music file stops using that track. Background music is tied to G3M's lifetime and stops when G3M exits, including when its main process terminates unexpectedly.

A theme names this file `background_music.<extension>`. The file is exported with the theme. The unfocused-playback preference is an application setting rather than a standard theme-export field.
