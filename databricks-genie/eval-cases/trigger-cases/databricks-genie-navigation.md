# databricks-genie -- Navigation Eval Cases

Navigation evals test which reference files get loaded during a conversation. Correct navigation means the right file is loaded at the right time -- and no unnecessary files are pre-loaded. Distinct from trigger evals (activation) and output evals (result quality).

---

## Case 1: SKILL.md Sufficient -- Scoping a New Agent

**Category:** SKILL.md-sufficient
**Tier:** 1 (simple)

**Input:**
> "What tables should I include in a new Genie agent for finance?"

**Expected Skill Activated:** databricks-genie
**Expected Files Loaded:** SKILL.md only

**Grading:**
- **Pass:** Answers from Step 2 (purpose, audience, five or fewer tables, 50-object limit) without loading references
- **Fail:** Loads all three references up front

---

## Case 2: Curation Reference -- Writing Instructions

**Category:** Single reference
**Tier:** 2 (medium)

**Input:**
> "Write the general instructions for our Genie agent, including when it should ask clarifying questions"

**Expected Files Loaded:** SKILL.md, references/space-curation.md

**Grading:**
- **Pass:** Loads space-curation.md for the four-part clarification structure and the exact summary heading
- **Fail:** Loads conversation-api.md or benchmark-evaluation.md

---

## Case 3: Benchmark Reference -- Triage Bad Results

**Category:** Single reference
**Tier:** 2 (medium)

**Input:**
> "Half my Genie benchmarks are rated Bad because of extra columns and wrong joins. What do I change?"

**Expected Files Loaded:** SKILL.md, references/benchmark-evaluation.md

**Grading:**
- **Pass:** Uses the root-cause triage table (projection via example SQL; joins via UC keys or join relationships)
- **Partial:** Loads benchmark-evaluation.md and space-curation.md together without needing the second
- **Fail:** Answers without loading benchmark-evaluation.md and misstates the rating rules

---

## Case 4: API Reference -- CI/CD Deployment

**Category:** Single reference
**Tier:** 2 (medium)

**Input:**
> "How do I promote a Genie agent from dev to prod with version control?"

**Expected Files Loaded:** SKILL.md, references/conversation-api.md

**Grading:**
- **Pass:** Recommends the `genie_spaces` bundle resource, `bundle generate genie-space`, and binding before deploy
- **Fail:** Proposes manual UI recreation in each workspace

---

## Case 5: Sequential Loading -- Full Lifecycle

**Category:** Multi-step
**Tier:** 3 (complex)

**Input:**
> "Design a Genie agent for supply chain, build benchmarks, then expose it to our Teams users"

**Expected Files Loaded:** SKILL.md, then space-curation.md (Step 4), then benchmark-evaluation.md (Step 5), then conversation-api.md (Step 7)

**Grading:**
- **Pass:** Loads each reference only when its step begins; notes the Teams app is Public Preview
- **Partial:** Correct content but references pre-loaded together
- **Fail:** Skips benchmarks before integration
