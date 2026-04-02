# auto-sync Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Create the `auto-sync` skill that keeps README, CHANGELOG, and ROADMAP synchronized with git history.

**Architecture:** A Claude Code skill with modular reference files for templates, parsing logic, and formatting rules. The main SKILL.md orchestrates the workflow, referencing focused helper files.

**Tech Stack:** Claude Code skill format (SKILL.md + reference/*.md files)

---

## File Structure

```
skills/auto-sync/
├── SKILL.md                    # Main skill - orchestrates workflow
└── references/
    ├── triggers.md              # Trigger phrases and detection
    ├── templates.md             # README/CHANGELOG/ROADMAP templates
    ├── new-project.md           # First-run behavior for new projects
    ├── existing-project.md      # Sync behavior for existing projects
    ├── changelog-format.md      # Keep a Changelog 1.1.0 rules
    ├── commit-parser.md         # Git commit parsing and filtering
    └── migration.md             # When and how to extract to separate files
```

---

## Tasks

### Task 1: Rename skill directory

**Files:**
- Modify: `skills/readme-generator/` → `skills/auto-sync/`

- [ ] **Step 1: Rename directory**

Run: `mv skills/readme-generator skills/auto-sync`

- [ ] **Step 2: Verify structure**

Run: `ls -la skills/auto-sync/`
Expected: Shows `SKILL.md` inside renamed directory

---

### Task 2: Create reference directory and trigger definitions

**Files:**
- Create: `skills/auto-sync/references/triggers.md`
- Modify: `skills/auto-sync/SKILL.md`

- [ ] **Step 1: Create triggers.md**

```markdown
# Trigger Phrases

## Slash Command
`/auto-sync`

## Natural Language
- "sync my docs"
- "auto-sync"
- "sync documentation"

## Detection
Match case-insensitive, allowing for punctuation variations.
```

- [ ] **Step 2: Update SKILL.md frontmatter with triggers**

Frontmatter additions:
```yaml
name: auto-sync
description: Keep project documentation synchronized with git history
trigger phrases:
  - "/auto-sync"
  - "sync my docs"
  - "auto-sync"
```

---

### Task 3: Create README template

**Files:**
- Create: `skills/auto-sync/references/templates.md`

- [ ] **Step 1: Write templates.md**

```markdown
# Templates

## README Structure

\`\`\`markdown
# {project_name}

{brief_description}

## Quick Links

- [Changelog](CHANGELOG.md) — full version history
- [Roadmap](ROADMAP.md) — detailed roadmap

## Changelog (Last 3 Versions)

{embedded_changelog}

[Full Changelog →](CHANGELOG.md)

## Roadmap

{embedded_roadmap}

[Full Roadmap →](ROADMAP.md)
\`\`\`

## CHANGELOG (Embedded - Last 3 Versions)

\`\`\`markdown
### [{version}] - {date}
### [{version}] - {date}
### [{version}] - {date}

[Full Changelog →](CHANGELOG.md)
\`\`\`

## ROADMAP (Embedded)

\`\`\`markdown
### In Progress
- [ ] {item}

### Planned
- [ ] {item}

### Delivered
- [x] {item}

[Full Roadmap →](ROADMAP.md)
\`\`\`

## CHANGELOG.md (Full)

\`\`\`markdown
# Changelog

## [Unreleased]

### Added
### Changed
### Deprecated
### Removed
### Fixed
### Security

## [{version}] - {date}

### Added
- {change}
\`\`\`

## ROADMAP.md (Full)

\`\`\`markdown
# Roadmap

## In Progress
- [ ] {item}

## Planned
- [ ] {item}

## Delivered
- [x] {item}
\`\`\`
```

---

### Task 4: Create changelog format reference

**Files:**
- Create: `skills/auto-sync/references/changelog-format.md`

- [ ] **Step 1: Write changelog-format.md**

```markdown
# Keep a Changelog 1.1.0

## Section Types (in order)
- Added — new features
- Changed — changes in existing functionality
- Deprecated — soon-to-be removed features
- Removed — now removed features
- Fixed — bug fixes
- Security — vulnerability updates

## Rules
- ISO 8601 dates: YYYY-MM-DD
- Unreleased section at top for unpublished changes
- Group same types together
- Latest version first
- Semantic Versioning (major.minor.patch)

## Breaking Changes
Mark with \`!:\` in commit or \`BREAKING:\` prefix
```

---

### Task 5: Create commit parser reference

**Files:**
- Create: `skills/auto-sync/references/commit-parser.md`

- [ ] **Step 1: Write commit-parser.md**

```markdown
# Commit Parsing Rules

## Include
- feat:, feature:, add: → Added
- fix:, bugfix: → Fixed
- refactor:, restructure: → Changed
- docs:, documentation: → Changed
- deprecate: → Deprecated
- remove:, delete: → Removed
- security:, vuln: → Security
- breaking, BREAKING → Breaking Change (add to Changed/Removed)

## Exclude (skip)
- fix typo
- lint
- merge
- bump
- deps
- update readme (from automated tools)
- ci
- chore

## Categorization
First word/prefix determines type.
Extract subject line after prefix.
Preserve issue/PR references in parentheses.
```

---

### Task 6: Create new project behavior

**Files:**
- Create: `skills/auto-sync/references/new-project.md`

- [ ] **Step 1: Write new-project.md**

```markdown
# New Project Behavior

## Detection
- No git history (or < 3 commits)
- No existing README.md
- No CHANGELOG.md

## Flow
1. Detect new project
2. Ask user for project name (default: directory name)
3. Ask user for brief description
4. Ask user for rough ROADMAP ideas (not commitments)
   - "What features are you thinking about?"
   - "Any planned milestones?"
5. Generate files:
   - README.md (from template)
   - CHANGELOG.md (with Unreleased section)
   - ROADMAP.md (with user's ideas as unchecked items)
6. Present to user for review
7. Write if approved
```

---

### Task 7: Create existing project behavior

**Files:**
- Create: `skills/auto-sync/references/existing-project.md`

- [ ] **Step 1: Write existing-project.md**

```markdown
# Existing Project Behavior

## Detection
- Has git history (> 3 commits)
- Existing README.md exists

## Flow
1. Read README.md from git history (last N commits)
2. Parse existing embedded CHANGELOG/ROADMAP
3. Run \`git log --oneline\` to get commits since last doc update
4. Parse commits using commit-parser rules
5. Categorize changes into changelog types
6. Detect version from package.json or git tags
7. Update embedded sections (last 3 versions)
8. Check migration thresholds
9. Present diff to user
10. Write if approved

## Version Detection
1. package.json version field
2. Git tags: v*.*.* or *.*.*
3. Commit messages: "release:", "v"
```

---

### Task 8: Create migration logic

**Files:**
- Create: `skills/auto-sync/references/migration.md`

- [ ] **Step 1: Write migration.md**

```markdown
# Migration Logic

## Thresholds
| Content | Embedded Limit | Action |
|---------|----------------|--------|
| CHANGELOG versions | 3 | Show last 3 only |
| ROADMAP items | 15 total | Extract to ROADMAP.md |

## Extraction Process
1. Count items in embedded sections
2. If exceeds threshold:
   - Move older/extra items to full file
   - Keep lean version in README
   - Add "→ Full {Type} →" link
3. Ensure full files exist before adding links

## Always Include in README
- Last 3 CHANGELOG versions
- Top 5-7 ROADMAP items per section (In Progress, Planned, Delivered)
- Links to full files
```

---

### Task 9: Write main SKILL.md

**Files:**
- Modify: `skills/auto-sync/SKILL.md`

- [ ] **Step 1: Write complete SKILL.md**

```markdown
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
```

---

### Task 10: Verify and test

**Files:**
- Review: All created files

- [ ] **Step 1: Verify file structure**

Run: `find skills/auto-sync -type f | sort`
Expected:
```
skills/auto-sync/SKILL.md
skills/auto-sync/references/changelog-format.md
skills/auto-sync/references/commit-parser.md
skills/auto-sync/references/existing-project.md
skills/auto-sync/references/migration.md
skills/auto-sync/references/new-project.md
skills/auto-sync/references/templates.md
skills/auto-sync/references/triggers.md
```

- [ ] **Step 2: Commit**

```bash
git add -A
git commit -m "feat: add auto-sync skill"
```

---

## Spec Coverage Check

| Spec Requirement | Task |
|------------------|------|
| Trigger phrases | Task 2 |
| Slash command / natural language | Task 2 |
| New project creates README + CHANGELOG + ROADMAP | Task 6 |
| User provides rough ROADMAP vision | Task 6 |
| Read existing README from git history | Task 7 |
| Parse git commits for changes | Task 7, Task 5 |
| Keep last 3 versions embedded | Task 8 |
| CHANGELOG follows Keep a Changelog | Task 4 |
| ROADMAP with checkboxes | Task 3 |
| Migration to separate files | Task 8 |
| Links to full files in README | Task 3 |

All spec requirements covered.
