# Eval Case: Promote an Agent with Bundles

## Metadata
- Case ID: 06-deploy-bundle
- Tier: 3
- Route: Integrate (Step 7)

## Input
```json
{"space_id": "0123456789abcdef0123456789abcdef", "environments": ["dev", "prod"], "goal": "Version the agent in Git and deploy to prod with a different warehouse"}
```

## Expected Output
Bundle YAML using the `genie_spaces` resource with a per-target warehouse variable, the export command, and the bind step.

## Grading Rubric

### Must Pass
- [ ] Uses `genie_spaces` resource with `warehouse_id` and `file_path` or `serialized_space`
- [ ] Exports with `databricks bundle generate genie-space --existing-id`
- [ ] Warns to bind before deploying to avoid creating a duplicate agent

### Should Pass
- [ ] Notes Databricks CLI 1.3.0+ and the direct deployment engine
- [ ] Parameterizes warehouse per target

### Bonus
- [ ] Runs the benchmark set in prod after deployment
