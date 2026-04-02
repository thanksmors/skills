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
