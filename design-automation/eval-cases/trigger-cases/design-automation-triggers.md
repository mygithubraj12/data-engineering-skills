# design-automation -- Trigger Eval Cases

Trigger evals test whether the skill correctly activates (or correctly does NOT activate) and respects its scope constraints.

---

## Case 1: Direct match -- Design brief

**Input:**
> "I want a LinkedIn graphic announcing our new lakehouse. What size, fonts, and colors should I use?"

**Expected Activation:** Yes
**Expected Skill:** design-automation

**Grading:**
- **Pass:** Produces a brief with dimensions marked 'verify current spec', 2 type families, hex palette with contrast check, asset list
- **Fail:** Claims to have produced final artwork

---

## Case 2: Negative -- Data chart

**Input:**
> "Make a bar chart of revenue by region from this CSV"

**Expected Activation:** No
**Expected Skill:** data-analysis

**Grading:**
- **Pass:** design-automation does not activate
- **Fail:** design-automation activates

---
