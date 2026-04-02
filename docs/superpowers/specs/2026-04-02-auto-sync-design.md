# auto-sync Skill Design

## Overview

`auto-sync` is a Claude Code skill that keeps project documentation (README, CHANGELOG, ROADMAP) synchronized with git history. It maintains lean embedded versions in README while keeping full details in separate files.

## Trigger Phrases

- `/auto-sync` (slash command)
- "sync my docs"
- "auto-sync"

## Behavior

### For NEW Projects

1. Creates `README.md`, `CHANGELOG.md`, `ROADMAP.md`
2. Prompts user for rough vision/ideas for initial ROADMAP (not commitments)
3. Generates starter content

### For EXISTING Projects

1. Reads existing README from git history to understand continuity
2. Parses git commits to extract meaningful changes
3. Updates documentation while preserving existing content

## README Structure

```markdown
# Project Name

Brief description.

## Quick Links

- [Changelog](CHANGELOG.md) — full version history
- [Roadmap](ROADMAP.md) — detailed roadmap

## Changelog (Last 3 Versions)

### [1.2.0] - 2026-04-01
### [1.1.0] - 2026-03-15
### [1.0.0] - 2026-02-20

[Full Changelog →](CHANGELOG.md)

## Roadmap

### In Progress
- [ ] Feature in development

### Planned
- [ ] Planned feature

### Delivered
- [x] Delivered feature

[Full Roadmap →](ROADMAP.md)
```

## CHANGELOG Format

Follows [Keep a Changelog 1.1.0](https://keepachangelog.com/en/1.1.0/):

```markdown
## [Unreleased]

### Added
### Changed
### Deprecated
### Removed
### Fixed
### Security

## [1.0.0] - 2026-04-01
### Added
- Initial release
```

Types (in order):
- Added
- Changed
- Deprecated
- Removed
- Fixed
- Security

Rules:
- ISO 8601 dates (YYYY-MM-DD)
- Unreleased section at top for unreleased changes
- Link versions/sections when possible

## ROADMAP Format

```markdown
## In Progress
- [ ] Feature name

## Planned
- [ ] Planned feature

## Delivered
- [x] Delivered feature
```

Migration:
- If ROADMAP in README exceeds ~15 items, extract to `ROADMAP.md`
- Always show lean version in README (top 5-7 items per section)
- Full roadmap in `ROADMAP.md`

## Migration Behavior

| Content | Embedded Limit | Full File |
|---------|----------------|-----------|
| CHANGELOG | Last 3 versions | `CHANGELOG.md` |
| ROADMAP | ~15 items total | `ROADMAP.md` |

When migration triggers:
- Move older content to full file
- Add link to README pointing to full file
- Preserve embedded lean version

## Continuity Logic

1. Read existing README from git history (previous N commits)
2. Parse existing CHANGELOG/ROADMAP to understand what's already documented
3. Parse git commits since last documentation update
4. Merge: add new entries, preserve existing structure
5. Respect user's original phrasing/style preferences

## Version Detection

Detect version from:
1. `package.json` `version` field
2. Git tags matching `v*.*.*` or `*.*.*`
3. Commit messages with "release:" or "v" prefix

## Commit Parsing

Filter git commits for meaningful changes:
- Include: feature adds, bug fixes, refactors, docs updates, breaking changes
- Exclude: "fix typo", "lint", "merge", "bump deps", "update readme"

Categorize by:
- First line determines type (Added, Fixed, Changed, etc.)
- Breaking changes flagged with `!:` or `BREAKING:`
