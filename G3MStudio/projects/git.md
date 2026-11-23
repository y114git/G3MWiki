# Git panel

The Git panel runs the `git` program on the folder of the open project. It stages, commits, switches branches and talks to one remote. This page lists what it does and what it leaves to other Git tools.

## Requirements

- The file must have a [project](overview.md). Without one the panel says "Git tracks the project folder. Enable the project for this data file to use Git."
- `git` must be installed and found on the `PATH`. Otherwise: "Git is not installed or not on the PATH." Only absolute `PATH` entries count, so a program planted in the current folder is never run.
- The project folder must be a repository. Otherwise the panel says "This project is not a Git repository." and shows **Initialize Repository**.

Open the panel with **View > Panels > Git**.

## First commit and push

1. Enable a project for the open file (see [The Project menu](project-menu.md#enable-a-project)).
2. Open **View > Panels > Git** and press **Initialize Repository**.
3. Press **Stage All**, type a **Commit message** and press **Commit**.
4. Enter the **Remote address (URL or folder)**, press **Add Remote**, then **Push**.

Sync the project first (**Ctrl+S**) so the folder holds your latest edits. A teammate who pulls changes into the project folder then uses **Project > Resolve Outside Changes...** to take them into the data file.

## Controls

| Control | What it does |
|---|---|
| **Initialize Repository** | Runs `git init` and writes `.gitignore` and `.gitattributes` when they are missing. Existing files are never replaced. |
| **Branch** (list) and **Switch Branch** | Pick a branch, then press **Switch Branch**. Picking alone changes nothing. With uncommitted tracked changes a dialog **Switch branch** asks "There are {count} uncommitted changes. Switch to '{branch}' anyway?" |
| **New branch name** and **New Branch** | Creates and switches to a branch. |
| **Fetch** | `git fetch --all --prune`. |
| **Pull** | `git pull --ff-only`. A pull that needs a merge fails and shows git's message; nothing is merged behind your back. |
| **Push** | Pushes the current branch. A branch without an upstream is published to the first remote and tracked. |
| **Remote address (URL or folder)** and **Add Remote** | Shown only while the repository has no remote. Adds a remote named `origin`. |
| Ahead and behind text | "{ahead} ahead, {behind} behind" compared with the upstream. |
| Changes list | One line per file: two status letters (staged, working tree) and the path. `?` is an untracked file. |
| **Stage All** and **Unstage All** | Stage or unstage every file. |
| **Refresh** | Reads status and log again. |
| **Commit message** and **Commit** | **Commit** is on when the message is not empty. After a successful commit the message is cleared. |
| Commit list | The latest 50 commits: short hash and subject. |
| Diff box | Shown under the list when you select a change. A new file shows "New file, not tracked yet." |

Double-click a change in the list to stage it, or to unstage it when it is staged. Fetch, Pull, Push and the ahead and behind text show only when a remote exists.

When there are no changes the panel says "No changes." Errors show as "Git: {reason}".

## What Git gets

The project folder is committed as it is. `.gitignore` written by **Initialize Repository** contains:

```
.g3mstudio/
build/
*.tmp
```

`.gitattributes` turns line-ending conversion off for `*.g3mproj`, `*.json`, `*.gml`, `*.vsh` and `*.fsh`, so a clone with `core.autocrlf=true` rebuilds the same data file, and marks `*.png`, `*.ogg`, `*.wav`, `*.mp3`, `*.ttf` and `*.otf` as binary.

When the folder holds audio or image files (`.png`, `.ogg`, `.wav`, `.mp3`) of 10 MB or more, the panel says "Large audio or image files found: {count}, for example {first}. Git LFS keeps the repository small." The panel does not set up LFS.

## Safety

- Arguments are passed as a list, never through a shell, and git never prompts for input (`GIT_TERMINAL_PROMPT=0`). A remote that needs a password fails instead of waiting. Use a credential helper or an SSH key.
- `fsmonitor` is turned off, so a project folder from someone else cannot start a program from its config.
- Commands that only read do not take the index lock, so the panel can refresh while you commit.
- Commits use your Git identity from your Git configuration.

## Not supported in the panel

Clone, merge, rebase, stash, discard of changes, history browsing beyond the latest 50 commits, tags and conflict resolution. Use the `git` command or another Git program on the project folder for these.

## Troubleshooting

| Message | Fix |
|---|---|
| "Git is not installed or not on the PATH." | Install Git and restart G3MStudio. |
| "Git: No remote is set. Add one first." | Enter a remote address and press **Add Remote**. |
| "Git: ..." after Pull | The branches diverged. Merge or rebase outside G3MStudio. |

## See also

- [Projects and Project mode](overview.md)
- [Snapshots](snapshots.md)
- [Import a UTMT project](utmt-import.md)
