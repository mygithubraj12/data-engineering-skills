# Eval Case: Triage a Failing Benchmark Run

## Metadata
- Case ID: 04-triage-results
- Tier: 3
- Route: Evaluate (Step 6)

## Input
```json
{"benchmark_results": "36 questions: 21 Good, 11 Bad, 4 Manual review needed. Bad reasons: 5 extra columns, 3 empty result for EMEA questions, 3 wrong join between orders and products producing duplicates. Manual: 4 questions without SQL Answer."}
```

## Expected Output
A root-cause table mapping each failure group to the smallest fix, plus an ordered re-run plan.

## Grading Rubric

### Must Pass
- [ ] Extra columns -> example SQL showing the expected projection
- [ ] Wrong join -> declare UC PK/FK or add a join relationship with correct cardinality
- [ ] Manual review items -> add SQL Answers
- [ ] Empty EMEA results -> check entity matching/value spelling and per-user row filters before changing instructions

### Should Pass
- [ ] Fixes one root cause at a time and re-runs selected questions, then the full set
- [ ] Reports baseline accuracy as 21/36 (58%) or explains manual items are excluded until reviewed

### Bonus
- [ ] Records results before the one-week result visibility window ends
