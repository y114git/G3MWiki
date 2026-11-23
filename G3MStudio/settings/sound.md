# Sound settings

The **Sound** category sets how sounds play inside G3MStudio (the sound view and the audio editor): the backend, the output device, the start volume and the buffer size. The values are read when a player starts to play.

## Controls

| Label | Control | Default | Range | Saved as |
| --- | --- | --- | --- | --- |
| **Sound backend** | List | **OpenAL Soft (in the program)** | **OpenAL Soft (in the program)**, **System player**, **Off (no sound)** | `audio.backend` (`openal`, `system`, `off`) |
| **Output device** | List | **Default** | **Default** and every device the backend lists | `audio.device` (empty is the system default) |
| **Start volume of the player (percent)** | Number | 100 | 0 to 100, step 5 | `audio.volume` |
| **Buffer size (milliseconds)** | Number | 100 | 20 to 500, step 10 | `audio.bufferMs` |

- Choosing another backend rebuilds the page so the device list belongs to it.
- A device that is unplugged stays in the list and stays chosen.
- When the chosen backend cannot start, a note says "No sound library was found ({file}) ..." with the library file name that is missing. See [Playback settings](../audio/playback-settings.md).
- A shorter buffer reacts faster to seeking. A longer buffer survives a busy machine.

## Values kept by the players

These are in the `audio` object of [settings.json](settings-file.md) and have no control here:

| Key | Default | Meaning |
| --- | --- | --- |
| `captureDevice` | empty | Microphone of the recorder. A name that is gone falls back to the default without being forgotten. |
| `visualizer` | true | Level meter and spectrum under the waveform. The toggle in the player changes it. |
| `waveformHeight` | 0 | Waveform height in pixels (40 to 800) after you drag a player to it. 0 keeps the built-in height. |

## Restore defaults

Resets the whole `audio` object.

## See also

- [Playback settings](../audio/playback-settings.md)
- [Player and waveform](../audio/player-and-waveform.md)
- [Default values](defaults-table.md)
