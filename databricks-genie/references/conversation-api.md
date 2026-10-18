# Genie API, SDK, and Deployment

## Table of Contents

- Prerequisites and Auth
- Chat Mode Endpoints
- Message Status Values
- Client Rules
- Python SDK Client
- Management Endpoints and serialized_space
- Bundle Deployment
- Integration Surfaces

Facts verified against Azure Databricks docs, October 2026. The API still uses `space` naming after the rename to Genie Agents.

---

## Prerequisites and Auth

- Databricks SQL entitlement and CAN USE on a pro or serverless SQL warehouse.
- At least CAN RUN on the agent and `SELECT` on its data for the calling identity.
- Prefer OAuth: user-to-machine for interactive apps, machine-to-machine with a service principal for automation. Service principal usage is billed; user usage of Genie Agents is free through January 31, 2027 per the July 2026 release notes, with SQL warehouse compute billed separately.

## Chat Mode Endpoints

Base path: `/api/2.0/genie/spaces/{space_id}`

| Operation | Method and path |
|-----------|-----------------|
| Start conversation | `POST .../start-conversation` |
| Send follow-up | `POST .../conversations/{conversation_id}/messages` |
| Get message | `GET .../conversations/{conversation_id}/messages/{message_id}` |
| Get query result | `GET .../messages/{message_id}/attachments/{attachment_id}/query-result` |
| Re-run expired query | `POST .../attachments/{attachment_id}/execute-query` |
| List conversations | `GET .../conversations` |
| Delete conversation | `DELETE .../conversations/{conversation_id}` |

Agent mode has separate endpoints; see the Agent mode API reference before building on them. Full request and response schemas live in the REST reference at docs.databricks.com/api/workspace/genie.

## Message Status Values

`SUBMITTED`, `FETCHING_METADATA`, `FILTERING_CONTEXT`, `ASKING_AI`, `PENDING_WAREHOUSE`, `EXECUTING_QUERY`, `COMPLETED`, `FAILED`, `QUERY_RESULT_EXPIRED`, `CANCELLED`

Terminal for polling: `COMPLETED`, `FAILED`, `CANCELLED`. A `COMPLETED` message does not change on re-poll.

## Client Rules

- Poll every 1-5 seconds with exponential backoff capped at one minute; stop after 10 minutes and surface a timeout.
- Implement retries with backoff; the API does not retry for you. No numeric rate limit is published, so handle `RATE_LIMIT_EXCEEDED` errors defensively.
- Start a new conversation per user session; reusing threads reduces accuracy.
- Stay under 200,000 conversations per agent: list and delete stale or test conversations.
- Genie cancels SQL running longer than 5 minutes and messages longer than 30 minutes.
- Log requests and responses for debugging and cost tracking, excluding tokens and result data that is sensitive.

## Python SDK Client

`WorkspaceClient()` resolves credentials through Databricks unified auth (environment variables or a configured profile). Never pass tokens as literals.

```python
import os
from databricks.sdk import WorkspaceClient

SPACE_ID = os.environ["GENIE_SPACE_ID"]  # validate: 32 lowercase hex chars

w = WorkspaceClient()
msg = w.genie.start_conversation_and_wait(
    space_id=SPACE_ID,
    content="What was net revenue by region last fiscal quarter?",
)
for att in msg.attachments or []:
    if att.text:
        print("Answer:", att.text.content)
    if att.query:
        print("SQL:", att.query.query)

follow_up = w.genie.create_message_and_wait(
    space_id=SPACE_ID,
    conversation_id=msg.conversation_id,
    content="Only for the EMEA region",
)
```

The `start_conversation_and_wait` and `create_message_and_wait` calls appear in the Databricks Apps Genie docs. Verify attachment field names (`text`, `query`) against the installed `databricks-sdk` version before relying on them.

CLI equivalents: `databricks genie start-conversation SPACE_ID "question"`, `databricks genie create-message SPACE_ID CONVERSATION_ID "question"`, and `databricks genie ask "question" --include-sql`.

## Management Endpoints and serialized_space

| Operation | Method and path | CLI |
|-----------|-----------------|-----|
| List | `GET /api/2.0/genie/spaces` | `databricks genie list-spaces` |
| Create | `POST /api/2.0/genie/spaces` | `databricks genie create-space WAREHOUSE_ID SERIALIZED_SPACE` |
| Get (export) | `GET /api/2.0/genie/spaces/{space_id}` | `databricks genie get-space SPACE_ID` |
| Update | `PATCH /api/2.0/genie/spaces/{space_id}` | `databricks genie update-space SPACE_ID --json ...` |
| Trash | `DELETE /api/2.0/genie/spaces/{space_id}` | `databricks genie trash-space SPACE_ID` |

`serialized_space` validation rules:
- Table identifiers use `catalog.schema.table`.
- All instruction IDs are unique 32-character lowercase hex; arrays are pre-sorted.
- At most one text instruction per agent; strings up to 25,000 characters; arrays up to 10,000 items.
- Each join spec `sql` holds exactly two elements: the condition and a relationship annotation such as `--rt=FROM_RELATIONSHIP_TYPE_MANY_TO_ONE--`.
- Each benchmark question has exactly one answer with format `SQL`.
- The docs show conflicting `version` values; export an existing agent with `get-space` and copy its `version` rather than hand-writing it.

## Bundle Deployment

Define agents as code with the `genie_spaces` resource in Declarative Automation Bundles. Requires Databricks CLI 1.3.0 or later with the direct deployment engine.

```yaml
resources:
  genie_spaces:
    sales_performance:
      title: Sales Performance
      warehouse_id: ${var.warehouse_id}
      file_path: ./genie/sales_performance.geniespace.json
      permissions:
        - group_name: sales-managers
          level: CAN_RUN
```

Export an existing agent into a bundle with `databricks bundle generate genie-space --existing-id <space_id>`; add `--resource <key> --watch --force` to keep the JSON in sync with UI edits. Run `databricks bundle deployment bind` before deploying, or the deploy creates a new agent instead of updating the existing one. `file_path` takes precedence over `serialized_space`. Verify permission level values against the bundle permissions reference for the installed CLI version.

## Integration Surfaces

| Surface | Status (Oct 2026) | Notes |
|---------|-------------------|-------|
| Genie One (formerly Databricks One) | GA | Business-user UI; consumer access suffices |
| Genie Agent managed MCP server | Requires Managed MCP Servers preview | `https://<workspace-host>/api/2.0/mcp/genie/{space_id}`; read-only; no conversation history |
| Copilot Studio | GA (Oct 7, 2026) | Adds an agent as a tool; requires Managed MCP Servers |
| Microsoft Teams app | Public Preview | Routed through Genie One; warehouse not selectable |
| Microsoft Foundry | Public Preview | MCP tool with OAuth identity passthrough |
| Databricks Apps | GA | Add the agent as an app resource with CAN RUN |

The legacy beta endpoint `/api/2.0/mcp/genie` is deprecated with an October 31, 2026 sunset; migrate to the per-agent MCP URL or the Genie One MCP.

Sources: learn.microsoft.com/azure/databricks/genie-agents/conversation-api, /genie-agents/troubleshooting, /dev-tools/cli/reference/genie-commands, /dev-tools/databricks-apps/genie, /dev-tools/bundles/resources, /agents/mcp-tools/genie-agent, /ai-bi/release-notes/2026
