# Player controls and waveform

The same player appears in the sound tab, the audio editor, the sound generator and the recorder. This page lists every control, the region, zoom, the visualizer and the keys.

## Controls

| Control | What it does |
|---|---|
| **Play** / **Pause** | Plays from the seek bar position, or from the region start when a region is set. While playing the button reads **Pause**. |
| **Stop** | Stops and sets the position back to 0. |
| **Play region** | Plays only the region. With Loop on, the region repeats. Shown where a region is available. |
| **Clear region** | Removes the region, so the whole sound plays again. |
| **Loop** | Plays again from the start when the end is reached: the region when one is set, otherwise the whole sound. |
| **-**, **+**, **Fit** | Zoom out, zoom in, show the whole sound. |
| **Visualizer** | Shows or hides the level meter and spectrum. |
| **Volume** | Slider, 0 to 100. Starts at the value set in [Settings > Sound](playback-settings.md). |
| Seek bar | Drag to move the position. While you drag, only the cursor and time move; the sound continues when you let go. |
| Time line | `0:01.2 / 0:03.5` and, with a region, `[0:00.5 - 0:02.0]`. Format `m:ss.f` (`h:mm:ss.f` from one hour). |

Only one player plays at a time. Starting another stops the first.

Playing needs an output device. Without one, or when the audio cannot be decoded, the sound goes to the system player (see [Playback settings](playback-settings.md)).

## Region

A region is a part of the sound. In the sound tab it only previews. In the audio editor it is the part that Apply keeps. In the generator and recorder it is the part that Create new sound and Replace keep.

Under the waveform, the fields **Region start and end (seconds)** hold the start and end. You can type values (steps of 0.05, two decimals, up to 100000). A change applies at once, also while the sound plays. A region needs an end after its start.

On the waveform:

| Action | Result |
|---|---|
| Click | Moves the playback position. |
| Drag | Selects a region. |
| Drag an edge of the region | Moves that edge. |
| Drag the middle of the region | Moves the whole region. |
| `Shift`+click | Moves the nearest edge to the pointer. |
| `Ctrl`+drag inside the region | Selects a new region. |
| Double-click | Removes the region. |
| `Esc` while dragging | Cancels the selection. |

Ends are rounded to hundredths of a second. The ends of the sound stay exact. The cursor changes over an edge or the middle of a region.

## Zoom and scroll

- `Ctrl`+wheel zooms at the pointer. `+` and `-` zoom at the cursor, or at the middle of the view when the cursor is out of view.
- The wheel scrolls while zoomed in. A scroll bar appears under the waveform. A horizontal wheel or a touchpad swipe scrolls it too.
- The smallest view is about 16 frames of the sound. The tips say `Already zoomed in as far as it goes.`, `The whole sound is already shown.` or `The audio is still loading.` when a zoom button cannot act.
- While playing, the view follows the cursor until you scroll it yourself. The next play follows again.

## Waveform size

The waveform is 64 pixels tall by default. Stereo sounds draw in two lanes when the waveform is at least 100 pixels tall. Drag the handle under the waveform to change the height (40 to 800 pixels). Every player opens at the last height you set.

## Visualizer

The visualizer shows a meter and a spectrum while the sound plays. Left bars: peak (line) and RMS (bar) of the left and right channel. Right bars: spectrum on a logarithmic axis from 40 Hz to 16000 Hz (limited to half the sample rate), 4 to 64 bars depending on width. A scale line is drawn every 20 dB down to -80 dB. After a stop or pause the bars fall to zero. Hidden, it does no work. The on/off choice is saved.

## Shortcuts

The keys work when the player has focus and you are not typing in a text or number box.

| Key | Action |
|---|---|
| `Space` | Play or pause |
| `L` | Toggle Loop |
| `R` | Rewind to 0 |
| `Left` / `Right` on the waveform | Move the region edge you used last by 0.01 s |
| `Shift`+`Left` / `Right` | Same, 0.1 s |
| `+` / `-` | Zoom in / out |

The edge moved by the arrow keys is the one you last grabbed or dragged (the end until then).

## See also

- [Sounds and the sound tab](sounds.md)
- [Audio editor](audio-editor.md)
- [Playback settings](playback-settings.md)
- [Troubleshooting](troubleshooting.md)
