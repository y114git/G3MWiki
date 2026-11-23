# Formats, import and export

G3MStudio reads WAV, OGG Vorbis and MP3 and stores WAV or OGG Vorbis in the data file. This page covers Replace Audio, making sounds from files, Export Audio and Export All Audio.

## Formats

| Format | Read (play, edit, import) | Stored in a data file |
|---|---|---|
| WAV | Yes. 8 bit and 16 bit are read directly; 24 bit and float files are read as 16 bit. | Yes, as 16 bit |
| OGG Vorbis | Yes | Yes |
| MP3 | Yes | No: converted to OGG |

The format is recognised from the first bytes, not the file name. Decoding is managed code inside the program, so no other software is needed on any platform. A sound is limited to about 64 million samples when decoded (about 25 minutes of stereo at 44.1 kHz) and fails with `The audio is too long to edit.` The player shows the length of an MP3 only after decoding.

## Replace Audio

1. Select a sound.
2. Choose **Sound** > **Replace Audio...**
3. In the file dialog (`Choose a WAV or OGG file`) pick a WAV, OGG or MP3 file.

The file is converted to the format the sound stores: a sound that holds WAV gets a WAV, any other sound gets OGG. A file that already has that format is kept as it is, except a WAV deeper than 16 bit, which is stored as 16 bit with the warning `{sound} was stored as 16 bit audio: GameMaker plays 16 bit WAV, and the file had more bits.` The sound length updates. The notification reads `Audio of {sound} replaced ({kb} KB).` It is one undo step.

You can also drop one audio file on a sound tab. Dropped anywhere else (one or more files), WAV, OGG and MP3 files make new sounds, pictures make sprites and GML files make scripts. **File** > **Import Pictures, Sounds and Scripts from Files...** does the same from a file dialog. A new sound from a file takes the file name as its resource name. MP3 becomes OGG; a WAV deeper than 16 bit becomes 16 bit.

### Warnings

A warning asks before the audio goes in when:

- The file is larger than 64 MB (`Put in a very large audio file anyway?`). Large sounds use much memory and the game can fail to load them.
- The audio is of another kind than the sound holds (`Put in audio of another kind anyway?`). The sound keeps its old type and flags.

Undo takes the change out until you save. A refused or unreadable file shows `'{name}' cannot be used: {reason}`.

### Embedded audio and included files

The **Embedded Audio** entries and audio files (`.ogg`, `.wav`, `.mp3`) in **Included Files** open in a tab with a player and two buttons, **Replace Audio...** and **Export Audio...**. Replace works the same way and writes one undo step. For an included file, the bytes are written to the included files folder as they are (after conversion to the same kind when it holds WAV or OGG). A warning, `Write the audio into the game folder?`, explains that the data file does not hold this change.

## Export Audio

1. Select a sound that holds audio in the data file.
2. Choose **Sound** > **Export Audio...**
3. Pick a name in `Save the audio as`.

The stored bytes are written unchanged, with the extension of the format (`.wav`, `.ogg` or `.mp3`). The notification reads `Audio of {sound} saved to {path}.` A sound whose audio is outside the data file shows `This sound keeps its audio outside the data file ({file}).` Use Export All Audio for those.

## Export All Audio

**Sound** > **Export All Audio...** asks for a folder and runs with progress and Cancel. It writes one file per sound, named like the sound, with the format extension. It also reads sounds kept in the `audiogroupN.dat` files beside the data file.

- Characters not allowed in file names become `_`.
- Names that collide (same name apart from case or invalid characters) get `_2`, `_3` and so on.
- A sound without audio is skipped with `No audio found.` Other failures are listed with their reason.

## See also

- [Sounds and the sound tab](sounds.md)
- [Storage and audio groups](storage-and-groups.md)
- [Guide: replace a sound](guide-replace-sound.md)
- [Troubleshooting](troubleshooting.md)
