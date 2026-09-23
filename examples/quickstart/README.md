# Marp quick start

This is a clean, ready-to-check Marp deck. `source.py` owns the marked source interval; `deck.md` owns its prose and slide structure. Only the body between the two `snippet-include` comments is updater-managed.

From the repository root:

```console
uv run marp-artifact-updater check deck.md --repo-root examples/quickstart
```

The clean example exits `0`: its generated region already matches `source.py`.

To see a bounded update, change the string in `source.py`, then run:

```console
uv run marp-artifact-updater check deck.md --repo-root examples/quickstart
uv run marp-artifact-updater update deck.md --repo-root examples/quickstart --apply
uv run marp-artifact-updater check deck.md --repo-root examples/quickstart
```

The first command reports the pending change without writing. The explicit `--apply` updates the snippet body only. The final check exits `0`; the surrounding slide prose remains unchanged.
