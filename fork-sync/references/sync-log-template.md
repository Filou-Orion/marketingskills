# Sync Log Template

Copy this template into your project as `SYNC_LOG.md` and fill it in after each sync.

---

## Sync: YYYY-MM-DD

**Upstream**: `owner/repo` vX.Y.Z (N items)
**Fork before sync**: vA.B.C + M custom commits (K items)
**Fork after sync**: K+new items

### Method

_Describe how the sync was performed: git merge, cross-fork PR, manual download, etc._

### Upstream Changes Absorbed

#### New Items
_List new files, features, or content added by upstream since last sync._

| Item | Version | Description |
|------|---------|-------------|
| example-item | 1.0.0 | What it does |

#### Improvements to Existing Items
_List modifications to files that already existed._

- `file-a`: Description of change
- `file-b`: Description of change

#### Infrastructure / Tooling Changes
_List non-content changes: CI, configs, scripts, dependencies._

### Conflicts Resolved

| File | Our Change | Upstream Change | Resolution |
|------|-----------|-----------------|------------|
| path/to/file | What we changed | What upstream changed | How we resolved it |

### Post-Merge Adjustments

_List any manual fixes applied after the merge (counter updates, reference fixes, etc.)._

1. Updated X
2. Fixed Y
3. Added Z

### Validation Result

```
Validation command and output here
```

### Open Items / Next Steps

- [ ] Any remaining issues to address
- [ ] Upstream features to evaluate for adoption
- [ ] Custom changes to consider upstreaming
