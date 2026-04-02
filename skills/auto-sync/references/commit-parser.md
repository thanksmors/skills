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
