# Storage and audio groups

A sound can keep its audio inside the data file or outside it. This page explains the flags, the audio group files, which cases the player can play, and the Audio Groups window.

## Where the audio lives

| Case | How it shows | Play | Edit and replace |
|---|---|---|---|
| Embedded in the data file | Info line shows size and length | Yes | Yes |
| In an audio group file (`audiogroupN.dat` beside the data file) | `This sound keeps its audio outside the data file. It plays from {file} (read only).` | Yes, read only | No: replace shows `'{sound}' keeps its audio outside the data file, so there is nothing here to replace.` |
| Streamed file beside the data file (named in the sound's File) | Same line | Yes, read only; a name without an extension also tries `.ogg` | No |
| File missing | `This sound keeps its audio outside the data file ({file}).` | No | No |

Paths are looked up only inside the folder of the data file. The audio group file name comes from the audio group's path, or `audiogroupN.dat` by default (N is the Group ID). Group file entries are read when needed.

Opening an `audiogroupN.dat` file in the open-file dialog works on its own and lists its audio entries.

## Flags

The Storage line of a sound shows its flags.

| Flag | Meaning |
|---|---|
| `IsEmbedded` | The audio is in the data file ("not streamed"). Best for effects, not for music. |
| `IsCompressed` | Smaller on disk and in memory, a little more CPU to play. |
| `IsDecompressedOnLoad` | Decompressed when the game loads, so it starts without delay. |
| `Regular` | Uses the new audio system, the default for modern games. |

Replace Audio keeps the flags. A sound made from a file, or generated audio of another format than the sound holds, changes Type, File and the compressed flag with the format (OGG sets compressed, WAV clears it).

## Embedded Audio

**Embedded Audio** in the Explorer lists the raw audio entries of the data file. Several sounds can play one entry. Each entry opens in a tab with a player, **Replace Audio...** and **Export Audio...**. Replacing an entry updates the length of every sound that plays it.

## Audio Groups window

Audio groups let a game load sound sets separately. Open **Tools** > **Audio Groups...** (needs an open file). The window is titled `Audio Groups`.

| Control | Action |
|---|---|
| Groups | List of audio groups. Pick one to see its members (`Pick a group to see its members.`). |
| Members | The sounds in the picked group. `This group has no members of this kind.` when empty. |
| Group name / **New group** | Creates a group (`Group {name} created.`). |
| **Rename group** | Renames the picked group (`Group renamed to {name}.`). |
| **Move picked to** / **Move** | Moves the picked members to the chosen group (`Moved {count} to {group}.`). |

A name that is not a plain name (letters, digits and underscores, not starting with a digit) asks `Use this name?` first, since GML code cannot use it. `This data file has no groups.` shows when the file has none. Every change is one undo step.

You can also select sounds in the Explorer and choose **Organize** > **Move to Audio Group...**, pick a group in `Move to audio group`, and confirm. The notification reads `Moved {count} entries to {group}.`

## See also

- [Sounds and the sound tab](sounds.md)
- [Formats, import and export](formats-import-export.md)
- [Glossary](../reference/glossary.md)
