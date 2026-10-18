# SkillOpt Task Set for databricks-genie

`tasks.json` is a hand-authored task file in the SkillOpt-Sleep `skillopt_sleep.tasks.v1` format, for optimizing `databricks-genie/SKILL.md` with [microsoft/SkillOpt](https://github.com/microsoft/SkillOpt). It is never loaded during skill execution.

## Contents

- 24 tasks: 14 `train`, 5 `val` (gates edits), 5 `test` (final held-out score)
- Every task uses a rule judge with outcome checks (`regex`, `contains`, `not_contains`, `no_refusal`) and a plain-language `description` per check. SkillOpt shows the optimizer only descriptions, never the regex.
- `"reviewed": false` on purpose. SkillOpt refuses real-backend runs until a human reviews the file and sets it to `true`.

Validated against SkillOpt `main` at commit `ae2f766` (2026-10-17): the file loads through `skillopt_sleep.tasks_file.load_tasks_file`, all checks pass `validate_checks`, and author-written reference answers score 1.0 on every task while a generic answer scores 0.

## Known Limitations

- **Regex brittleness.** The reference answers were written by the same author as the checks. A correct answer with different wording can fail a check. Review failures before trusting a low score, and loosen checks that reject correct answers.
- **SKILL.md only.** SkillOpt-Sleep supplies the target skill as prompt text; reference files are not loaded. Tasks that depend on reference-only facts may push the optimizer to copy reference content into SKILL.md, inflating its token count.
- **Doc drift.** Expected facts reflect Databricks docs as of October 2026. Re-verify limits, statuses, and dates before each optimization run.
- **Rule checks are not a substitute for expert review.** Run the output cases in `../cases/` by hand for nuanced quality.

## Run Procedure

Progress checklist: [ ] review [ ] mock run [ ] real run [ ] governance gate [ ] human adopt

1. Review every task and check. Edit wording, then set `"reviewed": true`.
2. Install SkillOpt from source (`--tasks-file` with fan-out and `--preferences` needs `main`, not PyPI 0.2.0). Verify flags with `python -m skillopt_sleep --help`.
3. Dry-run on the free mock backend to validate plumbing:

```bash
python -m skillopt_sleep dry-run --project "$(pwd)" --backend mock \
  --tasks-file databricks-genie/eval-cases/skillopt/tasks.json \
  --target-skill-path databricks-genie/SKILL.md
```

4. Run once on a real backend with a small edit budget and the repo's writing rules as preferences:

```bash
python -m skillopt_sleep run --project "$(pwd)" --backend claude \
  --tasks-file databricks-genie/eval-cases/skillopt/tasks.json \
  --target-skill-path databricks-genie/SKILL.md \
  --edit-budget 3 \
  --preferences "Third person. Imperative procedure steps. No explanatory prose. Keep SKILL.md under 2,000 tokens. Do not remove Scope Constraints or Input Sanitization." \
  --progress
```

   Set `"gate_no_regression": true` in the SkillOpt config file so no validation task may get worse.
5. Inspect the staged proposal. Reject it unless `pre-commit run --all-files` passes and the diff reads correctly.
6. Adopt manually. Never use `--auto-adopt` or `schedule` on this repository.

Task-limit flags bound work but are not spend limits. A real backend sends task text and the skill to the model provider; this file contains no customer data.
