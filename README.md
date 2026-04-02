# Skills

A collection of Claude Code skills for enhancing your development workflow.

## Installation

### Step 1: Add the marketplace

```bash
claude plugin marketplace add thanksmors/skills
```

### Step 2: Install the plugin

```bash
claude plugin install skills@thanksmors-skills
```

Or inside a Claude Code session:

```
/plugin install skills@thanksmors-skills
```

### Verify

Run `/plugin` to open the plugin manager and confirm `skills` appears in the **Installed** tab.

## Available Skills

### auto-sync

**Keep project documentation synchronized with git history.**

Triggers:
- `/auto-sync`
- "sync my docs"
- "auto-sync"

Creates and updates:
- `README.md` — project documentation
- `CHANGELOG.md` — full version history
- `ROADMAP.md` — feature roadmap

**Features:**
- Detects new vs existing projects
- Maintains lean embedded versions in README with links to full files
- Shows last 3 changelog versions in README
- ROADMAP with checkboxes: `- [x]` delivered, `- [ ]` planned
- Follows [Keep a Changelog 1.1.0](https://keepachangelog.com/)

**For new projects:**
- Prompts for project name, description, rough roadmap ideas
- Generates all documentation files

**For existing projects:**
- Reads existing README from git history
- Parses commits for meaningful changes
- Updates documentation while preserving existing content

## Adding New Skills

Add skills to the plugin's `skills/` directory inside `plugins/skills/`:

```
plugins/skills/skills/
└── your-skill-name/
    ├── SKILL.md          # Required
    └── references/       # Optional
        └── *.md
```

Then register the new skill in `plugins/skills/.claude-plugin/plugin.json`.

### Skill Structure

Each skill needs a `SKILL.md` with frontmatter:

```yaml
---
name: your-skill-name
description: What this skill does in one line
argument-hint: [optional-args]
allowed-tools:
  - Read
  - Bash
  - Write
trigger phrases:
  - "/your-skill"
  - "trigger phrase"
---

# Skill Name

## Purpose

Describe what this skill does.

## Usage

When user triggers via phrase or slash command...

## Tips

- Helpful guidance
```

## Repository Structure

```
thanksmors/skills/
├── .claude-plugin/
│   ├── marketplace.json     # Marketplace catalog
│   └── plugin.json          # Legacy (not used by marketplace)
├── plugins/
│   └── skills/              # The installable plugin
│       ├── .claude-plugin/
│       │   └── plugin.json  # Plugin manifest
│       ├── skills/          # Auto-discovered skills
│       │   └── auto-sync/
│       │       ├── SKILL.md
│       │       └── references/
│       └── docs/
├── CHANGELOG.md
├── ROADMAP.md
└── README.md
```

## Quick Links

- [Changelog](CHANGELOG.md)
- [Roadmap](ROADMAP.md)

## License

MIT
