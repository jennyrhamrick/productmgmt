---
name: okr-cascade
description: |
  Automatically generate pre-populated quarterly OKR trackers from Notion strategy templates. 
  Use this skill whenever you have a company/portfolio/team OKR hierarchy in Notion and need to create an Excel tracker for the quarter.
  Cascades company objectives → portfolio KRs → team OKRs in seconds. Saves 3-4 hours per quarter.
---

# OKR Cascade

Convert a hierarchical OKR strategy (company → portfolio → team) into a ready-to-use Excel tracker.

## When to Use

You have a Notion template with:
- Company-level objectives
- Portfolio-level KRs
- Individual team goals/OKRs
- Team ownership

You want a pre-populated Excel tracker where teams can immediately start tracking monthly progress.

## How It Works

**Input:** Notion template content (pasted as markdown or text)

**Processing:**
1. Parse the hierarchical structure (company → portfolio → team)
2. Extract objectives, KRs, team names, and any numeric baselines/targets
3. Generate a pre-populated Excel tracker with all data mapped

**Output:** `OKR_Tracker_[Quarter].xlsx` with:
- **Portfolio View**: Company objectives, portfolio KRs, team health rollup
- **Team 1-8 tabs**: Each team's objective and KRs pre-filled
- **Alignment Check**: Auto-mapped connections between team KRs and portfolio KRs
- All formulas intact, ready for monthly tracking

## Usage

```
/okr-cascade [quarter] [notion-content]
```

### Examples

**Paste Notion content directly:**
```
/okr-cascade Q2 2026 
# Company Objectives
- Increase platform stability to 99.95% uptime
- Grow user base by 30%

# Portfolio: Platform
- Krs: reduce latency by 40%, decrease error rate by 50%
- Owner: Platform Team

## Team 1: Infrastructure
- Objective: Achieve platform reliability SLOs
- KRs: 99.95% uptime, <50ms p99 latency
- Owner: Jane Smith

## Team 2: Payments
- Objective: Scale transaction processing
- KRs: 2B transactions/month, 99.9% settlement accuracy
- Owner: Alex Chen
```

**Link to Notion page (requires shared link):**
```
/okr-cascade Q2 2026 https://notion.so/my-okr-strategy
```

## Input Format

Notion markdown structure works best. Expected hierarchy:

```
# Company Objectives
- [objective 1]
- [objective 2]

# Portfolio: [Portfolio Name]
[portfolio details + KRs]

## Team [Number]: [Team Name]
- Objective: [objective]
- KRs: [KR 1], [KR 2]
- Owner: [Name]
```

## What Gets Extracted

- **Company objectives** → Portfolio View, top level
- **Portfolio KRs** → Portfolio View, linked to company objectives
- **Team names** → Auto-numbered Team 1-8 tabs
- **Team objectives** → Each team tab
- **Team KRs** → Pre-filled in KR table (5 rows per team)
- **Baselines/Targets** → If numeric values are present, extracted into Baseline/Target columns
- **Owners** → Extracted to Owner column

## Output Structure

The generated Excel file includes:

### Portfolio View Tab
- Company objectives table
- Portfolio KRs table with score formula: `(Current - Baseline) / (Target - Baseline)`
- Team health summary (avg score per team, confidence status)

### Team Tabs (Team 1 - Team 8)
- Team metadata (name, lead, quarter)
- Objective statement
- Key Results table with:
  - Baseline, Target, Monthly actuals (M1-M2-M3)
  - Auto-calculated Score (0.0-1.0 scale)
  - Confidence dropdown (On Track / At Risk / Off Track)
  - Owner field
- Conditional formatting: green ≥0.7, yellow 0.4-0.69, red <0.4

### Alignment Check Tab
- Maps each team KR to portfolio KRs
- Flags gaps (portfolio KRs with no team coverage)
- Shows alignment strength per mapping

## Handling Edge Cases

- **Missing team names**: Defaults to "Team 1", "Team 2", etc.
- **Missing baselines/targets**: Leaves those columns blank; teams fill in Excel
- **Malformed structure**: Logs warnings, includes what it could parse
- **More than 8 teams**: Creates all tabs needed (not limited to 8)

## What You Do After

1. Rename generic "Team X" labels if needed
2. Review Portfolio View for accuracy
3. Share team tabs with team leads
4. Teams fill in monthly actuals each month (M1, M2, M3)
5. Portfolio View auto-updates with team health rollup

## Tips

- **Notion tip**: Use a consistent template each quarter so parsing is reliable
- **Baseline/Target tip**: Include these as numbers in your Notion template and they'll be auto-extracted
- **Large portfolios**: If you have >8 teams, the skill generates however many tabs you need
- **Monthly tracking**: Monthly columns (M1, M2, M3) auto-calculate score; fill these each month

## Troubleshooting

**"Failed to parse hierarchy"**: Check that your Notion has clear section headers (e.g., "# Company Objectives", "## Team: [Name]")

**"Missing portfolio KRs"**: Make sure portfolio-level KRs are listed under a "# Portfolio" section

**"Team tabs are empty"**: Paste more complete Notion content with team objective and KR details

---

## Implementation

When invoked, this skill:

1. **Parses input** — Extract the quarter and Notion content (either pasted text or fetched from a link)
2. **Calls generate_okr_tracker.py** — Runs the Python script that parses the hierarchy and generates the Excel file
3. **Returns the file** — Saves `OKR_Tracker_[Quarter].xlsx` and provides it to the user

**Workflow:**
```bash
python scripts/generate_okr_tracker.py "[quarter]" "[notion_content]"
```

The script handles:
- Markdown parsing of company/portfolio/team structure
- Excel generation with formulas and formatting
- Error logging for malformed input

See `examples/sample-notion-input.md` for input format reference.

---

Created for product leaders. Saves ~3-4 hours per quarterly planning cycle.
