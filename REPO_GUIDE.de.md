# Repo-Guide: marketingskills (Deutsch)

Eine vollständige Evaluation des Repos und wie du es in der Praxis einsetzen kannst.

## Was ist dieses Repo?

**marketingskills** ist zweierlei in einem:

1. **Eine Sammlung von 35 "Agent Skills"** für Marketing-Aufgaben, die der [Agent Skills Spec](https://agentskills.io/specification.md) folgen. Skills sind strukturiertes Wissen + Instruktionen, die KI-Agenten (Claude Code, Codex, Cursor, Windsurf, …) automatisch aktivieren, wenn du eine passende Marketing-Aufgabe beschreibst.
2. **Ein Marketing-Tool-Registry** (`tools/`) mit 94 Tools: 61 zero-dependency CLI-Scripts, 45+ Integration-Guides, Composio-Layer für OAuth-Tools, und Verweisen auf native MCP-Server.

Das Repo ist außerdem ein **Claude Code Plugin-Marketplace** (via `.claude-plugin/marketplace.json`), d.h. du kannst es in Claude Code mit einem Befehl installieren.

- **Autor**: Corey Haines
- **Lizenz**: MIT
- **Repo**: `coreyhaines31/marketingskills`

---

## Installation

**Für Claude Code** (empfohlen):
```
/plugin marketplace add coreyhaines31/marketingskills
/plugin install marketing-skills
```

**Agent-agnostisch** (Codex, Cursor, Windsurf, …):
```
npx skills add coreyhaines31/marketingskills
```

**Weitere Optionen**: Clone, Git Submodule, Fork & Customize, SkillKit.

Nach Installation liegen die Skills unter `.agents/skills/` (Standardort) und triggern automatisch, sobald du z.B. sagst "Hilf mir diese Landing Page optimieren" → `page-cro` wird aktiv. Du kannst auch direkt `/page-cro`, `/email-sequence` etc. aufrufen.

---

## Die 35 Skills — was kannst du damit abdecken?

### Conversion Rate Optimization (7)

| Skill | Wofür |
|-------|-------|
| `page-cro` | Marketing-Seiten (Home, Pricing, Landing) für Conversion optimieren |
| `signup-flow-cro` | Registrierungs-Flows verbessern |
| `form-cro` | Formulare (Lead, Demo, Kontakt) optimieren |
| `onboarding-cro` | Post-Signup-Aktivierung |
| `paywall-upgrade-cro` | Free→Paid Konversion, In-App-Paywalls |
| `popup-cro` | Popups, Modals, Exit-Intent |
| `ab-test-setup` | Statistisch valide A/B-Tests planen |

### SEO & Technical (5)

`seo-audit` · `ai-seo` (LLM/ChatGPT-Sichtbarkeit) · `programmatic-seo` (Seiten at scale) · `schema-markup` (JSON-LD) · `site-architecture`

### Paid Advertising (2)

`paid-ads` (Google/Meta/LinkedIn/TikTok/X) · `ad-creative` (Variationen at scale)

### Email (2)

`email-sequence` (Lifecycle, Welcome, Nurture, Win-Back) · `cold-email` (Outbound-Sequenzen)

### Content & Copy (4)

`content-strategy` · `copywriting` · `copy-editing` (7-Sweeps-Framework) · `social-content` (LinkedIn/X/Insta/TikTok)

### Customer Insight (1)

`customer-research` (Interviews, Surveys, Voice-of-Customer)

### Pricing & Revenue (2)

`pricing-strategy` · `revops` (Lead Scoring, Lifecycle, Sales Handoff)

### Awareness & Lead Gen (7)

`lead-magnets` · `free-tool-strategy` · `referral-program` · `competitor-alternatives` · `launch-strategy` (Product Hunt etc.) · `marketing-ideas` (139+ Taktiken) · `marketing-psychology`

### Sales & Positioning (2)

`sales-enablement` (Decks, One-Pager, Objection Docs) · `product-marketing-context` (Fundament-Skill, wird von allen anderen gelesen)

### Community (1)

`community-marketing` (Discord, Slack, Circle, Reddit)

### Analytics (1)

`analytics-tracking` (GA4, GTM, Events, UTM)

### Retention (1)

`churn-prevention` (Cancel Flows, Save Offers, Dunning)

**Fundament-Skill: `product-marketing-context`** — den solltest du zuerst laufen lassen. Er erzeugt eine Context-Datei (Positioning, ICP, Proof Points), die alle anderen Skills dann als Input nutzen.

---

## Die Tool-Registry — 94 Marketing-Tools

Unter `tools/` findest du drei Zugriffswege auf Marketing-Plattformen:

### 1. Zero-Dependency CLI-Scripts (`tools/clis/`, 61 Tools)

Node.js 18+ Scripts ohne Dependencies. Auth via Umgebungsvariablen, JSON-Output, unterstützen `--dry-run`. Abgedeckt:

- **Analytics**: GA4, Mixpanel, Amplitude, Segment, Adobe, Plausible
- **SEO**: Search Console, Semrush, Ahrefs, DataForSEO
- **Data Enrichment**: Clearbit, Apollo, Clay, ZoomInfo
- **Email**: Mailchimp, Resend, SendGrid, Postmark, Klaviyo, Customer.io, Brevo, Beehiiv, Kit, ActiveCampaign
- **Email Outreach**: Hunter, Snov, Lemlist, Instantly
- **Ads**: Google Ads, Meta Ads, LinkedIn Ads, TikTok Ads
- **CRM/Sales**: Close, Outreach, Crossbeam
- **Payments/Affiliate**: Paddle, Stripe, Rewardful, Tolt, PartnerStack, Mention Me
- und viele mehr (Webinare, CRO, Scheduling, Forms, Messaging, Reviews, Video, Social …)

Nutzung: `node tools/clis/<name>.js` ohne Args → Help; mit `<cmd> --dry-run` → Vorschau.

### 2. Native MCP-Server (13 Tools)

Direkt im Agent nutzbar: **GA4, Stripe, Mailchimp, Google Ads, Resend, Zapier, ZoomInfo, Clay, Supermetrics, Coupler, Outreach, Crossbeam, Introw**

### 3. Composio-Layer (`tools/composio/`)

Für OAuth-lastige Tools ohne native MCP: **HubSpot, Salesforce, Meta Ads, LinkedIn Ads, Google Sheets, Slack, Notion, Airtable, Klaviyo, Gmail, Shopify** — ein einziger MCP-Server deckt 500+ Tools ab.

Index + Capabilities → `tools/REGISTRY.md`. Details pro Tool → `tools/integrations/<tool>.md`.

---

## Typische Workflows

**Landing Page launchen:**
`customer-research` → `product-marketing-context` → `copywriting` → `page-cro` → `schema-markup` → `analytics-tracking` → `ab-test-setup`

**B2B-Lead-Gen aufsetzen:**
`lead-magnets` → `form-cro` → `email-sequence` → `revops` → CLI-Tools (Apollo, Clay, HubSpot via Composio)

**SEO-Content at Scale:**
`content-strategy` → `seo-audit` → `programmatic-seo` → `ai-seo` → CLI (Search Console, Ahrefs/Semrush)

**Paid-Acquisition-Sprint:**
`paid-ads` → `ad-creative` → `page-cro` → `analytics-tracking` → CLI (Google/Meta/LinkedIn Ads)

**Subscription retten:**
`churn-prevention` → `email-sequence` (Win-Back) → `paywall-upgrade-cro` → `pricing-strategy`

---

## Was dieses Repo praktisch bedeutet

- **Du beschreibst Marketing-Aufgaben in natürlicher Sprache** ("Schreib mir eine Cold-Email-Sequenz", "Meine Pricing Page konvertiert schlecht") und der Agent zieht den passenden Skill automatisch.
- **Die Skills sind nicht nur Prompts, sondern strukturierte Frameworks** mit `references/`-Unterordnern (Templates, Playbooks, Checklisten, Copy-Patterns).
- **Für die Ausführung** (Daten aus GA4 holen, Kampagne in Meta Ads starten, Kontakte in HubSpot anlegen) hast du direkt CLI-Scripts und MCP-Zugriffe in einem Repo.
- **Versionierung** via `VERSIONS.md`: Agenten prüfen einmal pro Session auf Updates.

---

## Kritische Dateien zum schnellen Einstieg

| Datei | Warum |
|-------|-------|
| `README.md` | Übersicht + Installation |
| `AGENTS.md` (symlink von `CLAUDE.md`) | Spezifikation + Repo-Regeln für Agenten |
| `.claude-plugin/marketplace.json` | Plugin-Manifest mit allen 35 Skills |
| `skills/product-marketing-context/SKILL.md` | Fundament-Skill — zuerst nutzen |
| `skills/marketing-ideas/SKILL.md` | 139+ fertige Marketing-Taktiken |
| `tools/REGISTRY.md` | Tool-Index |
| `tools/composio/marketing-tools.md` | OAuth-Tools (HubSpot, Salesforce, …) |
| `VERSIONS.md` | Update-Tracking |
