# Genie Agent Curation Patterns

## Table of Contents

- Limits to Design Within
- Instruction Type Decision Table
- SQL Expressions
- Example SQL and Trusted Assets
- Join Relationships
- Prompt Matching
- Text Instructions Template
- Conflict Check
- Token Limit Relief

Facts verified against Azure Databricks docs, October 2026. Re-check limits before relying on them.

---

## Limits to Design Within

| Item | Limit | Counts toward it |
|------|-------|------------------|
| Data objects | 50 tables, views, or metric views | Each attached object |
| Instructions | 100 per agent | Each example SQL query, each SQL function, and the whole General instructions block (one) |
| Knowledge store snippets | 200 per agent | Table descriptions, join relationships, SQL expressions |
| Entity matching | 120 columns; 1,024 distinct values per column; 127 chars per value | String columns only |
| Benchmarks | 500 questions | Each question |

Column descriptions, synonyms, and prompt-matching settings do not count toward the snippet limit. A token limit also exists; Databricks does not publish the number. A warning appears as the agent approaches it, and messages stop when it is exceeded.

## Instruction Type Decision Table

| Need | Use | Why |
|------|-----|-----|
| Standard metric, filter, or derived attribute | SQL expression (Measure / Filter / Field) | Applied exactly as written; reusable |
| Multi-step or ambiguous question pattern | Example SQL | Genie matches prompts to verified SQL |
| Logic too complex for one query | Unity Catalog SQL function | Trusted asset; shareable |
| Join not declared as UC PK/FK | Join relationship | Correct JOIN cardinality |
| Global behavior (clarify, round, summarize) | Text instruction | Last resort only |

Databricks guidance: "Use text instructions only as a last resort." Never define a metric in text when a SQL expression can hold it.

## SQL Expressions

Add under **Configure > Examples > Add**, then choose **Filter**, **Measure**, or **Field**.

```text
Measure  Name: Net revenue
         Code: SUM(amount) - SUM(discount_amount)
         Synonyms: revenue, net sales
         Instructions: Excludes tax. Use for any revenue question.

Filter   Name: Closed won
         Code: stage = 'Closed Won'
         Synonyms: won deals, closed deals

Field    Name: Fiscal quarter
         Code: CONCAT('FY', YEAR(ADD_MONTHS(order_date, 11)), '-Q', QUARTER(ADD_MONTHS(order_date, 11)))
         Synonyms: FQ, fiscal qtr
         Instructions: Fiscal year starts February 1.
```

Filter code evaluates to a boolean; Measure code aggregates over rows; Field code transforms each row. Validate the fiscal-quarter logic against the business calendar before saving.

## Example SQL and Trusted Assets

- Title each example with the most typical phrasing of the user question.
- Add **Usage guidance** stating when the example applies.
- Parameterize with `:parameter_name` (types: String, Date, Date and Time, Decimal, Integer). Parameterized examples and SQL functions are trusted assets; answers built on them are labeled verified.
- Aim for at least five tested examples before exposing the agent through the API.

```sql
-- Title: What was net revenue by region for a quarter?
-- Usage guidance: Use when a user asks for revenue split by region for one fiscal quarter.
SELECT r.region_name,
       ROUND(SUM(o.amount) - SUM(o.discount_amount), 2) AS net_revenue
FROM sales.gold.fct_orders o
JOIN sales.gold.dim_region r ON o.region_id = r.region_id
WHERE o.fiscal_quarter = :fiscal_quarter
GROUP BY r.region_name
ORDER BY net_revenue DESC
```

## Join Relationships

Declare PK/FK constraints in Unity Catalog first; Genie saves them as joins automatically. Add manual joins only for undeclared keys. Choose cardinality (Many to one, One to many, One to one); use **Use SQL expression** for non-equi or multi-condition joins.

## Prompt Matching

- Format assistance and entity matching are enabled per column. Keep entity matching for low-cardinality string columns users name in questions (region, product line, status).
- Tables with row filters and masked columns are excluded from prompt matching. Disable entity matching on views over filtered or masked tables and on dynamic views.
- Format-assistance values are generated with the author's permissions and become shared context; disable it on sensitive columns.

## Text Instructions Template

Keep one short General instructions block. Order: global rules, clarification rules, then summary rules last.

```text
Business context
- Fiscal year starts February 1. "This quarter" means the current fiscal quarter.
- Round currency to 2 decimals.

Clarification rules
- When users ask about pipeline performance without a region or time range,
  you must ask a clarification question first. Example: "Which region and
  fiscal quarter should I use?"

Instructions you must follow when providing summaries
- State the date range covered.
- Name the tables used.
```

Clarification rules need four parts: trigger condition, missing details, required action, example question. The summary heading text must match exactly; only text instructions affect summaries, and summary length cannot be controlled.

## Conflict Check

Before saving, confirm rounding, date logic, fiscal calendar, and metric definitions match across text instructions, SQL expressions, and example SQL. Conflicting guidance makes nondeterministic answers more likely.

## Token Limit Relief

1. Hide unneeded columns or expose a narrower view.
2. Remove descriptions that restate column names.
3. Prune overlapping example SQL.
4. Shorten text instructions.

Sources: learn.microsoft.com/azure/databricks/genie-agents/best-practices, /genie-agents/tune-quality, /genie-agents/troubleshooting, /genie-agents/monitor
