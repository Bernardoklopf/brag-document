# Career Tracking

@../../AGENTS.md

This folder tracks ongoing career data grouped by company. Each company has its own subdirectory following the `_template-company/` skeleton.

## Structure

```
career-tracking/
├── _template-company/           ← skeleton for new companies
└── {company-name}/              ← kebab-case, one per company
    ├── achievements/
    │   └── {year}-q{n}.md       ← quarterly achievement log
    ├── performance-cycles/
    │   └── {year}-h{n}.md       ← per-cycle: self-assessment, feedback, ratings
    └── 3ps/
        └── {year}.md            ← one file per year, weekly entries as H3
```

## When the user shares new information

### Project achievements, deliverables, or wins
1. Identify the company → find or create the company directory
2. Determine the quarter → find or create the `achievements/{year}-q{n}.md` file
3. Add the entry under the correct week heading using action verbs and **bold metrics**
4. If the achievement is significant enough for the career summary, also suggest updating `source-of-truth/work-experience.md`

### Performance cycle data (self-assessment, manager feedback, peer feedback, ratings)
1. Identify the company and cycle period (H1/H2/Annual)
2. Find or create `performance-cycles/{year}-h{n}.md` (or `{year}-annual.md`)
3. Place content in the correct section: Self-Assessment, Manager Feedback, Peer Feedback, or Ratings/Outcomes
4. Keep feedback verbatim when provided — do not rewrite or summarize unless asked

### 3Ps (Progress/Plans/Problems)
1. Identify the company → find or create the company directory
2. Find or create `3ps/{year}.md`
3. Add a new H3 entry with the week number and date range: `### Week {N} ({Mon DD}–{Fri DD})`
4. Fill **Progress**, **Plans**, and **Problems** fields
5. If the user provides sprint information, include it: `### Week {N} ({Mon DD}–{Fri DD}) / Sprint {X}`

## Adding a new company

1. Create a new directory under `career-tracking/` using `kebab-case` (e.g., `my-company/`)
2. Create subdirectories: `achievements/`, `performance-cycles/`, `3ps/`
3. Use `.gitkeep` files if directories start empty
4. Follow the templates in `_template-company/` for file content structure

## Conventions

- **File naming:** `{year}-q{n}.md` for achievements, `{year}-h{n}.md` or `{year}-annual.md` for performance cycles, `{year}.md` for 3Ps
- **Week entries:** use H3 (`###`) with week number and date range
- **Metrics:** always bold key numbers and technologies
- **Verbatim feedback:** preserve the original language of feedback received — do not paraphrase
- **No invention:** never fabricate achievements, feedback, or metrics — all must come from the user
