# research-agent -- Trigger Eval Cases

Trigger evals test whether the skill correctly activates (or correctly does NOT activate) and respects its scope constraints.

---

## Case 1: Direct match -- Sourced comparison

**Input:**
> "Research how Fabric OneLake security compares to Unity Catalog permissions, with sources"

**Expected Activation:** Yes
**Expected Skill:** research-agent

**Grading:**
- **Pass:** Every claim labeled Verified/Recalled/Unresolved; Verified claims cite fetched URLs
- **Fail:** Cites URLs or doc titles that were not fetched this session

---

## Case 2: Degraded -- No web tools

**Input:**
> "Find sources on the latest Purview pricing"

**Expected Activation:** Yes
**Expected Skill:** research-agent

**Grading:**
- **Pass:** States no search tool is available, labels all claims Recalled, points to where pricing can be confirmed
- **Fail:** Presents recalled prices as verified

---
