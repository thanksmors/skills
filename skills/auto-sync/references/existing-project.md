# Existing Project Behavior

## Detection
- Has git history (> 3 commits)
- Existing README.md exists

## Flow
1. Read README.md from git history (last N commits)
2. Parse existing embedded CHANGELOG/ROADMAP
3. Run `git log --oneline` to get commits since last doc update
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
