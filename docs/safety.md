# Safety model

`marp-artifact-updater` treats a Markdown file as mostly immutable. It renders
only bodies bounded by a matching `*-include` opening and closing comment.
Text outside recognized regions is byte-preserved, including CRLF line endings.

## Write controls

- `check` never writes the Markdown target.
- `update` never writes the Markdown target unless `--apply` is present.
- For regions that do not use opt-in `python-call`, these commands perform no code execution.
- An apply writes a complete replacement to a temporary file in the target
directory, fsyncs it, then atomically replaces the target.
- A replacement failure leaves the original target unchanged and removes the
temporary file.
- A second successful update is idempotent: it produces no further diff.

## Path controls

Every Markdown input and referenced artifact is resolved under `--repo-root`.
Parent-directory traversal and existing symlinks that point outside the root are
refused. The updater never follows an escaped path to read or overwrite it.

## Execution controls

Saved notebook content is parsed as JSON and never executed. Python calls are disabled unless their exact module name is supplied with `--allow-python-module`; when enabled, they execute while the document is planned, including during `check` and dry-run `update`, and their module behavior is outside the normal no-exec boundary. See [Python calls](python-calls.md). The updater directly invokes no shell.

## Staleness

`dataframe` and `figure` regions can declare `depends_on`. The updater reports
an explicit warning if the referenced artifact is older than that dependency;
it does not regenerate the artifact.
