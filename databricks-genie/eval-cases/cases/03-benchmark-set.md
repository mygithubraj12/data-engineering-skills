# Eval Case: Build a Benchmark Set

## Metadata
- Case ID: 03-benchmark-set
- Tier: 2
- Route: Evaluate (Step 5)

## Input
```json
{"questions": ["Revenue by region last quarter", "Top 10 products by units this year", "Which reps are below quota?"]}
```

## Expected Output
Benchmark entries with 2-4 phrasings per question and one SQL Answer each, with fixed date literals and only requested columns.

## Grading Rubric

### Must Pass
- [ ] Provides 2-4 phrasings per question sharing one SQL Answer
- [ ] Replaces relative dates with fixed values in SQL Answers
- [ ] Keeps result sets under 5,000 rows or adds ORDER BY

### Should Pass
- [ ] Warns against copying benchmark questions into example SQL
- [ ] Returns only requested columns (extra columns rate Bad)

### Bonus
- [ ] Explains Chat vs Agent mode is selected per run
