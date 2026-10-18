# Genie Benchmark Evaluation

## Table of Contents

- Benchmark Design Rules
- Benchmark Set Template
- Chat Mode Ratings
- Agent Mode Ratings
- Running Benchmarks
- Root-Cause Triage
- Iteration Loop
- Reporting Template

Facts verified against Azure Databricks docs, October 2026.

---

## Benchmark Design Rules

- Source questions from real users (Monitor tab, request reviews, stakeholder interviews), not from the example SQL list.
- Write 2-4 phrasings per question with the same SQL Answer. Databricks recommends this range.
- Provide a **SQL Answer** for every Chat mode question; questions without one need manual review and get no automatic accuracy.
- Keep SQL Answer results under 5,000 rows, or add `ORDER BY` to both the answer and the expected output. Chat mode compares at most 5,000 rows.
- Return only the columns the question asks for. Extra columns rate **Bad**.
- Pin relative dates ("last quarter") to fixed values in the SQL Answer, or the ground truth drifts as data changes.
- Cover each audience question type: single metric, breakdown, trend, top-N, filter by entity, ambiguous (expects clarification).
- Each agent holds up to 500 benchmark questions. Benchmarks run as new conversations without thread context and are never used as Genie context.

## Benchmark Set Template

```yaml
- id: rev-by-region-q
  intent: Net revenue by region for one fiscal quarter
  phrasings:
    - "What was net revenue by region in FY2026 Q2?"
    - "Break down Q2 FY26 revenue by region"
    - "Revenue per region for fiscal Q2 2026"
  sql_answer: |
    SELECT r.region_name, ROUND(SUM(o.amount) - SUM(o.discount_amount), 2) AS net_revenue
    FROM sales.gold.fct_orders o
    JOIN sales.gold.dim_region r ON o.region_id = r.region_id
    WHERE o.fiscal_quarter = 'FY2026-Q2'
    GROUP BY r.region_name
    ORDER BY net_revenue DESC
  evaluation_note: "Lists every region with net revenue excluding tax."
```

## Chat Mode Ratings

| Condition | Rating |
|-----------|--------|
| SQL exactly matches SQL Answer | Good |
| Result set exactly matches | Good |
| Same data, different sort order | Good |
| Numeric values round to the same 4 significant digits | Good |
| Empty result set or error | Bad |
| Extra columns versus SQL Answer | Bad |
| Single-cell result differs | Bad |
| Cannot assess, or no exact match | Manual review needed |

## Agent Mode Ratings

An LLM judge grades Agent mode runs (multi-step reports). The optional **Evaluation note** guides the judge. Mode is chosen per run with the toggle on the **Benchmarks** tab, not per question.

## Running Benchmarks

- UI: **Benchmarks** > **Run benchmarks** or **Run selected** (requires CAN EDIT). Results appear on the **Evaluations** tab with Evaluation name, Execution status, Accuracy, and Created by.
- Result data stays visible for one week; the SQL remains afterward. Export or record results before then.
- CLI commands (Databricks CLI `genie` group):

```bash
databricks genie genie-create-eval-run "$SPACE_ID" --json @eval-config.json
databricks genie genie-list-eval-runs "$SPACE_ID"
databricks genie genie-get-eval-run "$SPACE_ID" "$EVAL_RUN_ID"
databricks genie genie-list-eval-results "$SPACE_ID" "$EVAL_RUN_ID"
databricks genie genie-get-eval-result-details "$SPACE_ID" "$EVAL_RUN_ID" "$RESULT_ID"
```

The CLI reference does not document the `eval-config.json` body. Check the REST API reference before scripting runs.

## Root-Cause Triage

| Symptom in generated SQL | Likely cause | Smallest fix |
|--------------------------|--------------|--------------|
| Wrong table or column chosen | Ambiguous names or weak descriptions | Improve column description, add synonyms, hide distractor columns |
| Metric computed differently | Metric undefined or defined only in text | Add a Measure SQL expression |
| Wrong filter value or misspelled entity | Prompt matching off for that column | Enable entity matching (if not masked or filtered) |
| Wrong join or duplicated rows | Missing or wrong cardinality | Declare UC PK/FK or add join relationship |
| Correct logic, extra columns | Over-eager SELECT | Example SQL showing the expected projection |
| Wrong date window | Fiscal calendar or "this quarter" undefined | Field expression plus one text rule |
| Complex question fails entirely | No pattern to follow | Parameterized example SQL or UC function |
| Answer differs from SQL Answer, both plausible | Ground truth ambiguous | Rewrite question or **Update ground truth** after review |
| Empty result for some users only | Row filters or permissions | Check per-user grants; benchmark as a representative user |

## Iteration Loop

1. Record baseline accuracy and the failing question IDs.
2. Fix one root cause at a time using the table above.
3. Re-run only the affected questions with **Run selected**, then the full set.
4. Keep the fix only if full-set accuracy does not drop. Revert otherwise.
5. Stop at the agreed target; add new real questions to the set after each release.

## Reporting Template

```markdown
| Run | Date | Questions | Good | Bad | Manual | Accuracy | Change applied |
|-----|------|-----------|------|-----|--------|----------|----------------|
| 1 | 2026-10-18 | 36 | 24 | 9 | 3 | 67% | Baseline |
| 2 | 2026-10-19 | 36 | 30 | 5 | 1 | 83% | Added net_revenue measure, region synonyms |
```

Sources: learn.microsoft.com/azure/databricks/genie-agents/monitor, /genie-agents/concepts, /dev-tools/cli/reference/genie-commands
