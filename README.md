# Skills

A collection of Claude Code skills for enhancing your development workflow.

## Installation

### Add to Claude Code

Run the following slash command inside a Claude Code session:

```
/plugin install https://github.com/thanksmors/skills
```

Claude Code clones the repository, reads `.claude-plugin/plugin.json`, and discovers skills from the `skills/` directory automatically.

### Verify Installation

Run `/plugin` to open the plugin manager and confirm `skills-marketplace` appears in the **Installed** tab.

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

## Repository Structure

```
thanksmors/skills/
├── .claude-plugin/
│   └── plugin.json          # Plugin manifest
├── skills/                   # Auto-discovered skill directories
│   ├── auto-sync/
│   │   ├── SKILL.md         # Skill definition
│   │   └── references/      # Supporting docs
│   └── your-next-skill/
├── docs/
│   ├── specs/
│   └── plans/
├── CHANGELOG.md
├── ROADMAP.md
├── README.md
└── .gitignore
```

## Quick Links

- [Changelog](CHANGELOG.md)
- [Roadmap](ROADMAP.md)

## License

MIT
