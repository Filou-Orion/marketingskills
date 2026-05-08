# Conflict Resolution Guide

Practical patterns for resolving merge conflicts in different fork scenarios. Each scenario describes the typical conflict types and proven resolution strategies.

---

## Scenario 1: Content Collection Fork

**Setup**: You forked a collection of content files (skills, plugins, templates). You added your own items and customized existing ones. Upstream keeps adding new items and improving existing ones.

**Typical conflicts:**

| Conflict | Example | Resolution |
|----------|---------|------------|
| Registry / manifest | `marketplace.json` lists 35 items (yours) vs 38 (upstream) | Take upstream manifest, add your items back, update total count |
| README table | Both sides added rows to a skill/plugin table | Take upstream table, insert your rows alphabetically |
| Shared metadata | Version counters, "total: N" statements | Recalculate from actual file count — never trust hardcoded numbers |
| YAML frontmatter | Upstream quoted a description field, you added a metadata field | Take upstream YAML structure, add your custom fields |

**Key principle**: Manifests and registries should always use the upstream version as base. Your entries are additions — they layer on top cleanly.

---

## Scenario 2: Configuration / Deployment Fork

**Setup**: You forked infrastructure-as-code (Terraform modules, Helm charts, Docker configs). You customized values for your environment. Upstream changes defaults and adds new options.

**Typical conflicts:**

| Conflict | Example | Resolution |
|----------|---------|------------|
| Default values | Upstream changed `replicas: 2` → `replicas: 3`, you had `replicas: 5` | Keep your value — your override is intentional |
| New required fields | Upstream added `securityContext:` block you do not have | Add the new field with upstream's default, then customize if needed |
| Removed deprecated options | Upstream removed `legacyMode: true` that you still use | Evaluate: is your usage still valid? If yes, keep and document. If no, migrate. |
| Version pins | Upstream bumped `image: app:2.5` → `app:3.0`, you pinned `app:2.3` | Decision required — do you need the old version? Check breaking changes. |

**Key principle**: Your overrides are intentional deviations. Document WHY each override exists so future syncs know whether to keep or drop them.

---

## Scenario 3: Translation / Localization Fork

**Setup**: You forked documentation to translate it. Upstream keeps updating the original language. Your translations need to track structural changes without losing translated content.

**Typical conflicts:**

| Conflict | Example | Resolution |
|----------|---------|------------|
| New sections | Upstream added "## Getting Started" section | Add the section with original-language content, mark it as untranslated |
| Restructured content | Upstream split one page into three | Follow the restructuring, redistribute your translations accordingly |
| Updated examples | Upstream changed code samples | Take the new code samples — code should not be translated |
| Removed content | Upstream deleted an outdated section you translated | Delete your translation too — keeping outdated translated content is worse than removing it |

**Key principle**: Structure follows upstream. Only prose content diverges (because it is translated). Mark untranslated sections clearly so they can be found later.

---

## Scenario 4: Framework Extension Fork

**Setup**: You forked a framework or library to add custom components, themes, or integrations. Upstream refactors internals and ships new features.

**Typical conflicts:**

| Conflict | Example | Resolution |
|----------|---------|------------|
| Import path changes | Upstream moved `src/utils/helpers.ts` → `src/lib/helpers.ts` | Update your imports to match. Search globally: `grep -rn "utils/helpers"` |
| API changes | Function signature changed from `init(config)` → `init(config, options)` | Update your calls. Check upstream migration guide if available. |
| Dependency bumps | Upstream bumped React 18 → 19 | Follow the bump — maintaining old dependencies creates compounding technical debt |
| Build config changes | Upstream switched from Webpack to Vite | This is a significant change — evaluate whether your custom build plugins have Vite equivalents before syncing |

**Key principle**: Keep your changes as isolated as possible. Custom components in their own directories. Extensions that do not modify upstream files. The less you touch upstream code, the fewer conflicts you will face.

---

## Universal Decision Tree

When you encounter ANY conflict you are unsure about:

```
Is the conflicting content YOUR custom addition?
├─ YES → Keep it. Layer it on top of the upstream version.
├─ NO → Was it upstream content you modified?
│   ├─ YES → Take upstream's new version. Re-apply your modification if still relevant.
│   └─ NO → It is pure upstream content. Take the upstream version.
└─ UNCLEAR → Ask the user. Show both versions and explain what each side changed.
```

**When in doubt, ask.** A 30-second question prevents a 30-minute rollback.
