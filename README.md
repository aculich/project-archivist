# Project Archivist

Short options brief for keeping agent projects findable: a *local* parent folder, GitHub for code, object storage for blobs, and a catalog over that parent.

Not a build. Pick a storage scenario (A–D), then clone a catalog.

## The brief

[docs/options-brief.md](docs/options-brief.md) — cloneable tools, GitHub vs Drive vs restic/GCS, and how to turn this markdown into a pageless Google Doc with [gogcli](https://github.com/openclaw/gogcli).

## Publish as a pageless Google Doc

```
gog docs create "Archivist options brief" --file docs/options-brief.md --pageless --json
```

Quality bar (default Docs styles, native tables, named links, no leftover markdown) is in the [gogcli section](docs/options-brief.md#gogcli-drive--docs-from-the-cli).
