# Project Archivist

A comparison brief for keeping AI-agent project folders findable.

If you bounce between Claude, Cursor, Codex, and friends, it is easy to lose the chat, lose the folder, or dump a 4 GB video into GitHub and watch LFS bills show up. This repo is not an app. It is a written answer to: *where should the code live, where should the big files live, and how do you ask “where is this / when did I last touch it” across many projects?*

The full write-up is [docs/options-brief.md](docs/options-brief.md). Start there if you want tools, costs, and commands.

## Who it is for

- You keep a pile of agent project directories and cannot remember which disk or Drive folder they landed in.
- You want **code on GitHub**, but videos, datasets, and fat collections somewhere that is not Git LFS by default.
- You use **Google Drive** as the human-visible hub and have already been burned by git working trees on streamed Drive.
- You want agents in any tool to read the same markdown memory, without standing up a product.

If you only need a wiki *inside one repo*, look at [Project Librarian](https://github.com/kkjk1176/project-librarian) instead. This brief is about the *parent* folder across projects.

## What is in this repo

| Path | What it is |
| --- | --- |
| [docs/options-brief.md](docs/options-brief.md) | The brief: cloneable catalogs, GitHub size/LFS limits, Drive vs restic/GCS pricing, four storage layouts, and how to turn the markdown into a pageless Google Doc with [gogcli](https://github.com/openclaw/gogcli). |
| `README.md` | This page. |

Nothing here installs a daemon or creates a database.

## What to do next

1. Read [the brief](docs/options-brief.md).
2. Choose a storage layout (the brief spells out cost and failure modes for each):
   - **Drive as the human hub** — mirror a parent folder (for example `AgentProjects/`) for markdown and small files; clone git repos *next to* or *outside* Drive.
   - **Local disk + restic → GCS** — keep the parent on a real disk; snapshot it to Google Cloud Storage. Do not use Drive as the live `.git` tree.
   - **Git + pointers** — GitHub holds code and pointers; bytes live in LFS only when they are small and low-churn, otherwise [DVC](https://dvc.org/doc/user-guide/data-management) or [git-annex](https://git-annex.branchable.com/) to your own bucket.
   - **Hybrid (the layout that still works at 100 projects)** — hot work stays local + GitHub; idle projects get a restic snapshot on GCS Standard; finished work goes to Coldline/Archive; a `catalog.md` row records path, GitHub URL, blob URI, last-touched, and temperature.
3. Clone a catalog for the parent folder, not a per-repo wiki. Closest fits: [Aikito](https://github.com/lsaint/aikito) (multi-agent, multi-project workspace) or [The Librarian](https://github.com/code-ministry-ltd/the-librarian) (markdown vault + MCP handoffs). Pair with [gita](https://github.com/nosarthur/gita) or [git-workspace](https://github.com/orf/git-workspace) so “what did I last touch?” is a CLI question.

The originating forum thread is [AI Folder and Project Management](https://hub.switchdimension.com/c/start-here/ai-folder-and-project-management-what-are-you-using).

### Optional: open the brief as a Google Doc

```
gog docs create "Archivist options brief" --file docs/options-brief.md --pageless --json
```

That path uses default Docs heading styles, native tables, and named links. Details are in the brief’s [gogcli section](docs/options-brief.md#gogcli-drive--docs-from-the-cli).
