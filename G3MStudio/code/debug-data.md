# Debug Data

Debug data is the file the GameMaker Studio 1.4 debugger reads next to a game. G3MStudio can write it from the code of the open data file, so you can step through a game's code in that debugger. This page covers the command, the three source modes and the output file.

## Write Debug Data

Choose **Tools > Write Debug Data...** (needs an open data file).

1. Pending text in code tabs is applied first. If a tab does not compile, the command stops.
2. A question asks "What the source text of each code entry holds" with three choices.
3. G3MStudio shows "Writing debug data..." and writes the file.
4. The result reads "Written {file}." A failure reads "Debug data not written: {reason}".

The file is `{data file name}.yydebug` in the folder of the data file, for example `data.yydebug` beside `data.win`. An existing file of that name is replaced. It is built from the model as it is now, including edits you have not saved to the data file.

## The three choices

| Choice | Source text of each code entry | Debug info |
|---|---|---|
| **Decompiled GML** | The decompiled GML | One debug info pair per entry |
| **Assembler, marks at stores, jumps and returns** | The assembly | A pair at stores, jumps and returns |
| **Assembler, marks at every instruction** | The assembly | A pair at every instruction |

Use **Decompiled GML** to read code in the debugger as in the editor. Use an assembler choice to step instruction by instruction. An entry that cannot be decompiled gets its error as a comment, so the other entries are kept.

The debug data is produced for the GameMaker Studio 1.4 debugger only. Running a game from G3MStudio (`F5`) does not need it.

## See also

- [Hex View and Raw Chunks](hex-view.md)
- [Code Entries](code-entries.md)
- [Run the game](../projects/run-game.md)
