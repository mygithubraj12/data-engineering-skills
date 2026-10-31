---
name: file-automation
description: "Use this skill when organizing messy folders, bulk-renaming files, sorting documents into a folder structure, or applying the same operation to many files consistently. Plans every change as a reviewable manifest, runs a dry run first, and keeps an undo log so batch operations can be reversed. Common phrases: \"rename all these files\", \"organize my downloads folder\", \"sort these PDFs by date\", \"clean up this directory\". Do NOT use for loading file contents into a database for analysis (use duckdb), file-based ingestion pipelines (use dlt-extract), or designing a multi-step recurring workflow (use automation-master)."
model:
  preferred: sonnet
  acceptable: [haiku, sonnet, opus]
  minimum: haiku
  allow_downgrade: true
  reasoning_demand: low
version: 1.0.0
---

# File Automation

Safe, reversible bulk file operations: rename, move, sort, deduplicate, and batch-process.

## When to Use This Skill

**Activate when:** Renaming many files to a convention, sorting files into folders by type/date/pattern, finding duplicates, or applying one transformation to a batch of files.

**Don't use for:** Querying file contents with SQL (use `duckdb`), building ingestion pipelines from files (use `dlt-extract`), designing broader recurring workflows (use `automation-master`).

## Scope Constraints

- Operates only inside the directory the user names (the "target root"). Never touches paths outside it.
- Never operates on the home directory root, system directories, version-control metadata (`.git/`), or hidden dotfiles unless the user names them explicitly and confirms.
- Never deletes files. "Delete" requests become a move into a holding folder named _to_delete inside the target root for the user to remove.
- Never overwrites an existing file; collisions get a numeric suffix (`name_1.ext`).
- Does not open or modify file contents unless the operation is explicitly content-based (e.g., convert encoding).

## Input Sanitization

- Resolve the target root to an absolute path and confirm it with the user before any write.
- Reject any computed destination whose resolved path is outside the target root (blocks `..` and symlink escapes).
- Strip or replace characters invalid on Windows/macOS/Linux (`<>:"/\|?*`, control chars) from generated filenames.
- Pass paths as quoted arguments or via Python `pathlib`; never build shell strings from filenames.

## Model Routing

| reasoning_demand | preferred | acceptable | minimum |
|-----------------|-----------|------------|---------|
| low | Sonnet | Haiku, Sonnet, Opus | Haiku |

## Procedure

Progress checklist: `[ ] Inventory  [ ] Rules  [ ] Manifest  [ ] Review  [ ] Execute  [ ] Verify`

Step 1: Inventory the target root.
  - List files recursively with size, extension, and modified time. Report the count.
  - If >10,000 files → confirm scope with the user before proceeding.

Step 2: Define rules.
  - Restate the naming/sorting rule as a pattern (e.g., `{YYYY-MM-DD}_{vendor}_{doc-type}.pdf`).
  - If the rule needs data not in the filename or metadata → ask; do not guess from content.

Step 3: Build the manifest.
  - Write `_file_ops_manifest.csv` in the target root with columns `source,destination,action,reason`.
  - Flag collisions, unmatched files, and files the rule cannot parse as `action=skip`.

Step 4: Review.
  - Show the first 20 rows plus counts per action. Proceed only after explicit user approval.

Step 5: Execute.
  - Apply each manifest row; append the inverse operation to `_file_ops_undo.csv` before each move (state checkpoint).
  - On any error → stop, report the row, leave the undo log intact.

Step 6: Verify.
  - Confirm file count before = after (plus skips). Confirm every destination exists.
  - Tell the user how to undo: replay `_file_ops_undo.csv`.

## Output Format

```text
Target: ./invoices (412 files)
Rule:   {invoice_date:YYYY-MM-DD}_{vendor}.pdf
Plan:   rename 398 | skip 14 (unparseable date) | collisions 0
Manifest: ./invoices/_file_ops_manifest.csv — approve to execute.
```

## Handoffs

- **Query or profile file contents** → `duckdb`
- **Recurring ingestion from these files** → `dlt-extract`
- **Larger recurring workflow** → `automation-master`
