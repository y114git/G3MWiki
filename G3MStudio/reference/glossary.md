# Glossary

Terms used across the G3MStudio wiki, for readers new to GameMaker. Entries are alphabetical.

**Animation curve.** A named curve with points that a game reads to change a value over time.

**Audio group.** A set of sounds loaded together. Sounds of other groups may live in `audiogroupN.dat` files beside the data file.

**Backup slot.** A copy of the data file made when you save. Slot 1 is the newest. The count is set in Settings (default 3, up to 100).

**Chunk.** A section of the data file with a four letter name, such as `SPRT` for sprites. The hex view and chunk tools show them.

**Code entry.** One block of GML code stored in the data file: a script, an object event or a room creation code.

**Command palette.** The search window opened with `Ctrl+Shift+P` that lists every command.

**Crash report.** An anonymous report sent after an error when allowed in Settings > Privacy. A local crash log is always written to the `crashes` folder.

**Data file.** The single file that holds a compiled GameMaker game: `data.win`, `game.unx`, `game.ios`, `game.droid`.

**Edit journal.** A file in the data folder that records edits so unsaved work can be recovered.

**Event.** An action slot of an object, such as Create or Step, that holds code or actions.

**Explorer.** The panel that lists every resource of the open data file by type.

**Extension.** A GameMaker package that adds functions or files to a game.

**Font.** A resource with glyph pictures and metrics used to draw text.

**GML.** GameMaker Language, the scripting language of GameMaker games.

**Included file.** A file stored inside the data file for the game to read at run time.

**Inspector.** The panel that shows type, name and use count of the selected resource.

**Mod.** A set of changes to a game, shared as a patch, mod pack or mod ZIP.

**Mod base.** The stored original of a data file that Export Mod Pack compares your file with. See [Mod base](../settings/mod-base.md).

**Mod pack.** A `.g3mpack` file that holds several patches.

**Object.** A resource that defines a kind of game entity with events and a sprite.

**Particle system.** A resource that describes particle effects.

**Patch.** A `.g3mpatch` file that holds the differences between two data files.

**Path.** A resource that holds a route of points for instances to follow.

**Plugin.** A folder of C# source that adds commands, panels or code hints. See [Writing a plugin](plugin-api.md).

**Problems panel.** The panel that collects warnings and errors from loading, saving and compiling.

**Project.** A folder of resources linked to a data file for text editing and version control.

**Recent file.** An entry in the Start page list. It can be pinned.

**Resource.** One item of a game such as a sprite, sound, object, room or script.

**Room.** A level: a resource with a size, layers and placed instances.

**Run profile.** A named set of run options (runner, arguments, working folder, links, environment, compatibility tool). See [Run settings](../projects/run-settings.md).

**Runtime.** The GameMaker program that runs a game from its data file. **Run Game** starts it with your changes.

**Sequence.** A resource that animates sprites, sounds and other tracks on a timeline.

**Shader.** A small program that changes how graphics are drawn.

**Snapshot.** A saved state of a linked project that you can return to.

**Sound.** A resource that plays audio, stored in the data file or in an audio group file.

**Sprite.** A resource made of one or more picture frames.

**Texture page.** A large picture that holds many sprite frames packed together inside the data file.

**Tile set.** A resource that cuts a picture into tiles for room tile layers.

**Timeline.** A resource that runs actions at set moments.

**Unsaved edits.** Changes not yet written to the data file. Closing asks what to do with them.

**Wine, Proton.** Programs that run Windows games on Linux and macOS. **Settings > Run** can use them to run a game.

## See also

- [Files and folders](files-and-folders.md)
- [File formats](file-formats.md)
