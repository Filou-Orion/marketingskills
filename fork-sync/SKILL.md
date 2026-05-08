---
name: fork-sync
description: "When the user wants to sync a forked repository with its upstream, merge upstream changes while preserving custom modifications, or manage a fork that has diverged from its origin. Use when the user mentions 'sync fork,' 'update fork,' 'upstream merge,' 'fork is behind,' 'merge upstream,' 'diverged fork,' 'keep fork up to date,' 'pull upstream changes,' or 'rebase on upstream.' This skill handles delta analysis, conflict resolution, and documentation of fork synchronization workflows."
metadata:
  version: 1.0.0
  author: filou-orion
  license: MIT
---

# Fork Sync

You are an expert at synchronizing forked repositories with their upstream origin. Your goal is to safely absorb upstream improvements while preserving every custom change the user has made — no data loss, no silent overwrites, no broken references.

## Before Starting

**Gather fork context before touching anything:**

1. **Upstream URL** — Where does the fork originate? Check `git remote -v` for an `upstream` remote or the GitHub fork indicator.
2. **Divergence** — How many commits ahead/behind? Run `git rev-list --left-right --count upstream/main...HEAD` or check GitHub's "This branch is N commits behind" banner.
3. **Last sync** — Does a `SYNC_LOG.md` exist? When was the last sync performed?
4. **Custom changes** — What has the user added or modified? Run `git log --oneline upstream/main..HEAD` to list local-only commits.
5. **Clean working tree** — Abort if `git status` shows uncommitted changes. Sync on a dirty tree risks losing work.

If the `upstream` remote is not configured:
```bash
git remote add upstream <original-repo-url>
git fetch upstream
```

---

## Phase 1: Delta Analysis

**Goal**: Understand what changed on both sides before making any changes.

### Upstream Side

1. **Fetch upstream** (do not merge yet):
   ```bash
   git fetch upstream main
   ```
2. **Count the gap**: `git rev-list --count HEAD..upstream/main` — how many upstream commits to absorb.
3. **List changed files**: `git diff --name-status HEAD...upstream/main` — categorize what moved.
4. **Check for version markers**: Look for VERSIONS.md, CHANGELOG.md, package.json, or release tags to understand what releases you are absorbing.

### Local Side

1. **List your commits**: `git log --oneline upstream/main..HEAD`
2. **List your changed files**: `git diff --name-only upstream/main..HEAD`
3. **Identify the overlap**: Files that appear in BOTH lists are potential conflict zones.

### Quick Assessment Output

Produce a summary table before proceeding:

```
Upstream commits to absorb:  N
Your commits since last sync: M
Files changed only upstream:  X (safe to overwrite)
Files changed only locally:   Y (safe to keep)
Files changed on both sides:  Z (need manual merge)
```

If Z = 0, the sync is trivial. If Z > 10, warn the user and suggest syncing more frequently.

---

## Phase 2: File Classification

Classify every changed file into exactly one category:

| Category | Condition | Action |
|----------|-----------|--------|
| **New upstream** | File exists upstream, not locally | Copy from upstream |
| **Modified upstream only** | File changed upstream, untouched locally | Overwrite with upstream version |
| **Modified locally only** | File changed locally, untouched upstream | Keep as-is (no action) |
| **Modified both sides** | File changed on both sides | Manual merge (Phase 3) |
| **Deleted upstream** | File removed upstream, exists locally | Evaluate — keep if it is your custom content, delete if it was upstream content |
| **Renamed upstream** | File moved/renamed upstream | Follow the rename, update local references |

### Decision Flowchart

```
For each changed file:
  ├─ Exists only upstream? → COPY
  ├─ Exists only locally? → KEEP
  ├─ Changed only upstream? → OVERWRITE
  ├─ Changed only locally? → KEEP
  ├─ Changed on both sides?
  │   ├─ Changes in different sections? → likely auto-mergeable
  │   └─ Changes overlap? → MANUAL MERGE (Phase 3)
  └─ Deleted upstream?
      ├─ Your custom file? → KEEP
      └─ Was upstream content? → DELETE
```

---

## Phase 3: Sync Execution

### Step 1: Try the Automated Path First

**Option A — Direct merge (preferred):**
```bash
git merge upstream/main --no-edit
```
Use merge, not rebase, when absorbing many upstream commits. Rebase replays each of your commits individually against the upstream — if you have 5 commits and there are conflicts, you resolve them up to 5 times instead of once.

**Option B — Cross-fork pull request (when direct merge is blocked):**
If the environment blocks `git fetch upstream` (sandbox, proxy, CI), create a cross-fork PR via GitHub:
- Base: your fork's main branch
- Head: `upstream-owner:main`
- This lets GitHub compute the merge server-side

**Option C — Manual file download (last resort):**
When both A and B fail, download files individually:
```bash
curl -sL "https://raw.githubusercontent.com/OWNER/REPO/main/path/to/file" -o "path/to/file"
```
This is tedious but reliable. Use it for sandboxed environments with no upstream git access.

### Step 2: Resolve Conflicts

When git reports conflicts, follow this principle:

> **Upstream as base, your additions layered on top.**

Do NOT try to "merge" line by line. Instead:
1. Take the upstream version of the file as the starting point
2. Re-apply your custom additions (sections, entries, configurations) on top
3. Verify nothing was lost from either side

**Common conflict types and resolution strategies:**

| Conflict Type | Example | Strategy |
|---------------|---------|----------|
| **Manifest / registry** | package.json, marketplace.json | Take upstream version, add your entries back |
| **Tables / lists** | README skill table, CHANGELOG | Take upstream rows, insert your rows alphabetically |
| **YAML frontmatter** | Version bumps, field changes | Take upstream values (they are newer), keep your custom fields |
| **Content sections** | Both sides added sections to same file | Keep both sections, verify ordering |
| **Counter / metadata** | "35 skills" vs "38 skills" | Recalculate from actual count |

### Step 3: Check for Semantic Conflicts

Git only detects textual conflicts. These silent issues are more dangerous:

- **Missing sections**: You added Section A at the end of a file. Upstream restructured the file. Your section is present but now appears in the wrong context.
- **Broken references**: Upstream renamed `docs/setup.md` to `docs/getting-started.md`. Your files still link to the old path.
- **Stale counters**: README says "42 tools" but the actual count is now 45.
- **Duplicate functionality**: You added a "Tool Integrations" section. Upstream independently added a "Tools Referenced" section with overlapping content.

**How to catch them:**
1. Search for internal links and verify targets exist: `grep -r "](.*\.md)" --include="*.md" | grep -v node_modules`
2. Count items in registries/manifests and compare to stated totals
3. Check for duplicate section headers: `grep "^## " file.md | sort | uniq -d`
4. If a validation script exists, run it

---

## Phase 4: Validation

Run through this checklist after every sync:

- [ ] All new upstream files are present locally
- [ ] All your custom files are intact (compare against your commit list from Phase 1)
- [ ] No duplicate sections in merged files (`grep "^## " file.md | sort | uniq -d`)
- [ ] No broken internal references (links to moved/renamed files)
- [ ] Manifests and registries updated (correct counts, your entries included)
- [ ] Version numbers and counters reflect the merged state
- [ ] Project validation passes (linter, tests, build — whatever the project uses)
- [ ] Commit message follows project conventions

**If the project has no validation script**, create a minimal check:
```bash
# Count expected items and compare
echo "Expected: N items"
ls path/to/items/ | wc -l

# Check for broken markdown links
grep -rn ']\(.*\.md\)' --include='*.md' | while read line; do
  file=$(echo "$line" | grep -oP ']\(\K[^)]+')
  [ ! -f "$file" ] && echo "BROKEN: $line"
done
```

---

## Phase 5: Documentation

### Create or Update SYNC_LOG.md

After every sync, document what happened. Use the template in [references/sync-log-template.md](references/sync-log-template.md).

A sync log serves three purposes:
1. **Audit trail** — What changed and why, for future you or collaborators
2. **Conflict memory** — How specific conflicts were resolved, so the next sync goes faster
3. **Drift detection** — Tracking how far you diverge over time

### Commit Message Format

```
chore: sync fork with upstream vX.Y.Z (N1 → N2 items)

Absorbed M upstream commits. Added [new items]. Merged [conflict files].
Resolved N conflicts while preserving all custom changes.
See SYNC_LOG.md for details.
```

### What NOT to Commit

- Merge artifacts (`.orig` files, conflict markers `<<<<<<<`)
- Unrelated changes discovered during sync (create a separate commit)
- Temporary diagnostic files

---

## Anti-Patterns

**These will cause data loss or make future syncs harder:**

| Anti-Pattern | Why It Is Dangerous | Do This Instead |
|-------------|--------------------|----|
| `gh repo sync --force` | Deletes all your custom commits | Use merge or PR-based sync |
| `git rebase` on 50+ upstream commits | Replays your commits one-by-one; conflicts multiply | Use `git merge` for large gaps |
| `-Xtheirs` merge strategy | Upstream always wins; your changes silently disappear | Merge manually, verify both sides |
| `git push --force` to shared branches | Rewrites history others depend on | Push normally after merge |
| Months without syncing | "Big bang merge" with dozens of conflicts | Sync at least every major upstream release |
| Monolithic custom commits | One giant commit touching 40 files is impossible to selectively reapply | Small, thematic commits |
| Renaming files upstream hasn't renamed | Future merges will conflict structurally | Extend files, don't restructure them |

---

## Sync Frequency Guidelines

| Upstream Activity | Recommended Sync Frequency |
|-------------------|---------------------------|
| Daily commits | Weekly or bi-weekly |
| Monthly releases | After each release |
| Quarterly major versions | After each major version |
| Dormant / archived | Only when you need a specific fix |

The cost of a sync scales with the gap. Small, frequent syncs are cheap. Large, rare syncs are painful.

---

## Task-Specific Questions

1. What is the upstream repository URL?
2. How many commits behind upstream are you?
3. What custom changes have you made to the fork? (New files, modified files, config changes)
4. When was the last time you synced?
5. Does the project have a validation script or test suite?
6. Are there any files you explicitly do NOT want to sync (e.g., custom configs)?
