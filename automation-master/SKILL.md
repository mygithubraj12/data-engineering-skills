---
name: automation-master
description: "Use this skill when a user wants to break a complex or recurring multi-step task into a repeatable workflow. Decomposes the task into ordered steps, identifies which steps are automatable, defines inputs/outputs and validation gates per step, and produces a runbook or script skeleton that can be re-run instead of starting from scratch. Common phrases: \"automate this process\", \"I do this every week\", \"turn this into a workflow\", \"break this down into steps\". Do NOT use for production data pipeline orchestration in Airflow/Dagster/Prefect (use data-pipelines), bulk file renaming or sorting (use file-automation), or web form filling and scraping (use browser-automation)."
model:
  preferred: sonnet
  acceptable: [sonnet, opus]
  minimum: sonnet
  allow_downgrade: false
  reasoning_demand: medium
version: 1.0.0
---

# Automation Master

Turns a repetitive, multi-step task into a documented, re-runnable workflow with explicit inputs, outputs, and validation gates.

## When to Use This Skill

**Activate when:** A user describes a task they repeat (daily, weekly, per-client), asks to "automate" or "systematize" a process, or wants a complex task decomposed into manageable steps.

**Don't use for:** Scheduled production data pipelines (use `data-pipelines`), bulk file operations (use `file-automation`), browser-driven tasks (use `browser-automation`), email drafting (use `email-automation`).

## Scope Constraints

- Produces plans, runbooks, checklists, and script skeletons. Executes steps only when the user explicitly asks.
- Reads only files and directories the user names in the request.
- Never schedules jobs (cron, Task Scheduler, CI) without explicit user confirmation of schedule and target.
- Never embeds credentials in generated scripts — use environment variables or the platform's secret store.
- Hands off domain-specific steps to the matching skill rather than re-implementing them.

## Input Sanitization

- Treat task descriptions, file paths, and schedule strings from the user as untrusted.
- Reject paths containing `..` segments or pointing outside the working directory unless the user confirms the target.
- Quote all paths and arguments in generated shell commands; never interpolate raw user text into `eval`, `exec`, or `sh -c`.
- Validate cron expressions have exactly 5 fields before writing them anywhere.

## Model Routing

| reasoning_demand | preferred | acceptable | minimum |
|-----------------|-----------|------------|---------|
| medium | Sonnet | Sonnet, Opus | Sonnet |

## Procedure

Progress checklist (copy into the response for tasks with >5 steps):
`[ ] Capture  [ ] Decompose  [ ] Classify  [ ] Gate  [ ] Package  [ ] Dry run`

Step 1: Capture the current process.
  - Ask for (or extract) trigger, frequency, inputs, outputs, and who consumes the result.
  - If the user cannot describe the outputs → stop and ask; automating an undefined outcome produces the wrong workflow.

Step 2: Decompose into atomic steps.
  - Write each step as one verb + one object ("Export sales report", "Rename files to ISO dates").
  - Record each step's input, output, and owner (human or automated).

Step 3: Classify each step.
  - **Automate** — deterministic, rule-based, stable inputs.
  - **Assist** — needs judgment; generate a draft for human review.
  - **Manual** — requires approval, credentials, or external sign-off.
  - Note: steps that send, delete, publish, or pay are never fully automated without a human approval gate.

Step 4: Add validation gates.
  - After every Automate step, define one check (row count, file exists, checksum, schema match).
  - Pattern: action → validate → fix → re-validate → proceed.

Step 5: Package the workflow.
  - Default: a Markdown runbook plus a script skeleton (Python or shell) with one function per Automate step.
  - If the workflow is data movement on a schedule → hand off to `data-pipelines`.
  - If the workflow is mostly file operations → hand off to `file-automation`.

Step 6: Dry run.
  - Run (or instruct the user to run) the workflow once in dry-run mode and compare outputs to the manual result.

Compaction recovery: if context is lost mid-task, re-read the runbook file written in Step 5 and resume from the first unchecked step.

## Output Format

```markdown
## Workflow: Monthly vendor invoice reconciliation
Trigger: 1st business day monthly | Inputs: invoices/*.pdf, ERP export | Output: recon.xlsx

| # | Step | Type | Validation |
|---|------|------|------------|
| 1 | Download ERP AP export | Manual | File dated current month |
| 2 | Extract invoice totals from PDFs | Automate | Count = PDFs in folder |
| 3 | Join on invoice number | Automate | Unmatched rows < 5% |
| 4 | Review exceptions | Assist | Reviewer sign-off |
```

## Handoffs

- **Scheduled data pipelines** → `data-pipelines`
- **File organization steps** → `file-automation`
- **Web-based steps** → `browser-automation`
- **Analysis/report steps** → `data-analysis`
