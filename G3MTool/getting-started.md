# Getting started

Extract the release package for your system into a writable folder. Open the graphical application, or run the CLI from a terminal. Keep accompanying release files together.

Open a terminal in the extracted folder. Examples below use `./G3MTool` on Linux and macOS. In Windows PowerShell, replace that executable prefix with `.\G3MTool.exe`. If the executable is on `PATH`, the folder prefix is unnecessary.

## Create a resource patch

Keep a clean original and a modified copy from the same game release:

```text
./G3MTool patch create original.win modified.win interface.g3mpatch
./G3MTool patch validate interface.g3mpatch --data original.win
./G3MTool patch apply original.win interface.g3mpatch test-output.win
```

The create command records resource differences. Validation checks the patch and its compatibility with the supplied original. Apply writes the test result to a separate output.

Start the game using a separate test installation containing the resulting DATA file and any required external resources. Validation and successful writing are file checks, not a gameplay test.

## Compare the result

```text
./G3MTool info interface.g3mpatch
./G3MTool diff modified.win test-output.win reports --full
```

Diff writes a Markdown report under `reports/`. Resource patching can reconstruct an equivalent resource state without reproducing every byte of the modified input. Use the report and playtesting to assess the result; use an xdelta patch when exact byte reproduction is the requirement.

## Package for G3M

A `.g3mpatch` is a patch, not a complete Library mod. Put it into a mod package with `mod_config.json`, or import the patch into G3M and configure its destination. See [creating mods](../G3M/mods/creating-mods.md).

Use explicit output paths. Several commands default to files next to the executable, and apply can use the original file's basename there. An explicit separate output avoids accidentally choosing an original as your destination.
