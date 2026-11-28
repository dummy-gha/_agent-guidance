# AGENTS.md Drift Report

> Last generated: 2026-11-28 15:16 UTC

## dummy-gha

> Organization: `dummy-gha` — 4 repo(s) scanned

| Repository | Status | Has marker | CLAUDE.md bridge | skills-bootstrap | fleet-memory | fleet-guidance | contributor-rules | Open PR | Sections | Notes |
|------------|--------|------------|-------------------|------------------|--------------|----------------|-------------------|---------|----------|-------|
| [`dummy-gha/acsi-agentskills`](https://github.com/dummy-gha/acsi-agentskills) | **up-to-date** | yes | bridge-ok | — | — | — | ok | none | python | gitattr-ok |
| [`dummy-gha/acsi-agentskills-private`](https://github.com/dummy-gha/acsi-agentskills-private) | **up-to-date** | yes | bridge-ok | ok | — | — | ok | none | none | gitattr-ok; lock: acsi-agentskills@3ec0b51 |
| [`dummy-gha/repo-settings`](https://github.com/dummy-gha/repo-settings) | **up-to-date** | yes | bridge-ok | ok | — | — | ok | none | python | gitattr-ok; lock: acsi-agentskills@69882ea |
| [`dummy-gha/wsl-automation`](https://github.com/dummy-gha/wsl-automation) | **up-to-date** | yes | bridge-ok | ok | — | — | ok | none | none | gitattr-ok; lock: acsi-agentskills@69882ea |

> **Registry names discovery did not return (1):**
> - `skills-evals` — classified under `skills_bootstrap` in `repos.yml`, but no owner scanned this run returned it. A name to look at, not a verdict: it may be private, renamed, transferred, or unreadable by this run's credential. Confirm before removing the entry

## Plugin copies of the fleet guidance

Files [`dummy-gha/acsi-agentskills`](https://github.com/dummy-gha/acsi-agentskills) bundles verbatim from this repo. Anything but ok means its copy needs a refresh pull request there (a plugin runtime change, so it waits on that repo's review).

| Copy | Source here | Status | sha256 expected / found |
|------|-------------|--------|-------------------------|
| [`plugins/acsi-anything-anywhere/hooks/fleet-guidance/fleet-guidance.md`](https://github.com/dummy-gha/acsi-agentskills/blob/HEAD/plugins/acsi-anything-anywhere/hooks/fleet-guidance/fleet-guidance.md) | `agents-md/base.md` | **drifted** | `a09d766b` / `3cb4e75d` |
| [`plugins/acsi-anything-anywhere/hooks/fleet-guidance/fleet-memory.sh`](https://github.com/dummy-gha/acsi-agentskills/blob/HEAD/plugins/acsi-anything-anywhere/hooks/fleet-guidance/fleet-memory.sh) | `.claude/hooks/fleet-memory.sh` | ok | `9bb5d7dd` / `9bb5d7dd` |

---

**Status legend**

| Status | Meaning |
|--------|---------|
| **up-to-date** | Managed section and the contributor rules match their expected content, and no retired owner-only file is left |
| **drift-detected** | Managed section or contributor rules have diverged or are missing, or a retired owner-only file is left — needs sync |
| **pr-open** | A sync PR is already open for this repo |
| **no-agents-md** | Repo does not have an AGENTS.md yet |
| **update-failed** | An error occurred while checking this repo |
| **fetch-failed** | A file this row is built from could not be read, or could not be understood — the request failed for a reason this run could not resolve to a plain absence (a 401, a 403, a rate limit, a 5xx, a network fault, or a 404 on a repo the credential cannot see at all), or the decoded byte count disagreed with the API's own `size`, or the bytes arrived whole and would not parse. **Notes** names the file; every column it feeds is withheld as `?` rather than guessed; see issue #81 |

**Codex project-doc budget**

A `codex-truncated:` clause in **Notes** means that repo's AGENTS.md is
larger than Codex's `project_doc_max_bytes` (default 32768 bytes,
codex-cli 0.154.0). Codex cuts the file at that byte and says nothing a
session can see — measured 2026-09-14, a 55,788-byte AGENTS.md rendered
through `codex debug prompt-input` as exactly 32,768 bytes, ending
mid-heading. Claude Code reads the whole file and is unaffected, so this
is a note and never a status: the managed half is held under budget by
this repo's tests, the overflow is in `## Repo-specific additions`, and
only that repo can trim it. The sync warns and syncs anyway. See
`docs/decisions/0012-codex-gets-the-guidance-as-user-instructions.md`.

**Hook-script line endings**

Every row's **Notes** says `gitattr-ok` when the repo's root `.gitattributes`
carries `.claude/hooks/*.sh text eol=lf`, and `gitattr-missing` when it does
not; the next sync appends it. Without it, a Windows `core.autocrlf=true`
checkout gives the hook scripts CRLF and bash in WSL cannot parse them. A
note, never a status. See
`docs/decisions/0019-sync-pins-hook-scripts-to-lf-via-gitattributes.md`.

**CLAUDE.md bridge legend**

| Bridge status | Meaning |
|---------------|---------|
| bridge-ok | CLAUDE.md imports `@AGENTS.md` (line-start, outside code fences) |
| **no-import** | CLAUDE.md exists but never imports `@AGENTS.md` — Claude Code will not see the managed guidance |
| missing | No CLAUDE.md yet — sync adds the bridge in its next PR |
| ? | `CLAUDE.md` could not be read — withheld, not guessed (the row reads **fetch-failed**) |

**skills-bootstrap legend**

Delivery is opt-in and double-keyed: the repo must be listed in
`repos.yml`'s `skills_bootstrap.repos` **and** already carry its own
`skills.lock`. See `docs/decisions/0001-skills-bootstrap-delivery-is-opt-in.md`.

| Status | Meaning |
|--------|---------|
| ok | Hook present, byte-equal to the pinned copy, and registered in `.claude/settings.json` |
| **no-entry** | Hook is present but **nothing runs it** — no SessionStart entry names it. Silently dead |
| **drifted** | Hook present but differs from the pinned copy — the next sync overwrites it |
| **stale-matcher** | Hook registered with the legacy SessionStart matcher `startup\|resume`, so a forked session skips it — the next sync widens it to `startup\|resume\|fork` |
| **missing** | Allowlisted and has a lock, but no hook — the next sync delivers it (unless the pinned hook was unavailable fleet-wide that run; the sync log says `pinned hook unavailable this run`) |
| **blocked** | Allowlisted and has a lock, but the repo gitignores `.claude/` — `git add` cannot stage the hook, so every sync skips it with a warning. Does **not** self-heal: change that repo's `.gitignore`, or drop it from the allowlist |
| **refused** | Allowlisted and has a lock, but `.claude/settings.json` is not parseable JSON — the sync will not edit it, and withholds the hook rather than leave one nothing runs. Does **not** self-heal: fix that file |
| **degraded** | Hook present in a repo with no `skills.lock` — it prints `skills: DEGRADED` into every session and no sync will revisit it. Commit a lock, or remove the hook |
| no-lock | Allowlisted, no `skills.lock` yet — delivery deliberately withheld until the repo declares its bundles |
| **unmanaged** | Hook present in a repo that is **not** allowlisted — it still runs; the sync has no delete path, so remove it by hand |
| unverified | Could not fetch the pinned hook this run — the drift comparison was skipped |
| — | Not allowlisted and no hook present |
| ? | A file this column is decided from (the hook, `skills.lock`, `.claude/settings.json`, or the repo's ignore rules) could not be read — withheld, not guessed (the row reads **fetch-failed**) |

**fleet-memory, fleet-guidance and contributor-rules legend**

Since ADR 0043 a consumer repo no longer carries the owner-only guidance:
the **fleet-memory** column is `.claude/hooks/fleet-memory.sh` and the
**fleet-guidance** column is `.claude/hooks/fleet-guidance.md`, and the
sync removes both. The contributor-rules column compares
`.github/fleet-contributor-rules.md` with its source, which the sync
delivers in both AGENTS.md modes.

| Status | Meaning |
|--------|---------|
| — | (fleet-memory, fleet-guidance) Absent, as expected |
| **leftover** | (fleet-memory, fleet-guidance) Present, whatever its bytes — drift; the next sync removes it |
| ok | (contributor-rules) Present and byte-equal to the source in this repo |
| **drifted** | (contributor-rules) Present but its bytes differ from the source |
| **missing** | (contributor-rules) Absent; the next sync delivers it |
| ? | Contents read failed or was partial — withheld; the row reads **fetch-failed** |

**Cron-coverage classification**

`scripts/check-cron-coverage.js` audits a disk, so it cannot notice a
repo that is not checked out; `repos.yml`'s `cron_coverage.fleet` is what
makes an absent one an error there. This report is the only thing that
sees the whole account, so it is where a repo classified by neither key
surfaces — as the block above, or not at all when every repo is
classified. See `docs/decisions/0003-cron-coverage-is-fleet-listed.md`.

The **Notes** column carries each allowlisted repo's lock pins
(`registry@shortref`, one per federated source). Nothing else in the
fleet surfaces a stale lock: a lock pinned far behind still installs
cleanly and still reports `OK` in-session, by design.

## Repo-specific guidance overlap (warn-only)

Compared 97 blocks from the `## Repo-specific additions` part of 4 repos. Nothing here fails the run. Each entry is input for the weekly review in the README ("Repeated guidance across repos"), which decides whether to promote it to base.md, a skill or an agent-check, or delete the local copy. Repo text is not quoted because this report is public and some repos are private.

### Repeated across repos (0)

None.

### Local copies of shared guidance (1)

- [`dummy-gha/wsl-automation` L232-234](https://github.com/dummy-gha/wsl-automation/blob/HEAD/AGENTS.md#L232-L234) repeats 47% of `agents-md/base.md` "Pinning GitHub Actions"

## Language fit (warn-only)

Per repo: sections that do not match its languages, language statistics skewed by fixture, generated or vendored files, and manifests Dependabot does not cover. Nothing here fails the run; the nudge issue "Language fit findings" asks a fleet worker to fix each repo. Private repos show a category and a count only, because this report is public.

### Sections (0)

None.

### Linguist overrides (0)

- `dummy-gha/acsi-agentskills` (info): trace languages (under 1%): PowerShell (9,631 B)

### Dependabot manifests (1)

- `dummy-gha/acsi-agentskills-private`: no dependabot.yml, but the repo has manifests or workflows

### Not checked (0)

None.
