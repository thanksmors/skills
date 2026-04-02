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
