# Marp Artifact Updater

**Keep a hand-authored Marp deck synchronized with artifacts your project already produced.**

Marp Artifact Updater materializes selected source artifacts—such as code snippets, CSV tables, figures, equations, quotations, and provenance—into small, explicit regions of a Marp Markdown deck. It is for the gap between producing an artifact and rendering a presentation:

```text
analysis, notebook, benchmark, or source code
                    ↓
      saved CSV, SVG, image, or snippet
                    ↓
          Marp Artifact Updater
                    ↓
          hand-authored Marp Markdown
                    ↓
                Marp CLI / renderer
```

It does **not** run the research pipeline or render slides. It refreshes only the regions you explicitly delegate to it.

## First success: a bounded Marp update

The clean [`examples/quickstart/`](examples/quickstart/) deck is ready to check. From a repository checkout:

```console
uv sync --group dev
uv run marp-artifact-updater check deck.md --repo-root examples/quickstart
```

That check is read-only and exits `0`: the managed snippet already matches its source. To see an update, change the string in `examples/quickstart/source.py`, then run:

```console
uv run marp-artifact-updater check deck.md --repo-root examples/quickstart
uv run marp-artifact-updater update deck.md --repo-root examples/quickstart --apply
uv run marp-artifact-updater check deck.md --repo-root examples/quickstart
```

The first command reports the pending change without writing. `--apply` updates only the body between the `snippet-include` comments; the deck's surrounding prose and slide structure remain yours. The final check confirms the deck is current.

## Why use it with Marp?

Use this tool when a deck should show results that already exist elsewhere in the repository, but building or refreshing the deck must not rerun expensive, privileged, or environment-specific work. Typical inputs include a tested source snippet, a benchmark CSV, a generated figure, or provenance produced by an upstream pipeline.

The updater has deliberately limited authority over the deck:

- only recognized, explicit generated regions are owned by the updater;
- `check` is always read-only, and `update` needs `--apply` to write;
- text outside owned regions is byte-preserved, including existing CRLF line endings;
- input and artifact paths stay under an explicit `--repo-root`; parent traversal and escaped symlinks are refused;
- the normal materialization path invokes no shell and never executes notebooks;
- writes are atomic, and a successful repeat update produces no further diff.

See the detailed [safety model](docs/safety.md) and [generated-region reference](docs/include-blocks.md).

## Installation

This pre-alpha project is currently installed from a checkout; this README does not claim a registry release.

For development or one-off use from a checkout:

```console
uv sync --group dev
uv run marp-artifact-updater --help
```

To install that checkout as a user-facing command:

```console
uv tool install .
marp-artifact-updater --help
```

`python -m marp_artifact_updater` provides the same command surface. Python 3.12+ and [uv](https://docs.astral.sh/uv/) are required.

## Core commands

```console
marp-artifact-updater check slides/deck.md --repo-root .
marp-artifact-updater update slides/deck.md --repo-root .          # dry run
marp-artifact-updater update slides/deck.md --repo-root . --apply  # write explicit regions
marp-artifact-updater check slides/deck.md --repo-root . --json
```

A pending update in `check` or dry-run `update` exits `1`. Malformed or unsafe input exits `2`. `--json` emits stable machine-readable change information, including region counts, warnings, paths, and SHA-256 fingerprints.

## Supported artifacts

- **Snippets** from named source intervals, including saved notebook code cells without running the notebook.
- **Dataframes** from CSV. Other table formats are deliberately unsupported and fail with an explicit message.
- **Figures** including common image formats and HTML/HTM artifacts.
- **Quotes** and **equations** from named source regions.
- **Provenance** blocks for recording the materialization context.
- **Python calls**, only as an opt-in exception: the exact module must be allowlisted with `--allow-python-module`. This is not part of the normal no-shell/no-notebook materialization workflow; read [Python calls](docs/python-calls.md) first.

`dataframe` and `figure` regions may declare `depends_on`. The updater warns when the artifact is older than that dependency; it does not regenerate the artifact.

## Marp Artifact Updater and Markdown Artifact Updater

**Start here when your destination is a Marp deck.** This repository provides the Marp-facing command, examples, and framing.

[Markdown Artifact Updater](https://github.com/ineryo/markdown-artifact-updater) is the general Markdown sibling for hand-authored Markdown. The repositories currently expose closely aligned safety and generated-region behavior, but remain separately packaged and governed; the tracked portfolio does not formally designate either repository as the other's canonical implementation. This README does not promise future consolidation or Marp-only features that do not exist today.

In short: Marp Artifact Updater is the concrete Marp entry point; Markdown Artifact Updater is the general Markdown sibling. Both keep artifact generation, materialization, and rendering as separate responsibilities.

## More examples and reference

- [`examples/quickstart/`](examples/quickstart/) — small clean onboarding deck.
- [`examples/snippet-demo/`](examples/snippet-demo/) — a fuller five-slide, executable snippet demo.
- [`examples/basic/`](examples/basic/) — intentionally stale fixture for demonstrating and testing an update; it is not the primary onboarding path.
- [Generated-region syntax and format options](docs/include-blocks.md)
- [Safety model](docs/safety.md)
- [Architecture](docs/architecture.md)
- [Contributing](CONTRIBUTING.md)

## Development

```console
uv sync --group dev
uv run pytest
uv run black --check .
uv run ruff check .
uv run pre-commit run --all-files
```

## License and security

This project is licensed under the [MIT License](LICENSE). For private vulnerability reporting, see [SECURITY.md](SECURITY.md).
