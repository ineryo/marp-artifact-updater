# Architecture

## Responsibility boundary

Marp Artifact Updater is the **materialization** step in a presentation workflow:

```text
artifact generation → explicit-region materialization → Marp rendering
```

It reads already-produced repository artifacts and refreshes only explicit generated regions in a Marp Markdown file. It does not own the upstream analysis/build pipeline and it does not render slides.

## Runtime layers

- `marp_artifact_updater.cli` owns argument parsing, human-readable output, JSON output, and exit status.
- `parser` recognizes explicit include-region syntax.
- `snippets` reads named source intervals, including saved notebook code cells without executing them.
- `paths` confines document and artifact paths to the declared repository root.
- `updater` resolves regions, reports staleness, and performs an atomic replacement only when `update --apply` is explicit.
- `handlers` render the bounded supported artifact kinds.

The normal execution boundary is intentionally small: the updater directly invokes no shell and notebooks are parsed, not run. `python-call` is a separately explicit path that requires an exact command-line module allowlist; the allowlisted module remains responsible for its own behavior.

## Ownership model

Human-authored slide prose, structure, and all text outside recognized regions remain outside the updater's authority. A generated region is a projection of its declared artifact source; a `check` can report it stale without taking authority to regenerate that artifact.

See [the safety model](safety.md) for the enforced write, path, and execution controls, and [include blocks](include-blocks.md) for the public region grammar.
