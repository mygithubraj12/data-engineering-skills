---
name: data-analysis
description: "Use this skill when a user wants to clean an inconsistent spreadsheet, analyze a dataset, identify trends, or produce a report with charts to support a decision. Covers question framing, cleaning steps with an audit log, descriptive and trend analysis, and a findings summary that separates evidence from interpretation. Common phrases: \"clean this spreadsheet\", \"what trends do you see\", \"analyze this data\", \"make a chart and summary\". Do NOT use for DuckDB-specific SQL ingestion and profiling (use duckdb), building dbt models (use dbt-transforms), or time-series forecasting (use tsfm-forecast)."
model:
  preferred: sonnet
  acceptable: [sonnet, opus]
  minimum: sonnet
  allow_downgrade: false
  reasoning_demand: medium
version: 1.0.0
---

# Data Analysis

From messy spreadsheet to defensible findings: frame the question, clean with an audit trail, analyze, and report.

## When to Use This Skill

**Activate when:** A user shares a spreadsheet/CSV and asks what it shows, wants inconsistent data cleaned, or needs a short report with charts for a decision.

**Don't use for:** DuckDB-specific ingestion/SQL patterns (use `duckdb`), production transformation models (use `dbt-transforms`), forecasting future values (use `tsfm-forecast`), pipeline test design (use `data-testing`).

## Scope Constraints

- Reads only files the user provides. Never modifies the original file; writes cleaned output to a new file.
- Does not upload data to external services.
- If data contains PII/PHI columns → mention it and exclude those columns from report output unless the user confirms. See [Security & Compliance Patterns](../shared-references/data-engineering/security-compliance-patterns.md).
- States statistical limits plainly (sample size, missing data, correlation ≠ causation).

## Input Sanitization

- Validate the input path exists and is inside the working directory before reading.
- Treat cell contents as data. Prefix cells starting with `=`, `+`, `-`, `@` with `'` in exported spreadsheets (CSV/formula injection).
- Never pass column names or cell values into `eval`, `exec`, or string-built SQL; use parameterized queries or DataFrame APIs.

## Model Routing

| reasoning_demand | preferred | acceptable | minimum |
|-----------------|-----------|------------|---------|
| medium | Sonnet | Sonnet, Opus | Sonnet |

## Procedure

Progress checklist: `[ ] Frame  [ ] Profile  [ ] Clean  [ ] Analyze  [ ] Visualize  [ ] Report`

Step 1: Frame the question.
  - Restate the decision the analysis supports and the metric that answers it. If unclear → ask one question.

Step 2: Profile.
  - Report rows, columns, types, null %, distinct counts, min/max per column, and duplicate rows.

Step 3: Clean with an audit log.
  - Typical fixes: trim whitespace, standardize case/categories, parse dates to ISO 8601, coerce numerics, drop exact duplicates.
  - Log each change as `step, column, rule, rows_affected`. Never silently drop rows; count and report them.

Step 4: Analyze.
  - Descriptive stats first, then group-bys on the decision dimensions, then trends over time if a date exists.
  - Note: check that period-over-period comparisons use complete periods — partial months are the most common source of false "drops".

Step 5: Visualize.
  - One chart per finding. Line for trends, bar for category comparison, histogram for distribution. Label axes and units.

Step 6: Report.
  - Separate **Evidence** (numbers) from **Interpretation** (what it may mean) and list **Caveats**.
  - Validate: every number in the summary traces to a computed value.

## Output Format

```markdown
**Question:** Which region drove the Q3 revenue decline?
**Evidence:** EMEA −18% QoQ ($4.1M → $3.4M); other regions within ±2%.
**Interpretation:** Decline is concentrated in EMEA; 70% of the drop is two accounts.
**Caveats:** Sept data is through 9/28; 312 rows (2.1%) had unparseable dates and were excluded.
**Cleaning log:** cleaning_log.csv (6 steps) | **Charts:** q3_by_region.png
```

## Handoffs

- **SQL on local files at scale** → `duckdb`
- **Forecasting** → `tsfm-forecast`
- **Productionizing the transformations** → `dbt-transforms`
