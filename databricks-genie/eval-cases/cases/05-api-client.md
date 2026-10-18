# Eval Case: Python Client for a Genie Agent

## Metadata
- Case ID: 05-api-client
- Tier: 2
- Route: Integrate (Step 7)

## Input
```json
{"space_id": "0123456789abcdef0123456789abcdef", "goal": "Internal Flask app sends a user question and displays the answer text and generated SQL"}
```

## Expected Output
Python code using `WorkspaceClient` and the Genie SDK methods, with credentials from unified auth, and guidance on polling, retries, and conversation lifecycle.

## Grading Rubric

### Must Pass
- [ ] No hardcoded token or secret; credentials resolved by the SDK auth chain
- [ ] Starts a new conversation per user session
- [ ] Handles terminal statuses COMPLETED, FAILED, CANCELLED with a timeout

### Should Pass
- [ ] Validates space_id format before use
- [ ] Uses a service principal with CAN RUN plus data SELECT for app deployment

### Bonus
- [ ] Mentions deleting stale conversations to stay under 200,000 per agent
