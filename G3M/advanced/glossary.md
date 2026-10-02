# Terms

| Term | Meaning in G3M |
| --- | --- |
| DATA | A GameMaker resource file, commonly `data.win` or `game.unx` |
| Mod package | Configuration plus bundled payload and support files |
| Operation | One configured patch, copy, extraction, or information entry |
| Priority | Which mod wins an unmergeable overlap within a step; higher rows have higher priority |
| Step | A group applied against one starting state; later steps use preceding results |
| Profile | A separate mod library and launch selection |
| Mod version | A saved copy of one mod's files |
| Game version | A saved snapshot of game installation files |
| User-data folder | The game's per-user files, separate from its installation |
| Placeholder | A named path root such as `${game_path}` or `${game_data_path}` |
| Virtual archive path | A destination inside an archive, such as `${game_path}/assets.zip/textures/menu.png` |
| Launch restoration | Returning tracked session files to their backed-up state after a normal launch |
