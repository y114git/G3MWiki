# Graphical application

Choose an **Operation** at the top of the form. The input labels, options, and run-button label adapt to that operation. **Clear** resets the form; it does not delete generated output files.

## Inputs and outputs

| Operation | Fields to provide |
| --- | --- |
| Create a patch | Original DATA, modified or supported input, patch output |
| Apply a patch | Original DATA, patch input, DATA output |
| Validate a patch | Patch; optional original DATA for compatibility checking |
| Merge patches | Original DATA, at least two inputs, optional patch and DATA outputs |
| Compare files | First file, second file, Markdown report destination |
| Inspect a file | DATA or patch; output appears in Activity |
| Create XDelta patch | Original and modified files, binary patch output |
| Apply XDelta patch | Original file, binary patch, output file |
| Run XDelta arguments | One xdelta argument per line |
| Run a script | Script, optional DATA, output DATA when DATA is provided, optional arguments |
| Run a program | Program and one argument per line |

**Browse** selects the corresponding file or folder. **Add files** appends inputs to a list. Additional-input and argument fields use one item per line.

For merge, arrange inputs **lowest priority first**. When a DATA output is supplied, the result is applied there. The optional merged patch output retains the combined package. With no DATA output, provide a patch destination.

## Options

- **Batch mode** changes Create, Apply, or Merge into independent repeated jobs against the same original. Create and Apply use an output directory. Merge takes one comma-separated low-to-high set per line, with a DATA output directory and optional patch directory.
- **Use XDelta fallback** includes a binary fallback during resource-patch creation or tries the binary path during application.
- **Create XDelta output** produces binary patches in Create's batch mode. It cannot be combined with fallback embedding. For a single binary patch, choose **Create XDelta patch** as the operation.
- **Continue after errors** allows later batch jobs to run after a failure.
- **Merge GML code** attempts to combine overlapping code edits.
- **Merge JSON properties** combines supported property edits.
- **Low-memory merge** is available for a single merge. It lowers memory use and cannot combine with the GML or JSON merge options. Inputs still target the same original.
- **Write detailed report** requests detailed comparison or merge reports for operations that support it.

**Cache folder** is optional and stores reusable analysis. It does not hold the source game's complete files.

Options appear only for operations that support them. Leaving the merge checkboxes unchecked resolves unmergeable overlaps by priority. Enabling a merge option attempts to combine the corresponding edits; inspect the report when a conflict remains.

## Activity

The progress bar and messages report the current operation. **Expand** enlarges the log view, **Save** writes its text to a file, and the Activity **Clear** action clears displayed log content without removing results.

The run action is unavailable during a job. Closing the window while working waits for the operation before closing; it is not a Cancel button. Avoid terminating the process while outputs are being written.

Script operations can display questions, text input, and file or directory dialogs. Program arguments are passed directly, without shell interpretation. Only run trusted code.
