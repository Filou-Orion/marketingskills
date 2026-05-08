# Upstream Sync Log

## Sync: 2026-05-08

**Upstream**: `coreyhaines31/marketingskills` v1.10.0 (41 skills)
**Fork before sync**: `filou-orion/marketingskills` v1.6.0 + 4 custom commits (36 skills)
**Fork after sync**: 42 skills (41 upstream + webinar-marketing)

### Method

Standard `git fetch` from upstream was blocked by the sandbox proxy (502 on `coreyhaines31/marketingskills`). Workaround:

1. Created cross-fork PR via MCP (`coreyhaines31:main` -> `filou-orion:main`), PR #2
2. GitHub reported merge conflicts (`mergeable_state: dirty`) — 37 changed files, 81 upstream commits
3. Downloaded all 37 upstream files via `curl` from `raw.githubusercontent.com`
4. Manually merged conflict files, overwrote non-conflicting files, created new files
5. Closed PR #2 (no longer needed after manual sync)

### Upstream Changes Absorbed

#### 6 New Skills (v1.7.0 - v1.10.0)

| Skill | Version | Added In | Description |
|-------|---------|----------|-------------|
| aso-audit | 1.0.0 | v1.8.0 | App Store Optimization audits |
| competitor-profiling | 1.0.0 | v1.8.0 | Competitive intelligence via Firecrawl + DataForSEO |
| directory-submissions | 1.0.0 | v1.8.0 | Product Hunt, G2, AI directories, backlink strategy |
| image | 1.0.0 | v1.9.0 | AI image generation, design tools, OG images |
| video | 1.0.0 | v1.9.0 | AI video production (Hyperframes, HeyGen, Veo, Runway) |
| co-marketing | 1.0.0 | v1.10.0 | Partner identification, joint campaigns |

#### Improvements to Existing Skills

- `seo-audit`: Added International SEO & Localization section (hreflang, canonicalization, i18n sitemaps)
- `paid-ads`: Added conversion tracking reference (Google, Meta, LinkedIn, TikTok pixel setup)
- `social-content`: Bumped to v1.3.0 with Short-Form Video section (TikTok, Reels, Shorts)

#### Infrastructure Changes

- Zapier SDK integration for 8,000+ app access
- CLI security hardening: Supermetrics API key moved to header, ZoomInfo JWT masked by default
- Fixed plugin loading: removed `./` prefix from marketplace.json skill paths (#243)
- Fixed `source` field in marketplace.json for Claude Code schema validation (#270)
- Fixed community-marketing YAML frontmatter (#240)
- Fixed Zapier webhook URL validation (#247)
- Added HeyGen and Hyperframes tool integration guides
- Updated tools/REGISTRY.md with new tools

### Conflicts Resolved

| File | Our Change | Upstream Change | Resolution |
|------|-----------|-----------------|------------|
| `.claude-plugin/marketplace.json` | +webinar-marketing, v1.0.0 | +6 skills, v1.10.0, path fixes | Upstream base + webinar-marketing = 42 skills |
| `README.md` | +webinar-marketing row | +6 skill rows, diagram update | Upstream base + webinar-marketing in table + Growth Engineering category |
| `skills/community-marketing/SKILL.md` | +Tool Integrations, +References | YAML fix (quoted description) | Applied YAML fix + restored Task-Specific Questions + Related Skills + kept our additions |
| `skills/seo-audit/SKILL.md` | +Tool Integrations section | +International SEO section | Both sections kept (different positions in file) |
| `skills/social-content/SKILL.md` | +Tool Integrations section | +Short-Form Video section, v1.3.0 | Both sections kept, version bumped to 1.3.0 |

### Post-Merge Adjustments

1. **VERSIONS.md**: Added `webinar-marketing v1.0.0` entry + sync changelog
2. **REPO_GUIDE.de.md**: Updated skill count from 35 to 42, added all 7 new skills to categories
3. **community-marketing fix**: Restored missing Task-Specific Questions and Related Skills sections that were accidentally dropped in our original Tool Integrations commit

### Validation Result

```
bash validate-skills.sh
  Passed: 42
  Warnings: 0
All skills are valid!
```

### Files Changed (37 from upstream + 4 post-merge adjustments)

**New files created (23):**
- 6 SKILL.md files (aso-audit, co-marketing, competitor-profiling, directory-submissions, image, video)
- 12 reference files across new skills
- 3 reference files for existing skills (conversion-tracking, international-seo, short-form-video)
- 2 tool integration guides (heygen.md, hyperframes.md)

**Modified files merged (5 with conflicts):**
- .claude-plugin/marketplace.json
- README.md
- skills/community-marketing/SKILL.md
- skills/seo-audit/SKILL.md
- skills/social-content/SKILL.md

**Modified files overwritten (9, no conflicts):**
- .gitignore, VERSIONS.md, skills/paid-ads/SKILL.md
- tools/REGISTRY.md, tools/clis/supermetrics.js, tools/clis/zapier.js, tools/clis/zoominfo.js
- tools/integrations/zapier.md

**Post-merge edits (3):**
- VERSIONS.md (webinar-marketing entry)
- REPO_GUIDE.de.md (skill count + categories)
- SYNC_LOG.md (this file)

### Our Custom Changes Preserved

All 4 commits from our fork remain intact:
1. `docs: add German repo evaluation guide` (REPO_GUIDE.de.md)
2. `fix: resolve validation warnings and add missing references` (references for 5 skills)
3. `feat: add tool integrations to 18 skills and strengthen 2 weak skills` (30 skills now have Tool Integrations)
4. `feat: add webinar-marketing skill` (SKILL.md with complete webinar framework)
