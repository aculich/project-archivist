# Archivist options brief

A comparison of tools and storage layouts for keeping AI-agent projects findable. Not a product. Not a build.

**Rule of thumb:** Drive is a good *human* hub; restic + GCS is a better *backup/archive*; Git LFS is a poor default for videos and churny binaries. Keep a *local* parent folder (not a live git working tree on streamed Drive), put *code* on GitHub, put *blobs and bulky collections* in object storage, and run a *catalog* over that parent.

---

## The problem

You will recognize this if several of these are true:

- You work across Claude, Cursor, Codex, and other agents, and the useful conversation died in a tool you are not in today.
- Project folders multiply — Desktop, `~/code`, a Drive stream, last week’s unzip — and you cannot answer *where is this* or *when did I last touch it*.
- Code belongs on GitHub, but the mp4, the dataset, and the folder with 8,000 small files do not. One huge file, one git repo, and one giant directory are three different failure modes.
- There is no catalog: no single list of path, GitHub URL, blob store, last-touched date, and whether the project is hot, idle, or done.

That is three jobs, not one:

1. **Catalog** — find projects, last-touched dates, “what lives where.”
2. **Shared agent memory** — markdown any tool can read, so work started in one agent can be picked up in another.
3. **Storage policy** — GitHub for code; somewhere else for big files and large collections; hot / warm / cold as projects idle.

---

## What the compared tools enable

A parent folder plus a catalog means you stop hunting. Cloneable workspaces ([Aikito](https://github.com/lsaint/aikito), [The Librarian](https://github.com/code-ministry-ltd/the-librarian)) give every agent the same markdown memory. Git helpers ([gita](https://github.com/nosarthur/gita), [git-workspace](https://github.com/orf/git-workspace)) answer “what did I last touch?” at the CLI. Pointer stores and restic keep videos and fat directories out of GitHub so a 4 GB file does not become an LFS bill. The layouts below exist so monthly cost can *fall* as projects graduate, instead of paying for a 2 TB Drive forever or treating Git as a backup ([GitHub: git is not a backup tool](https://docs.github.com/en/repositories/working-with-files/managing-large-files/about-large-files-on-github)).

Closest cloneable catalogs: [Aikito](https://github.com/lsaint/aikito) (multi-agent, multi-project workspace) and [The Librarian](https://github.com/code-ministry-ltd/the-librarian) (markdown vault + MCP handoffs). [Project Librarian](https://github.com/kkjk1176/project-librarian) is a *per-repo* wiki, not a cross-project index.

---

## Switch Dimension

This comparison started from the thread [AI Folder and Project Management — what are you using?](https://hub.switchdimension.com/c/start-here/ai-folder-and-project-management-what-are-you-using) in the [Switch Dimension](https://hub.switchdimension.com) community (the hub title is *Build With AI - Switch Dimension*; the public site brands as [Switch Dimension AI](https://www.switchdimension.com/)).

Aaron Culich: that community is a rich, actually interesting conversation among people shipping with the same agents — and he would welcome others who are stuck on the same folder-and-memory mess. Official pages do not publish a referral or invite code. Join from the [Switch Dimension](https://www.switchdimension.com/) waitlist (next Build With AI cohort) or the [community hub](https://hub.switchdimension.com).

---

## Tools you can clone

| Clone this | What it actually is | Use it for |
| --- | --- | --- |
| [lsaint/aikito](https://github.com/lsaint/aikito) | Git-managed personal workspace for instructions, skills, MCP, and durable markdown memory across many agents and projects. Install: `uv tool install aikito` or `brew install lsaint/tap/aikito`. No database. Companion [chat-distiller](https://github.com/lsaint/chat-distiller) turns browser chats into inbox notes. | One parent workspace every agent can see. |
| [code-ministry-ltd/the-librarian](https://github.com/code-ministry-ltd/the-librarian) | Markdown+git vault (memories, handoffs, references) with a resident curator and 7 MCP verbs. Cross-harness handoffs for Claude Code, Codex, OpenCode, Hermes, Pi. | Packaging work started in one tool and picked up in another. |
| [kkjk1176/project-librarian](https://github.com/kkjk1176/project-librarian) | Repo-local planning wiki (`wiki/startup.md`) plus hooks for Codex, Claude Code, Cursor, Gemini CLI. | Per-repo session memory, not a global catalog. |
| [jimy-r/agent-workspace-architecture](https://github.com/jimy-r/agent-workspace-architecture) | Reference layout: roles, typed memory, task board, audits. Claude-Code files; ideas port. | Steal the memory/task conventions, don’t treat as a product. |
| [orf/git-workspace](https://github.com/orf/git-workspace) | Sync a local directory with GitHub/GitLab/Gitea; clone new repos; move deleted ones to `.archived/`. | Keep the GitHub side of a parent folder honest. |
| [nosarthur/gita](https://github.com/nosarthur/gita) | Status dashboard + batch git across many local repos. | “What did I last touch?” at the CLI. |
| [myrepos](https://myrepos.branchable.com/) | `mr` — register repos, `mr update` them all. Same author family as [git-annex](https://git-annex.branchable.com/). | Heterogeneous VCS fleet. |
| [github/git-sizer](https://github.com/GitHub/git-sizer) | Measures repo bloat (size, object count, huge blobs). | Decide if a collection belongs in Git at all. |

macOS launchers (not catalogs, but they open the folder fast): [RepoPad](https://repopad.com/) (Spotlight / Alfred / Raycast), [Raycast Repository Manager](https://www.raycast.com/francesco_mecchi/repository-manager), [kfdm/alfred-repos](https://github.com/kfdm/alfred-repos). Pair with `mdfind` / Spotlight on a local parent dir.

**None of these is the full Librarian.** Closest composition: Aikito or The Librarian for agent memory + `gita` / `git-workspace` / RepoPad for “where is the repo” + a small markdown index (`catalog.md`) that records *path, github, blob store, last-touched, temperature*.

---

## macOS layer (practical)

- **Parent directory on a real disk**, e.g. `~/AgentProjects` or `~/GitHub`. Point Claude / Cursor / Codex at that root. That is the pattern that avoids live `.git` trees on streamed Drive.
- **Drive for desktop:** [stream vs mirror](https://support.google.com/drive/answer/13401938). Streaming on macOS 12.1+ uses File Provider; files are online unless marked offline, and the Drive app must be running ([macOS Drive help](https://support.google.com/drive/answer/12178485), [manage Drive for desktop](https://support.google.com/drive/answer/16631477)). Mirroring keeps a full local copy. [Working git *inside* a synced cloud folder is a known foot-gun](https://tonym.us/move-github-repos-to-google-drive.html) (index races, dehydrated objects, “bad object”). If Drive is in the loop, use it as a *bare remote* or a *docs/blob* tree — not as the working clone. CLI for the same Drive/Docs tree: [gogcli](#gogcli-drive--docs-from-the-cli).
- **Finder aliases + tags** for “active / idle / archived.” Spotlight/`mdfind` is the free catalog for local files.
- **Raycast / Alfred / RepoPad** to jump to a repo without hunting folders.
- **restic + `gcloud`** for snapshots to GCS ([restic GCS backend](https://restic.readthedocs.io/en/stable/030_preparing_a_new_repo.html)): service account JSON, `gs:bucket:/path`, `storage.objects.{create,delete,get,list}`.

---

## gogcli (Drive / Docs from the CLI)

[gog / gogcli](https://github.com/openclaw/gogcli) is a single binary for Drive, Docs, Gmail, Calendar, Sheets, and the rest of Workspace from a terminal or agent ([README](https://github.com/openclaw/gogcli/blob/main/README.md)). Install: `brew install openclaw/tap/gogcli` ([install](https://gogcli.sh/install.html)); releases also ship as ZIPs and `ghcr.io/openclaw/gogcli`. Not a catalog. Use it when Drive is the *human* hub and you want scripts to list folders, upload blobs, and turn markdown into a real Doc.

Auth is a Desktop OAuth client, then ([quickstart](https://github.com/openclaw/gogcli/blob/main/docs/quickstart.md)):

```
gog auth credentials ~/Downloads/client_secret_....json
gog auth add you@gmail.com --services drive,docs
gog auth doctor --check
```

Headless / split-machine: `--manual` or `--remote --step 1` / `--step 2`. Daily Drive: `gog drive ls`, `gog drive search`, `gog drive upload`, `gog drive download`, `gog drive share`.

### Markdown → pageless Google Doc

Two converters. Do not mix them up.

| Path | What it actually is | Use it for |
| --- | --- | --- |
| Drive import: `gog docs create "Title" --file brief.md` or `gog drive upload brief.md --convert-to doc` | Google’s own markdown importer — same family as **File → Open** / Drive **Open with Google Docs** ([Workspace update, Jul 2024](https://workspaceupdates.googleblog.com/2024/07/import-and-export-markdown-in-google-docs.html), [Use Markdown in Docs](https://support.google.com/docs/answer/12014036)). Drive `files.create` with a Google Workspace `mimeType` is the convert hook ([Drive uploads](https://developers.google.com/workspace/drive/api/guides/manage-uploads)). Whole-document only ([docs editing](https://gogcli.sh/docs-editing.html)). | First create of a whole Doc. |
| Local renderer: `gog docs write <id> --replace --markdown --file brief.md` | gog parses the markdown and issues Docs API `batchUpdate` (headings, lists, tables, images, links) ([docs editing](https://gogcli.sh/docs-editing.html)). | Updates and tab-scoped re-renders. Drive’s converter cannot target one tab. |

Recipe that matches this brief:

```
gog docs create "Archivist options brief" --file options-brief.md --pageless --json
# later rewrite the same Doc:
gog docs write <docId> --replace --markdown --file options-brief.md --pageless
```

`--file` on create imports markdown (inline images from public HTTPS URLs) ([`docs create`](https://gogcli.sh/commands/gog-docs-create.html)). `--pageless` sets Docs `documentMode` to pageless. Pageless is continuous scroll: wide tables, images that reflow, no page breaks; headers, footers, page numbers, and columns are hidden / unavailable ([page setup](https://support.google.com/docs/answer/11528737)). Existing Doc: `gog docs page-layout <id> --layout pageless`.

Quality bar — default Docs styles, real tables, named clickable links, no leftover raw markdown:

- **Default named styles, not homemade fonts.** `#` / `##` must land as Docs named styles `HEADING_1` / `HEADING_2` (also `TITLE`, `SUBTITLE`, `NORMAL_TEXT`, `HEADING_3`–`HEADING_6`) ([`NamedStyleType`](https://developers.google.com/workspace/docs/api/reference/rest/v1/documents#NamedStyleType); gog `--named-style` / `--heading-level` set the same enum ([docs editing](https://gogcli.sh/docs-editing.html))). Check: `gog docs headings list <id>`. Promote a leftover: `gog docs format <id> --match "The problem" --heading-level 2`. Do not paint Arial-14 over everything — inherit the named style.
- **Real tables, not pipe text.** GFM `| a | b |` through `--markdown` becomes a native table; `gog docs insert-table <id> --rows N --cols M --values-json '[[…]]' --at-end` if you want a guaranteed native table ([docs editing](https://gogcli.sh/docs-editing.html)). Pageless is the right layout for wide comparison tables. Verify: `gog docs tables list <id> --json`. If `gog docs cat` still shows `| --- |`, the converter missed it — fix the markdown (need a header + separator row) or use `insert-table`.
- **Named clickable links.** Write `[Aikito](https://github.com/lsaint/aikito)`, not a bare URL and not leftover `[Aikito](https://…)`. That is a Docs `TextStyle.link` ([format text](https://developers.google.com/workspace/docs/api/how-tos/format-text); same markdown link shape Docs documents in the UI ([Use Markdown](https://support.google.com/docs/answer/12014036))). Patch: `gog docs format <id> --match "Aikito" --link https://github.com/lsaint/aikito`. `--link` accepts HTTP(S), `mailto:`, bookmark IDs, and heading slugs. Internal `[text](#slug)` is the foot-gun: it used to land as a raw `#slug` that Docs does not jump ([gogcli#633](https://github.com/openclaw/gogcli/issues/633)). Prefer full HTTPS URLs for a shareable brief. `gog docs paragraphs list <id> --json` reports link metadata.
- **No leftover raw markdown.** Do not paste a `.md` as plain text (that is how `**bold**` and `[name](url)` survive). Do not `gog drive upload brief.md` *without* `--convert` / `--convert-to doc` — that stores a markdown blob, not a Doc ([`drive upload`](https://gogcli.sh/commands/gog-drive-upload.html)). Do not use Pandoc `{#slug}` on headings; it leaks as literal heading text ([gogcli#703](https://github.com/openclaw/gogcli/issues/703)). YAML frontmatter (`---`) is stripped on Drive convert unless you pass `--keep-frontmatter`. After convert, `gog docs cat <id>` should have no `**`, fence markers, or `[text](url)` leftovers; `gog docs export <id> md` is the round-trip check.

---

## Code vs one huge file vs lots of files

These are different problems.

### GitHub hard limits

[GitHub blocks files over 100 MiB](https://docs.github.com/en/repositories/working-with-files/managing-large-files/about-large-files-on-github); warns above 50 MiB; browser uploads max 25 MiB. [Recommended repo size: under 1 GB, strongly under 5 GB](https://docs.github.com/en/repositories/working-with-files/managing-large-files/about-large-files-on-github). [On-disk `.git` recommended max 10 GB](https://docs.github.com/en/repositories/creating-and-managing-repositories/repository-limits); directory width recommended max 3,000 entries; single-object recommended max 1 MB. Git is [explicitly not a backup tool](https://docs.github.com/en/repositories/working-with-files/managing-large-files/about-large-files-on-github). Releases can host large binaries (no total-size/bandwidth cap stated) but each file still has to fit the [LFS per-file max for your plan](https://docs.github.com/en/repositories/working-with-files/managing-large-files/about-git-large-file-storage).

### One or a few huge blobs (e.g. an mp4)

| Tool | How it works | When it fits | When it breaks |
| --- | --- | --- | --- |
| [Git LFS](https://docs.github.com/en/repositories/working-with-files/managing-large-files/about-git-large-file-storage) | Pointer in git, bytes on GitHub LFS. Per-file max **2 GB Free/Pro, 4 GB Team, 5 GB Enterprise Cloud**. | Occasional binaries you want tied to a commit. | A 4 GB video on Free is rejected. Every rewrite stores a **full new copy** ([LFS billing](https://docs.github.com/en/billing/managing-billing-for-your-products/managing-billing-for-git-large-file-storage/about-billing-for-git-large-file-storage)). |
| [git-annex](https://git-annex.branchable.com/) | Symlink/pointer in git; content in *any* remote (disk, SSH, S3, …). Oldest, most flexible ([LWN comparison](https://lwn.net/Articles/774125/)). | Multi-remote, offline, “this video lives on Drive *and* GCS.” Steeper learning curve. | Weak GitHub UI; not a GitHub-hosted LFS replacement. |
| [DVC](https://dvc.org/doc/user-guide/data-management) | `.dvc` pointer files; cache + *your* S3/GCS/SSH. Content-addressed, good for directories of many files ([large-dataset notes](https://dvc.org/doc/user-guide/data-management/large-dataset-optimization); [SO](https://stackoverflow.com/questions/71663330/what-is-the-advantage-of-dvc-git-annex-git-lfs-for-large-or-binary-files-over)). | Datasets and collections; APFS reflinks on macOS avoid double disk use. | Overkill for one mp4; Python-centric. |
| Pointer + object store | Commit a sidecar (`video.mp4.uri` → `gs://…` or Drive file ID). | Anything bigger than LFS max, or high-churn media. | You must restore files yourself. |
| GitHub Releases | Attach the finished asset to a tag. | Distribution, not daily editing. | Same per-file LFS-plan cap. |

**LFS money:** Free/Pro include **10 GiB storage + 10 GiB bandwidth / month**; Team / Enterprise Cloud **250 GiB** each ([LFS billing](https://docs.github.com/en/billing/managing-billing-for-your-products/managing-billing-for-git-large-file-storage/about-billing-for-git-large-file-storage)). Enterprise metered overage was published at **$0.07 / GiB-month storage** and **$0.0875 / GiB bandwidth** ([GitHub changelog, Jun 2024](https://github.blog/changelog/2024-06-03-new-enterprise-accounts-have-metered-billing-for-git-lfs/)). Push a 500 MB file twice (1-byte change) → **1 GB storage, 0 bandwidth**; each clone/pull of it burns bandwidth ([same billing doc](https://docs.github.com/en/billing/managing-billing-for-your-products/managing-billing-for-git-large-file-storage/about-billing-for-git-large-file-storage)).

### Lots of files, none individually huge

GitHub’s pain is **object count and fat directories**, not only file size ([git-sizer](https://github.com/GitHub/git-sizer), [GitHub blog](https://github.blog/developer-skills/github/measuring-the-many-sizes-of-a-git-repository/)). Do **not** LFS every small file. Prefer:

- Keep the collection **out of git**; commit a manifest or DVC directory target ([DVC](https://dvc.org/doc/user-guide/data-management/large-dataset-optimization)).
- Split into subprojects; avoid 3,000+ entries in one folder ([repo limits](https://docs.github.com/en/repositories/creating-and-managing-repositories/repository-limits)).
- Snapshot the tree with **restic** (chunked, deduped) instead of committing it.

---

## Cloud cost: Drive vs restic / GCS

### Google Drive (hot, human-visible)

Official [Google One](https://one.google.com/about/plans) (USD monthly): **15 GB free** (shared with Gmail/Photos); **100 GB $1.99**; **2 TB $9.99** (AI Plus); **5 TB $19.99** (AI Pro). [Workspace](https://workspace.google.com/pricing?hl=en_us): **30 GB / 2 TB / 5 TB pooled per user** on Starter / Standard / Plus. [Max file 5 TB](https://developers.google.com/workspace/drive/api/guides/limits); **750 GB / day** upload or copy; copies of files **> 750 GB** are blocked ([same](https://developers.google.com/workspace/drive/api/guides/limits)).

Drive does **not** have cheap cold tiers. You pay the One/Workspace flat fee whether the folder is active or idle. Good for markdown + current blobs you browse in Finder. Bad as the only store once you pass a few TB, or as a live git working tree.

### GCS + restic (hot / warm / cold)

Official regional (e.g. Iowa) **hourly** rates ([GCS pricing](https://cloud.google.com/storage/pricing)): Standard **$0.000027397 / GiB-hour** (~**$0.020 / GiB-month**); Nearline **$0.000013699** (~**$0.010**); Coldline **$0.000005479** (~**$0.004**); Archive **$0.000001644** (~**$0.0012**). Retrieval: **$0 / $0.01 / $0.02 / $0.05 per GiB**. Minimum duration: **none / 30 / 90 / 365 days**; delete early and you still pay the minimum.

| Temperature | Class | ~1 TB / month at rest | Pull the whole 1 TB once |
| --- | --- | --- | --- |
| Hot | Standard | ~$20 | $0 retrieval |
| Warm | Nearline | ~$10 | +$10 |
| Cool | Coldline | ~$4 | +$20 |
| Cold | Archive | ~$1.20 | +$50 |

Plus operations and egress. **Do not put a frequently pruned restic repo on Nearline/Coldline** — prune rewrites packs and triggers early-delete fees ([restic forum](https://forum.restic.net/t/regional-vs-nearline-vs-coldline-google-storage/1168)). Pattern that works: restic repo on **Standard**; copy *finished* project tarballs or restic snapshots you will not prune onto Coldline/Archive via lifecycle (lifecycle class changes skip early-delete ([GCS pricing](https://cloud.google.com/storage/pricing))).

`gcloud` / Autoclass can move objects automatically; Autoclass has its own management fee (**$0.0025 per 1,000 objects / 30 days** ([GCS pricing](https://cloud.google.com/storage/pricing))) — painful for millions of tiny files, fine for a modest blob store.

### Scale stories (same 1 TB of idle project data)

- **Drive 2 TB plan:** $9.99/mo whether you touch it or not. Simple. No real “archive discount.”
- **GCS Standard + restic:** ~$20/mo, instant restore, prune-safe.
- **GCS Archive copies of finished projects:** ~$1.20/mo + a painful restore bill when you need them.
- **GitHub LFS for that 1 TB:** on Free, 10 GiB included then ~**$70/mo** storage at $0.07/GiB *plus* bandwidth every clone. Wrong tool.

---

## Four layouts

### Drive as the human hub

**Layout:** Drive parent `AgentProjects/` (mirror, not stream). Markdown + small assets live there. Code still on GitHub; clone *next to* or *outside* Drive. Catalog agent rooted on that folder.

**Clone:** [Aikito](https://github.com/lsaint/aikito) or [The Librarian](https://github.com/code-ministry-ltd/the-librarian) in that parent. [RepoPad](https://repopad.com/) / Raycast to open folders. CLI for Drive/Docs: [gogcli](#gogcli-drive--docs-from-the-cli).

**Huge file:** put the mp4 in Drive (up to 5 TB, watch the 750 GB/day cap). Commit a path or Drive ID in the git repo. Do not LFS it unless it is < 2 GB and rarely changes.

**Lots of files:** keep the collection as a Drive folder; git gets a manifest. Do not sync a fat `.git` through Drive.

**Cost:** One/Workspace flat fee. Fine until you outgrow 2–5 TB or need real archive pricing.

**Risk:** Streamed Drive + git = corruption. Shared Drive sync conflicts.

### Local disk + restic → GCS

**Layout:** `~/AgentProjects` on APFS. GitHub for code. `restic backup ~/AgentProjects` → `gs:bucket:/restic` on **Standard**. Optional lifecycle: copy *closed* projects to Coldline/Archive as a second prefix, not by mutating the live restic repo.

**Clone:** [gita](https://github.com/nosarthur/gita) + [git-workspace](https://github.com/orf/git-workspace) + Aikito/Librarian for memory.

**Huge file:** restic chunk/dedup handles one mp4 well. Git repo holds a relative path.

**Lots of files:** restic is the right tool (dedup, no git object explosion).

**Cost:** disk + ~$0.02/GiB-month hot backup. Archive only what you will not prune.

**Risk:** You must run backups; Drive is not in the loop unless you also export.

### Git + pointer store (LFS / annex / DVC)

**Layout:** GitHub = code + pointers. Bytes in GitHub LFS *or* (better) DVC/annex → GCS/S3. Drive optional for humans.

**Clone:** same catalog tools; add [DVC](https://dvc.org/doc/user-guide/data-management) or [git-annex](https://git-annex.branchable.com/). Measure with [git-sizer](https://github.com/GitHub/git-sizer).

**Huge file:** LFS only if under plan max and low churn. Otherwise annex/DVC/GCS. A 3 GB mp4 is already past Free/Pro LFS.

**Lots of files:** DVC directory target or annex; never one LFS file per small asset.

**Cost:** LFS overage is expensive vs GCS Standard. DVC/annex to your bucket ≈ GCS rates above.

**Risk:** LFS quota blocks pushes; CI clones burn bandwidth.

### Hybrid (recommended once you have more than a handful of projects)

**Layout:**

- **Hot (this month):** local `~/AgentProjects` + GitHub. Optional Drive *mirror of docs/exports only*.
- **Warm (idle but might reopen):** restic snapshot on GCS Standard (or Nearline only if you prune a few times a year).
- **Cold (done):** one tarball or restic-copy prefix on Coldline/Archive; local tree deleted or thinned; catalog row says *where* and *when*.
- **Index:** markdown `catalog.md` (or Aikito notes) with `name, github, local_path, blob_uri, last_touched, temp`.

**Huge file:** local + restic; pointer in git. Drive only if you need to share in Finder.

**Lots of files:** same as local disk + restic; git never holds the collection.

**Cost:** Drive for the small human set + GCS for the growing bulk. As projects graduate, monthly cost falls instead of paying 2 TB Drive forever.

**Risk:** More moving parts. Worth it once you have more than a handful of active projects or any real media/data.

---

## Choose a layout

- Want it this week, mostly markdown, Drive as the place humans browse: **Drive as the human hub**.
- Want backups and scale without Drive-as-git: **Local disk + restic → GCS**.
- Already have datasets tied to commits: **Git + pointer store** (prefer DVC/annex over GitHub LFS).
- Want the thing that still works at 100 projects and a pile of mp4s: **Hybrid**.

After you choose, next slice is a concrete macOS folder layout + catalog schema + one clone (Aikito or The Librarian) wired to it.

---

## Sources

- [AI Folder and Project Management (Switch Dimension hub)](https://hub.switchdimension.com/c/start-here/ai-folder-and-project-management-what-are-you-using)
- [Switch Dimension community hub](https://hub.switchdimension.com)
- [Switch Dimension AI (official site)](https://www.switchdimension.com/)
- [kkjk1176/project-librarian](https://github.com/kkjk1176/project-librarian)
- [code-ministry-ltd/the-librarian](https://github.com/code-ministry-ltd/the-librarian)
- [lsaint/aikito](https://github.com/lsaint/aikito) / [PyPI aikito](https://pypi.org/project/aikito/)
- [jimy-r/agent-workspace-architecture](https://github.com/jimy-r/agent-workspace-architecture)
- [orf/git-workspace](https://github.com/orf/git-workspace)
- [nosarthur/gita](https://github.com/nosarthur/gita)
- [myrepos](https://myrepos.branchable.com/)
- [RepoPad](https://repopad.com/)
- [Raycast Repository Manager](https://www.raycast.com/francesco_mecchi/repository-manager)
- [kfdm/alfred-repos](https://github.com/kfdm/alfred-repos)
- [About large files on GitHub](https://docs.github.com/en/repositories/working-with-files/managing-large-files/about-large-files-on-github)
- [Repository limits](https://docs.github.com/en/repositories/creating-and-managing-repositories/repository-limits)
- [About Git Large File Storage](https://docs.github.com/en/repositories/working-with-files/managing-large-files/about-git-large-file-storage)
- [Git LFS billing](https://docs.github.com/en/billing/managing-billing-for-your-products/managing-billing-for-git-large-file-storage/about-billing-for-git-large-file-storage)
- [GitHub changelog: metered LFS pricing](https://github.blog/changelog/2024-06-03-new-enterprise-accounts-have-metered-billing-for-git-lfs/) (Jun 2024)
- [github/git-sizer](https://github.com/GitHub/git-sizer)
- [Measuring the many sizes of a Git repository](https://github.blog/developer-skills/github/measuring-the-many-sizes-of-a-git-repository/)
- [Large files with Git: LFS and git-annex (LWN)](https://lwn.net/Articles/774125/)
- [Git LFS alternatives (files.link)](https://files.link/blog/git-lfs-alternatives)
- [DVC vs Git-LFS vs LakeFS](https://reintech.io/blog/dvc-vs-git-lfs-vs-lakefs-ml-data-versioning)
- [DVC large dataset optimization](https://dvc.org/doc/user-guide/data-management/large-dataset-optimization)
- [Stack Overflow: DVC / annex / LFS vs git](https://stackoverflow.com/questions/71663330/what-is-the-advantage-of-dvc-git-annex-git-lfs-for-large-or-binary-files-over)
- [git-annex](https://git-annex.branchable.com/)
- [Google Cloud Storage pricing](https://cloud.google.com/storage/pricing)
- [nOps GCS pricing 2026 guide](https://www.nops.io/blog/google-cloud-storage-pricing/)
- [restic: preparing a new repo (GCS)](https://restic.readthedocs.io/en/stable/030_preparing_a_new_repo.html)
- [restic forum: Nearline/Coldline vs prune](https://forum.restic.net/t/regional-vs-nearline-vs-coldline-google-storage/1168)
- [Google One plans](https://one.google.com/about/plans)
- [Google Workspace pricing](https://workspace.google.com/pricing?hl=en_us)
- [Drive API usage limits](https://developers.google.com/workspace/drive/api/guides/limits)
- [Stream & mirror files with Drive for desktop](https://support.google.com/drive/answer/13401938)
- [Use Drive for desktop on macOS](https://support.google.com/drive/answer/12178485)
- [Manage Google Drive for desktop](https://support.google.com/drive/answer/16631477)
- [Git remotes on Google Drive](https://tonym.us/move-github-repos-to-google-drive.html)
- [openclaw/gogcli](https://github.com/openclaw/gogcli) / [README](https://github.com/openclaw/gogcli/blob/main/README.md)
- [gogcli install](https://gogcli.sh/install.html)
- [gogcli quickstart](https://github.com/openclaw/gogcli/blob/main/docs/quickstart.md)
- [gogcli Docs editing](https://gogcli.sh/docs-editing.html)
- [`gog docs create`](https://gogcli.sh/commands/gog-docs-create.html)
- [`gog docs write`](https://gogcli.sh/commands/gog-docs-write.html)
- [`gog drive upload`](https://gogcli.sh/commands/gog-drive-upload.html)
- [Import and export Markdown in Google Docs (Workspace update, Jul 2024)](https://workspaceupdates.googleblog.com/2024/07/import-and-export-markdown-in-google-docs.html)
- [Use Markdown in Google Docs, Slides, & Drawings](https://support.google.com/docs/answer/12014036)
- [Change a document’s page setup: pages or pageless](https://support.google.com/docs/answer/11528737)
- [Docs API: NamedStyleType](https://developers.google.com/workspace/docs/api/reference/rest/v1/documents#NamedStyleType)
- [Docs API: format text / links](https://developers.google.com/workspace/docs/api/how-tos/format-text)
- [Drive API: upload and convert](https://developers.google.com/workspace/drive/api/guides/manage-uploads)
- [gogcli#633: `#slug` links vs Docs heading IDs](https://github.com/openclaw/gogcli/issues/633)
- [gogcli#703: Pandoc `{#slug}` leaks into headings](https://github.com/openclaw/gogcli/issues/703)

Search note: `parallel-cli` v0.9.3 is installed but not authenticated (`PARALLEL_API_KEY` unset; `parallel-cli auth` → “Not authenticated”). Switch Dimension naming and join paths are from first-party fetch of [switchdimension.com](https://www.switchdimension.com/) and [hub.switchdimension.com](https://hub.switchdimension.com). No official referral/invite URL was published on those pages.
