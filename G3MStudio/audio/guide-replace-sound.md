# Guide: replace a sound

This guide puts your own audio file into an existing sound, checks it and, if needed, trims it. Result: the sound plays the new audio in the game and Undo can bring the old one back.

## Steps

1. Open the data file and double-click the sound (for example `snd_jump`) under **Sounds**.
2. Press **Play** to hear the old audio.
3. Choose **Sound** > **Replace Audio...** and pick a WAV, OGG or MP3 file.
4. Wait for the notification `Audio of snd_jump replaced (NN KB).`
5. Press **Play**. The Info line shows the new size and length.
6. To trim: choose **Sound** > **Edit Audio...**, set **Start (seconds)** and **End (seconds)** (or drag on the waveform), then press **Apply**.
7. Save the file.

Shortcut: drop the audio file on the open sound tab.

## Notes

- The file is converted to what the sound stores (WAV or OGG). An MP3 never stays MP3.
- A sound kept in an audio group file cannot be replaced here (see [Storage and audio groups](storage-and-groups.md)).
- To undo, use Undo before you save.

## See also

- [Formats, import and export](formats-import-export.md)
- [Audio editor](audio-editor.md)
- [Troubleshooting](troubleshooting.md)
