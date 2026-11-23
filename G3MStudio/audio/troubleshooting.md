# Troubleshooting

Messages and fixes for sounds, the audio editor, the generator and the recorder. Messages are quoted as the program shows them.

## Playback

| Problem or message | Cause and fix |
|---|---|
| Nothing plays, no message | Backend is **Off (no sound)**, or the volume slider is 0. Check [Playback settings](playback-settings.md). |
| `No audio output device was found, so the sound cannot play here.` | No output device. Connect one, or choose another **Output device**. |
| `The OpenAL library could not be loaded, so sound plays through the system player. Put {file} in the folder next to the program.` | Put `soft_oal.dll`, `libopenal.dylib` or `libopenal.so` next to the program, or keep using the System player. |
| `No program could play {path}. Open it yourself.` | The system has no program for the file type. Open the temporary file yourself. |
| `This sound has no audio data to play.` | The sound has no audio in the data file. |
| `This sound keeps its audio outside the data file ({file}).` | Audio group or streamed file is missing beside the data file. Put the file there. |
| Sound stops when another tab plays | Only one player plays at a time. |
| Player shows `?` for length | The length of an MP3 is known after decoding. |

## Editing and import

| Message | Meaning |
|---|---|
| `Select a sound first.` | No sound selected or in front. |
| `Only a sound stored in the file as WAV, OGG or MP3 can be edited here. Select one, or use Replace Audio.` | The sound has no editable audio. |
| `'{name}' cannot be used: {reason}` | Unreadable or invalid file, or a write failure. |
| `The OGG file could not be read: ...`, `The MP3 file could not be read: ...` | Damaged or unsupported file. |
| `The audio is too long to edit.` | More than about 25 minutes of stereo. |
| `The audio file is empty.` | Zero-byte file. |
| `The gain must be between 0 and 1600 percent.` | Volume out of range. |
| `The cut would leave no audio.` | Start and end are too close. |
| `'{sound}' keeps its audio outside the data file, so there is nothing here to replace.` | See [Storage and audio groups](storage-and-groups.md). |
| `Put in a very large audio file anyway?` | Over 64 MB. |
| `Put in audio of another kind anyway?` | The file is not WAV or OGG, or differs from what the sound holds. |

## Generator and recorder

See the message tables in [Recorder](recording.md) and [Sound generator](synth.md). Common cases: `Enter a name for the new sound.`, `Record a sound first.`, `Nothing was recorded. Check that the microphone works and press Record again.`

## See also

- [Playback settings](playback-settings.md)
- [Formats, import and export](formats-import-export.md)
- [Recorder](recording.md)
