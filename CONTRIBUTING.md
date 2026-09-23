# Contributing

Thank you for considering a contribution.

## Product boundary

This project materializes already-produced artifacts into explicit generated regions in Marp Markdown. Contributions should preserve its conservative authority boundary:

- `check` and dry-run `update` remain read-only; writes require `update --apply`.
- Text outside recognized regions remains outside the updater's authority.
- Paths remain confined to the declared repository root.
- The normal materialization path does not invoke a shell or execute notebooks.
- Python calls remain an opt-in exception requiring an exact command-line module allowlist.
- Artifact generation and Marp rendering belong to upstream and downstream tools, not this updater.

Avoid adding broad templating, build orchestration, rendering, or unconstrained execution without separately authorized scope.

## Local checks

Use Python 3.12+ and uv:

```console
uv sync --group dev
uv run pytest
uv run black --check .
uv run ruff check .
uv run pre-commit run --all-files
```

Keep changes focused, add behavior tests before implementation, and ensure the repository is clean before requesting review.

## Licensing and publication

By contributing to this repository, you agree that your contributions will be licensed under the repository's [MIT License](LICENSE).
