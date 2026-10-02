# info

```text
G3MTool info <data-or-patch> [--cache <directory>] [--verbose] [--json]
```

Info reads a GameMaker DATA file or a `.g3mpatch` archive, including a patch ZIP with `g3mpatch.json`.

The normal view reports metadata, counts, and short resource breakdowns. **Verbose**, also `-v`, requests per-resource listings. **Cache** reads and writes reusable DATA analysis. **JSON** emits structured information for another program to read.

```text
G3MTool info data.win --cache cache
G3MTool info interface.g3mpatch --json
```

Info does not apply a patch. To check compatibility with a specific original, run `patch validate <patch> --data <original>`.
