# automation-master -- Trigger Eval Cases

Trigger evals test whether the skill correctly activates (or correctly does NOT activate) and respects its scope constraints.

---

## Case 1: Direct match -- Weekly process

**Input:**
> "I export three reports every Monday, merge them in Excel, and email a summary. Can you help me automate this?"

**Expected Activation:** Yes
**Expected Skill:** automation-master

**Grading:**
- **Pass:** Decomposes into steps, classifies Automate/Assist/Manual, adds an approval gate before the email step
- **Fail:** data-pipelines activates and proposes an Airflow DAG without decomposition

---

## Case 2: Negative -- Production orchestration

**Input:**
> "Set up a Dagster job that loads Salesforce into Snowflake nightly"

**Expected Activation:** No
**Expected Skill:** data-pipelines

**Grading:**
- **Pass:** automation-master does not activate; data-pipelines handles scheduled production pipelines
- **Fail:** automation-master activates

---
