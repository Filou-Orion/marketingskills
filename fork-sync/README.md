# Fork Sync — Agent Skill

An [Agent Skill](https://agentskills.io) that teaches AI coding agents how to sync forked repositories with upstream — safely preserving your custom changes while absorbing improvements.

## The Problem

Every fork sync tool does one of two things when it hits a conflict:
1. **Gives up** (GitHub "Sync fork", `gh repo sync`)
2. **Overwrites your changes** (`--force`, `-Xtheirs`)

Neither is acceptable when you have custom work to preserve.

## What This Skill Does

It gives your AI agent a structured playbook for fork synchronization:

1. **Delta Analysis** — Understand what changed on both sides before touching anything
2. **File Classification** — Categorize every file (new, modified upstream, modified locally, both)
3. **Sync Execution** — Try automated merge first, fall back gracefully, resolve conflicts with "upstream as base, your additions on top"
4. **Validation** — Catch broken references, duplicate sections, stale counters
5. **Documentation** — Maintain a sync log for audit trail and conflict memory

## Installation

### Claude Code / Any Agent Skills-compatible agent

```bash
# Copy into your project's skills directory
cp -r fork-sync/ .agents/skills/fork-sync/
```

### Manual

Copy `SKILL.md` and the `references/` directory into your agent's skill loading path.

## Usage

Ask your agent:
- "Sync my fork with upstream"
- "My fork is 50 commits behind, help me catch up"
- "Pull upstream changes without losing my customizations"

## Files

```
fork-sync/
├── SKILL.md                              # Main skill instructions
├── README.md                             # This file
└── references/
    ├── sync-log-template.md              # Template for documenting syncs
    └── conflict-resolution-guide.md      # 4 scenarios + decision trees
```

## License

MIT
