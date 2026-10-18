# Eval Case: Scope a New Sales Agent

## Metadata
- Case ID: 01-scope-new-agent
- Tier: 1
- Route: Design (Steps 2-3)

## Input
```json
{"domain_goal": "Regional sales managers ask about weekly revenue, pipeline, and quota attainment", "tables": ["sales.gold.fct_orders", "sales.gold.dim_region", "sales.gold.dim_product", "sales.gold.fct_pipeline", "sales.gold.dim_rep", "sales.silver.orders_raw", "sales.silver.crm_events", "sales.gold.fct_quota"]}
```

## Expected Output
A scoping plan naming one purpose and one audience, a reduced table list (five or fewer, silver tables excluded or replaced with views), and a list of 10-20 candidate questions.

## Grading Rubric

### Must Pass
- [ ] Recommends five or fewer tables and excludes the silver tables
- [ ] States one audience and purpose
- [ ] Lists prerequisites: Unity Catalog, pro or serverless SQL warehouse, consumer `SELECT`

### Should Pass
- [ ] Suggests a view or metric view to pre-join pipeline and quota data
- [ ] Mentions declaring PK/FK constraints so joins are saved automatically

### Bonus
- [ ] Notes the 50-object hard limit
