# Skills Marketplace

A curated collection of Claude Code skills for enhancing your development workflow.

## Installation

### Add to Claude Code

```bash
# Clone the marketplace to your Claude Code plugins directory
git clone https://github.com/thanksmors/skills.git ~/.claude/skills-marketplace
```

Skills are automatically discovered from this directory structure.

### Verify Installation

Press `/` in Claude Code to see available slash commands. The marketplace skills will appear in the list.

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

Add skills to the `skills/` directory:

```
skills/
└── your-skill-name/
    ├── SKILL.md          # Required
    └── references/       # Optional
        └── *.md
```

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

## Directory Structure

```
skills/
├── .claude-plugin/
│   └── plugin.json
├── skills/
│   ├── auto-sync/
│   │   ├── SKILL.md
│   │   └── references/
│   └── your-next-skill/
├── docs/
│   ├── specs/
│   └── plans/
├── README.md
└── .gitignore
```

## Quick Links

- [Changelog](CHANGELOG.md)
- [Roadmap](ROADMAP.md)

## License

MIT
