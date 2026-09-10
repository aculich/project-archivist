# Project Archivist

A comparison brief for keeping AI-agent project folders findable. Not an app.

If you bounce between Claude, Cursor, Codex, and other agents, you already know the mess: the useful chat died in a tool you are not in today; the folder is on Desktop, Drive, or last week’s unzip; the code belongs on GitHub but the 4 GB video and the 8,000-file dataset do not; and there is no catalog that answers *where is this* or *when did I last touch it*.

This repo is a written comparison of tools and storage layouts for that problem. The deep write-up is [docs/options-brief.md](docs/options-brief.md).

## Who it is for

- You keep a pile of agent project directories and cannot remember which disk or Drive folder they landed in.
- You want **code on GitHub**, but videos, datasets, and fat collections somewhere that is not Git LFS by default.
- You use **Google Drive** as the human-visible hub and have already been burned by git working trees on streamed Drive.
- You want agents in any tool to read the same markdown memory, without standing up a product.

If you only need a wiki *inside one repo*, look at [Project Librarian](https://github.com/kkjk1176/project-librarian) instead. This brief is about the *parent* folder across projects.

## What the compared tools enable

A parent folder plus a catalog means you stop hunting. [Aikito](https://github.com/lsaint/aikito) or [The Librarian](https://github.com/code-ministry-ltd/the-librarian) give every agent the same markdown memory. [gita](https://github.com/nosarthur/gita) / [git-workspace](https://github.com/orf/git-workspace) answer “what did I last touch?” Pointer stores and restic keep huge files and fat directories out of GitHub so monthly cost can fall as projects idle, instead of paying 2 TB of Drive forever or treating git as a backup.

## Switch Dimension

The originating thread is [AI Folder and Project Management — what are you using?](https://hub.switchdimension.com/c/start-here/ai-folder-and-project-management-what-are-you-using) in the [Switch Dimension](https://hub.switchdimension.com) community.

Aaron Culich: that conversation is rich and actually interesting, and he would welcome others who are stuck on the same folder-and-memory mess. Official pages do not publish a referral or invite code — join from the [Switch Dimension](https://www.switchdimension.com/) waitlist or the [hub](https://hub.switchdimension.com).

## What is in this repo

| Path | What it is |
| --- | --- |
| [docs/options-brief.md](docs/options-brief.md) | The brief: the problem, cloneable catalogs, GitHub vs Drive vs restic/GCS, four named layouts, and how to turn the markdown into a pageless Google Doc with [gogcli](https://github.com/openclaw/gogcli). |
| `README.md` | This page. |

Nothing here installs a daemon or creates a database.

## What to do next

1. Read [the brief](docs/options-brief.md).
2. Choose a storage layout (the brief spells out cost and failure modes for each):
   - **Drive as the human hub** — mirror a parent folder (for example `AgentProjects/`) for markdown and small files; clone git repos *next to* or *outside* Drive.
   - **Local disk + restic → GCS** — keep the parent on a real disk; snapshot it to Google Cloud Storage. Do not use Drive as the live `.git` tree.
   - **Git + pointers** — GitHub holds code and pointers; bytes live in LFS only when they are small and low-churn, otherwise [DVC](https://dvc.org/doc/user-guide/data-management) or [git-annex](https://git-annex.branchable.com/) to your own bucket.
   - **Hybrid** — hot work stays local + GitHub; idle projects get a restic snapshot on GCS Standard; finished work goes to Coldline/Archive; a `catalog.md` row records path, GitHub URL, blob URI, last-touched, and temperature.
3. Clone a catalog for the parent folder, not a per-repo wiki. Closest fits: [Aikito](https://github.com/lsaint/aikito) or [The Librarian](https://github.com/code-ministry-ltd/the-librarian).

### Optional: open the brief as a Google Doc

```
gog docs create "Archivist options brief" --file docs/options-brief.md --pageless --json
```

That path uses default Docs heading styles, native tables, and named links. Details are in the brief’s [gogcli section](docs/options-brief.md#gogcli-drive--docs-from-the-cli).
