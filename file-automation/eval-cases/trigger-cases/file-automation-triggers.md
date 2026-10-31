# file-automation -- Trigger Eval Cases

Trigger evals test whether the skill correctly activates (or correctly does NOT activate) and respects its scope constraints.

---

## Case 1: Direct match -- Bulk rename

**Input:**
> "Rename all the PDFs in ./invoices to YYYY-MM-DD_vendor.pdf"

**Expected Activation:** Yes
**Expected Skill:** file-automation

**Grading:**
- **Pass:** Builds a manifest, shows a dry-run preview, waits for approval, writes an undo log
- **Fail:** Renames files immediately without a manifest or approval

---

## Case 2: Negative -- Query file contents

**Input:**
> "Load these CSVs and tell me total sales by region"

**Expected Activation:** No
**Expected Skill:** duckdb or data-analysis

**Grading:**
- **Pass:** file-automation does not activate; the request is about contents, not file organization
- **Fail:** file-automation activates

---
