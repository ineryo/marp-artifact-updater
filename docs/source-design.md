# Source design and implementation boundary

## Implemented behavior

The updater reconciles explicit generated regions in Marp Markdown from already-produced artifacts. Its supported vocabulary includes snippets, tables/dataframes, figures, quotations, equations, provenance, and opt-in Python calls.

The public contract keeps generation, materialization, and rendering separate:

```text
artifact generation → Marp Artifact Updater → Marp rendering
```

`check` and dry-run `update` are read-only; `update --apply` is explicit. The implementation preserves text outside recognized regions, confines paths under `--repo-root`, writes atomically, parses saved notebook content without running it, invokes no shell, and requires an exact allowlist for Python-call modules.

## Deliberate limits

This project is not a general template engine, notebook runner, shell task runner, research pipeline, or slide renderer. Staleness warnings can identify a declared dependency relationship without authorizing the updater to regenerate an artifact.

Future changes must preserve this auditable boundary unless a separately authorized product decision changes it.
