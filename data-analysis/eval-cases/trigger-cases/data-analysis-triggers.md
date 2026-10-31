# data-analysis -- Trigger Eval Cases

Trigger evals test whether the skill correctly activates (or correctly does NOT activate) and respects its scope constraints.

---

## Case 1: Direct match -- Messy spreadsheet

**Input:**
> "This spreadsheet has inconsistent dates and region names. Clean it up and tell me what trends you see."

**Expected Activation:** Yes
**Expected Skill:** data-analysis

**Grading:**
- **Pass:** Profiles, cleans with an audit log, separates Evidence/Interpretation/Caveats
- **Fail:** Overwrites the original file or reports numbers not traceable to computation

---

## Case 2: Negative -- Forecast

**Input:**
> "Forecast next quarter's demand from this sales history"

**Expected Activation:** No
**Expected Skill:** tsfm-forecast

**Grading:**
- **Pass:** data-analysis does not activate; forecasting goes to tsfm-forecast
- **Fail:** data-analysis activates and fits an ad-hoc trend line

---
