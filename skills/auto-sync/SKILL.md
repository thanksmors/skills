---
name: auto-sync
description: Keep project documentation (README, CHANGELOG, ROADMAP) synchronized with git history
argument-hint: [sync|check]
allowed-tools:
  - Read
  - Bash
  - Write
  - Glob
  - Grep
trigger phrases:
  - "/auto-sync"
  - "sync my docs"
  - "auto-sync"
---

# auto-sync

## Purpose

Keeps project documentation synchronized with git history. Creates and updates README.md, CHANGELOG.md, and ROADMAP.md with lean embedded versions while maintaining full details in separate files.

## Usage

When user triggers via phrase or slash command:

1. **Detect project state** — new or existing
2. **For new projects:**
   - Ask for project name, description, rough roadmap ideas
   - Generate README.md, CHANGELOG.md, ROADMAP.md
   - Present for review, write if approved

3. **For existing projects:**
   - Read existing README from git history
   - Parse git commits for meaningful changes
   - Update embedded sections (last 3 changelog versions, lean roadmap)
   - Check migration thresholds
   - Present diff for review, write if approved

## Detailed Behavior

See reference files for complete logic:
- references/new-project.md — first-run behavior
- references/existing-project.md — sync behavior
- references/changelog-format.md — Keep a Changelog rules
- references/commit-parser.md — commit categorization
- references/migration.md — when to extract to separate files
- references/templates.md — output templates

## Project Detection

**New project:** < 3 commits, no README.md
**Existing project:** ≥ 3 commits, README.md exists

## Version Detection

1. package.json version field
2. Git tags (v*.*.* or *.*.*)
3. Release commit messages

## Changelog Types (Keep a Changelog 1.1.0)

In order: Added, Changed, Deprecated, Removed, Fixed, Security

## ROADMAP Sections

- In Progress — features being developed
- Planned — intended features
- Delivered — completed features

## Migration Triggers

- CHANGELOG: > 3 versions → link to CHANGELOG.md
- ROADMAP: > 15 items → extract to ROADMAP.md

## Tips

- Keep descriptions concise and project-specific
- Extract version and dependencies from package.json if available
- Use inline code formatting for commands and file paths
- Preserve user's original phrasing in existing docs
