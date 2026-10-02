# diff

```text
G3MTool diff <first> <second> [output-directory]
    [--full] [--cache <directory>] [--json]
```

Inputs are GameMaker DATA files or resource patch archives. Diff writes `diff_<timestamp>.md` in the requested directory, which defaults to `diff/` beside the executable.

The standard report summarizes resources and changed-file lists. **Full** adds unified text, GML, and JSON differences and detailed texture-page, reference, and asset-order comparisons. It can take longer and produce a much larger report.

```text
G3MTool diff original.win modified.win reports --full --cache cache
```

**JSON** prints the command result and report location in machine-readable form; the generated report remains Markdown. A successful comparison returns 0 whether or not differences exist. Diff is not a byte-equality exit-code test.

Patch-to-patch comparison describes their declared and packaged resource changes. It does not prove that both patches run successfully on a particular original file.
