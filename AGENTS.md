# Repository Agent Guidance

## Scope

This repository is independently governed. Work only within the explicitly authorized task and tracked-path boundary. Do not add a remote, publish a registry release, select a different license, or make a structural product decision without explicit human authority.

## Current product boundary

The implemented command supports deterministic `check` and explicit `update --apply` of recognized generated regions in Marp Markdown. Its documented supported artifact types include snippets, tables/dataframes, figures, quotations, equations, provenance, and an explicitly allowlisted Python-call exception.

Preserve the product's deliberately narrow authority: no implicit writes, no shell execution, no notebook execution, repository-root containment, and no responsibility for artifact generation or slide rendering. Do not expand into a generic build system, renderer, or unconstrained execution framework without separately authorized scope.

## Quality

Use uv with Python 3.12+. Run pytest, Black, Ruff, and local pre-commit hooks before review. Keep the worktree clean and surface blockers explicitly.
