# Examples

- [`quickstart/`](quickstart/) is the clean, smallest onboarding path. It begins current, then lets you make and apply one bounded source change.
- [`snippet-demo/`](snippet-demo/) is a complete five-slide executable Marp presentation example.
- [`basic/`](basic/) is an intentionally stale minimal fixture used to demonstrate and test an update. It is not the primary newcomer path.

All commands are run from the repository root. For the primary quick start:

```console
uv run marp-artifact-updater check deck.md --repo-root examples/quickstart
```

See [`quickstart/README.md`](quickstart/README.md) for the complete check → explicit apply → verification flow.
