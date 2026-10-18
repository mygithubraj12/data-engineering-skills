# databricks-genie -- Trigger Eval Cases

Trigger evals test whether the skill correctly activates (or correctly does NOT activate) on various user inputs. Distinct from navigation evals (file traversal) and output evals (result quality).

---

## Case 1: Direct Match -- Build a Genie Space for Sales

**Category:** Direct match
**Tier:** 1 (simple)

**Input:**
> "Help me set up a Genie space for our sales team on the gold tables"

**Expected Activation:** Yes
**Expected Skill:** databricks-genie

**Observation Points:**
- "Genie space" is a primary trigger phrase (legacy name for Genie Agent)
- Request maps to the Design path (Step 2 scoping)

**Grading:**
- **Pass:** Skill activates, asks for audience and target questions, and recommends five or fewer tables
- **Partial:** Skill activates but jumps to listing every gold table without scoping
- **Fail:** Skill does not activate; or ai-data-integration activates and proposes a custom NL-to-SQL build

---

## Case 2: New Name -- Genie Agent Accuracy

**Category:** Direct match
**Tier:** 1 (simple)

**Input:**
> "Our Genie agent keeps getting revenue wrong. How do I fix it?"

**Expected Activation:** Yes
**Expected Skill:** databricks-genie

**Observation Points:**
- Uses the current product name "Genie agent"
- Maps to the Tune path; the expected fix is a Measure SQL expression, not a text instruction

**Grading:**
- **Pass:** Skill activates and recommends defining revenue as a SQL expression (Measure) and checking for conflicting definitions
- **Partial:** Skill activates but recommends only adding a text instruction
- **Fail:** Skill does not activate

---

## Case 3: Casual Phrasing -- Chat With Our Data

**Category:** Casual phrasing
**Tier:** 2 (medium)

**Input:**
> "Business users want to ask questions about our Databricks data in plain English without writing SQL. What's the quickest way?"

**Expected Activation:** Yes
**Expected Skill:** databricks-genie

**Observation Points:**
- No "Genie" keyword; intent matches the managed Databricks NL-to-SQL product
- Competes with ai-data-integration (custom NL-to-SQL); on Databricks the managed option comes first

**Grading:**
- **Pass:** Skill activates and recommends a Genie Agent before any custom build, listing prerequisites (Unity Catalog, pro or serverless SQL warehouse)
- **Partial:** ai-data-integration activates but mentions Genie as an option
- **Fail:** A custom LLM pipeline is proposed with no mention of Genie

---

## Case 4: Evaluation -- Benchmark Questions

**Category:** Direct match
**Tier:** 2 (medium)

**Input:**
> "How should I write benchmarks to measure our Genie space accuracy before rollout?"

**Expected Activation:** Yes
**Expected Skill:** databricks-genie

**Observation Points:**
- Maps to the Evaluate path (Step 5)

**Grading:**
- **Pass:** Skill activates; recommends 2-4 phrasings per question with a SQL Answer, under 5,000 rows or ordered, and keeping benchmarks separate from example SQL
- **Partial:** Skill activates but gives generic LLM eval advice without Genie's SQL Answer mechanics
- **Fail:** data-testing activates and proposes dbt tests

---

## Case 5: API Integration -- Genie in an App

**Category:** Direct match
**Tier:** 2 (medium)

**Input:**
> "Call our Genie space from a Python app and show the SQL it generates"

**Expected Activation:** Yes
**Expected Skill:** databricks-genie

**Grading:**
- **Pass:** Skill activates and uses the Conversation API or SDK with polling, backoff, and a new conversation per session
- **Partial:** Skill activates but polls without backoff or timeout
- **Fail:** Skill does not activate; or code hardcodes a token

---

## Case 6: Negative -- PySpark Job on Databricks

**Category:** Negative
**Tier:** 1 (simple)

**Input:**
> "Write a PySpark job on Databricks that deduplicates orders and writes a Delta table"

**Expected Activation:** No
**Expected Skill:** python-data-engineering

**Grading:**
- **Pass:** databricks-genie does not activate
- **Fail:** databricks-genie activates because it sees "Databricks"

---

## Case 7: Negative -- Unity Catalog Row Filters

**Category:** Negative
**Tier:** 2 (medium)

**Input:**
> "Design row-level security for our Unity Catalog sales tables by region"

**Expected Activation:** No (unless the user ties it to a Genie Agent)
**Expected Skill:** data-governance

**Observation Points:**
- Row filters affect Genie (per-user access, prompt matching exclusions), but policy design belongs to governance

**Grading:**
- **Pass:** data-governance activates; databricks-genie stays inactive or appears only as a handoff
- **Fail:** databricks-genie leads the response with policy design

---

## Case 8: Ambiguous -- Metric Views

**Category:** Ambiguous
**Tier:** 3 (complex)

**Input:**
> "Should I build metric views for our KPIs?"

**Expected Activation:** Conditional (clarify consumer)
**Expected Skill:** databricks-genie (if the consumer is a Genie Agent), dbt-transforms or data-governance otherwise

**Grading:**
- **Pass:** Clarifies whether the metric views will serve a Genie Agent; if yes, explains that metric views pre-define measures and help stay within the 50-object limit
- **Partial:** Activates and answers for Genie without confirming the consumer
- **Fail:** Neither skill addresses the question
