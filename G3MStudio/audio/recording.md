# Recorder

The recorder takes audio from a microphone and turns it into a new sound or the audio of the selected sound. Nothing is recorded until you press Record. Recording is 44100 Hz, mono, 16 bit WAV.

## Open it

**Sound** > **Record Sound...**, a right-click on the **Sounds** row in the Explorer, or the **Record Sound...** button in the [audio editor](audio-editor.md) (there it only replaces that sound). The window is titled `Record sound`.

## Controls

| Control | What it does |
|---|---|
| **Microphone** | Lists the microphones. The system default is marked `{name} (default)`. Your choice is remembered; picking the default keeps "follow the system default". A remembered microphone that is unplugged stays remembered and is used again when it returns. |
| **Refresh** | Looks for microphones again, for example after you plug one in. |
| **Input gain (percent)** | Slider 0 to 400, default 100. Applies while recording. Above 100 percent a loud sound can clip. After the recording the slider is off: `The gain applies while recording. Record again to use another gain.` |
| **Start after a 3 second countdown** | Waits three seconds after you press Record (`Recording starts in {seconds}...`). Stop cancels it. |
| **Record** / **Record again** | Starts. Record again discards the take after a confirmation. |
| **Pause** / **Resume** | Pauses and continues the same take. |
| **Stop** | Ends the recording or the countdown. |
| Level meter | Peak and RMS, left and right, while recording. |
| Red dot, time | `{time} recorded, {size} MB`. |

Controls that cannot be used while recording say so in their tip: `This cannot be changed while a recording is in progress.`

If a sample reaches the loudest possible level the window shows `Clipping: the sound reached the loudest possible level and is distorted. Lower the input gain or move away from the microphone, then record again.`

While the microphone opens, the program stops any sound it plays, so it does not record itself.

## After recording

The take shows in the player with its waveform. Listen to it, then drag on the waveform to choose the part to keep. The line `Keeps {seconds} of {total} seconds, 44100 Hz, mono, 16 bit WAV` shows the result.

| Button | Action |
|---|---|
| **Create new sound** | Makes a new sound from the name field (suggested `snd_recorded`). Not shown when opened from the audio editor. |
| **Replace audio of {sound}** | Puts the take into the selected sound, as one undo step. |
| **Cancel** | Closes. |

Both need a take (`Record a sound first.`). The region is the part saved, otherwise all of it. The take lives in memory only. Closing with an unused take asks first: `The recording is not in a sound yet. If you go on, it is lost.`

## Limits

- No time limit. A take stops at about 6.7 hours (the largest 16 bit WAV size), with `The recording reached the longest take that can be stored and stopped. What was recorded is kept.`
- It stops earlier when memory runs low: `The recording stopped because memory is running low. What was recorded is kept.`
- Nothing recorded: `Nothing was recorded. Check that the microphone works and press Record again.`

## Problems

| Message | Fix |
|---|---|
| `The OpenAL library could not be loaded, so the microphone cannot be used. Put {file} in the folder next to the program.` | Library file: `soft_oal.dll` (Windows), `libopenal.dylib` (macOS), `libopenal.so` (Linux). |
| `No microphone was found. Connect one and press Refresh.` | Connect one, press Refresh. |
| `The microphone could not be opened. Another program may be using it. Stop that program and try again.` | Close the other program. |
| `The system does not allow this program to use the microphone. Allow it in the privacy options of your system, then press Refresh.` | On Windows this reads the privacy setting for desktop apps. Other systems refuse at open. |
| `The microphone stopped working. The recording up to that moment is kept.` | Unplugged or lost. The take so far is kept. |

## See also

- [Guide: record a sound](guide-record-sound.md)
- [Player controls and waveform](player-and-waveform.md)
- [Troubleshooting](troubleshooting.md)
