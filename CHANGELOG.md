# Changelog

All notable changes to this project will be documented in this file.

## Unreleased

- Reframed the README around bounded Marp artifact materialization, added a clean newcomer quick start, and clarified the relationship with Markdown Artifact Updater.
- Updated architecture documentation to match the implemented materialization, path-confinement, and execution-boundary contract.

- Added C++ snippet extraction using valid `// snippet:start/end` markers for
  `.cpp`, `.cc`, `.cxx`, `.hpp`, and `.h` sources.
- Adopted the MIT License for publication.
- Hardened deterministic generated-region updates for public review.
- Added safety, idempotence, containment, symlink, CRLF, stale-artifact,
  atomic-failure, notebook non-execution, and Python-call allowlist coverage.
- Added public safety and Python-call documentation, executable examples, and
  a cross-platform CI/build matrix.
