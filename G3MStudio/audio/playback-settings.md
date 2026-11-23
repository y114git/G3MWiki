# Playback settings

The **Sound** section of the settings controls how sounds play inside G3MStudio (sound tab and audio editor). The recorder and the generator preview use the same output. Changes apply the next time a sound starts to play.

## Options

| Label | Default | Range | Notes |
|---|---|---|---|
| Sound backend | OpenAL Soft (in the program) | OpenAL Soft (in the program), System player, Off (no sound) | See below. |
| Output device | Default | The devices of the backend | `Default` follows the system. A device that is unplugged stays chosen until you change it. |
| Start volume of the player (percent) | 100 | 0 to 100, step 5 | The volume slider of each new player starts here. |
| Buffer size (milliseconds) | 100 | 20 to 500, step 10 | Shorter reacts faster to seeking. Longer survives a busy computer. |

### Backends

| Backend | Behaviour |
|---|---|
| OpenAL Soft (in the program) | Plays in the program. The native library is `soft_oal.dll` on Windows, `libopenal.dylib` on macOS and `libopenal.so` on Linux, expected in the folder next to the program. Output device and buffer apply. |
| System player | Writes the audio to a private temporary folder (`g3mstudio-audio-...`) and opens it with the program your system uses for that file type. The sound does not play inside the window. |
| Off (no sound) | Nothing plays, and the system player is not used. |

If the OpenAL library cannot load, the section shows `The OpenAL library could not be loaded, so sound plays through the system player. Put {file} in the folder next to the program.` and sound goes to the system player. If no program can open the file, the sound tab shows `No program could play {path}. Open it yourself.` When no output device is found the editor shows `No audio output device was found, so the sound cannot play here.`

On Linux, if sound fails in the program, pick another Output device, or choose System player.

## Saved with the settings, without a control here

- The **Visualizer** checkbox of the player.
- The waveform height you dragged to (40 to 800 pixels).
- The microphone chosen in the [recorder](recording.md).

## See also

- [Player controls and waveform](player-and-waveform.md)
- [Settings](../settings/README.md)
- [Troubleshooting](troubleshooting.md)
