# Eval Case: Fix Inconsistent Revenue

## Metadata
- Case ID: 02-define-metric
- Tier: 2
- Route: Tune (Step 4)

## Input
```json
{"space_id": "0123456789abcdef0123456789abcdef", "symptom": "Revenue answers sometimes include tax and sometimes subtract discounts. Text instructions say 'revenue means net of discounts'."}
```

## Expected Output
A Measure SQL expression for net revenue, removal or alignment of the text instruction, and a conflict check across example SQL.

## Grading Rubric

### Must Pass
- [ ] Defines revenue as a Measure SQL expression with code, synonyms, and instructions
- [ ] Says text instructions are a last resort for metric definitions
- [ ] Checks existing example SQL for conflicting revenue logic

### Should Pass
- [ ] Recommends re-running affected benchmarks after the change

### Bonus
- [ ] Notes SQL expressions count toward the 200 knowledge store snippet limit
