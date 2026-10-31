# Data Engineering Skills for Claude Code

[![Claude Skill](https://img.shields.io/badge/Claude-Skill-8A6BFF)](https://github.com/dtsong/data-engineering-skills)
[![License](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](LICENSE)
[![Skills](https://img.shields.io/badge/Skills-14-green.svg)](CATALOG.md)

> Expert-level Claude Code skills for data engineering: dbt, Fivetran, Kafka, Airflow, Snowflake, and more.

This repository contains a curated suite of skills that enable Claude Code to provide expert guidance for data engineering workflows. Whether you're building data pipelines, modeling in dbt, integrating SaaS tools, or streaming events with Kafka, these skills help Claude understand your context and provide detailed, actionable guidance.

**What problem does this solve?** Data engineering involves many specialized tools (dbt, Fivetran, Kafka, Airflow, Snowflake, BigQuery, etc.) with deep best practices. These skills give Claude the domain expertise to help you make decisions, write code, debug issues, and architect solutions across the modern data stack.

---

## What Are You Trying to Do?

Find your use case below and install the corresponding skill:

| I want to... | Use this skill | Status |
|--------------|---------------|--------|
| Connect **Salesforce, NetSuite, Stripe, or HubSpot** to my data warehouse | [data-integration](data-integration/) | Available |
| Set up **Fivetran or Airbyte** connectors | [data-integration](data-integration/) | Available |
| Build **real-time streaming pipelines** with Kafka or Flink | [event-streaming](event-streaming/) | Available |
| Stream data into **Snowflake, BigQuery, or Databricks** | [event-streaming](event-streaming/) | Available |
| Write **dbt models, tests, and documentation** | [dbt-transforms](dbt-transforms/) | Available |
| Set up **dbt CI/CD** with slim CI and artifacts | [dbt-transforms](dbt-transforms/) | Available |
| Optimize **dbt performance** (incremental models, materializations) | [dbt-transforms](dbt-transforms/) | Available |
| Write **Python for data engineering** (dbt-py, PySpark, Pandas, API scripts) | [python-data-engineering](python-data-engineering/) | Available |
| Design **Airflow or Dagster** DAGs | [data-pipelines](data-pipelines/) | Available |
| Schedule and monitor **data pipelines** | [data-pipelines](data-pipelines/) | Available |
| Use **AI/LLMs in data workflows** (embeddings, semantic search, MCP) | [ai-data-integration](ai-data-integration/) | Available |
| **Analyze local CSV/Excel/Parquet** files with DuckDB | [duckdb](duckdb/) | Available |
| Run a **data cleaning engagement** for a client | [client-delivery](client-delivery/) | Available |
| Build **portable DLT pipelines** from file sources | [dlt-extract](dlt-extract/) | Available |
| Generate **zero-shot time-series forecasts** with foundation models | [tsfm-forecast](tsfm-forecast/) | Available |
| Design a **data testing strategy** with SQL assertions and test reports | [data-testing](data-testing/) | Available |
| Implement **data governance** — cataloging, lineage, classification, access control | [data-governance](data-governance/) | Available |
| Set up **data observability** — freshness monitoring, alerting, incident response | [data-observability](data-observability/) | Available |
| Work with **Azure Data Factory, Synapse, Fabric, or SQL Server** | [microsoft-data-stack](microsoft-data-stack/) | Available |
| Migrate **SSIS packages** to modern ETL (ADF, dbt, dlt) | [microsoft-data-stack](microsoft-data-stack/) | Available |
| **Automate a recurring multi-step task** as a re-runnable workflow | [automation-master](automation-master/) | Available |
| **Bulk rename or organize files** safely with dry run and undo | [file-automation](file-automation/) | Available |
| **Extract data from a web page** or automate a web form | [browser-automation](browser-automation/) | Available |
| **Clean a messy spreadsheet** and report trends | [data-analysis](data-analysis/) | Available |
| Turn an idea into a **design brief** | [design-automation](design-automation/) | Available |
| **Research a topic with verified sources** | [research-agent](research-agent/) | Available |
| **Summarize email threads** and draft replies | [email-automation](email-automation/) | Available |
| Choose between **Snowflake, BigQuery, Databricks, DuckDB, Synapse, or SQL Server** | [shared-references/warehouse-comparison](shared-references/data-engineering/warehouse-comparison.md) | Available |
| Implement **data quality checks** (freshness, completeness, accuracy) | [shared-references/data-quality-patterns](shared-references/data-engineering/data-quality-patterns.md) | Available |

**Don't see your use case?** Check the full [Catalog](CATALOG.md) or open an [issue](https://github.com/dtsong/data-engineering-skills/issues) to request a new skill.

---

## Quick Install

### Option 1: Install All Skills (Recommended for Platform Engineers)

```bash
git clone https://github.com/dtsong/data-engineering-skills
cd data-engineering-skills
./install.sh
```

This installs all available skills to `~/.claude/skills/data-engineering-skills/`.

### Option 2: Install by Role

```bash
./install.sh --role analytics-engineer
./install.sh --role data-platform-engineer
./install.sh --role integration-engineer
./install.sh --role ml-engineer
```

See [Role-Based Presets](#role-based-presets) below for what each role includes.

### Option 3: Install Specific Skills

```bash
./install.sh --skills dbt-transforms,event-streaming
./install.sh --skills data-integration
```

### Option 4: Manual Install

```bash
git clone https://github.com/dtsong/data-engineering-skills ~/.claude/skills/data-engineering-skills
```

### Windows Support

Skills work on **Windows** via WSL (Windows Subsystem for Linux) or Git Bash:

- **WSL (recommended):** Run `./install.sh` from your WSL terminal. Skills install to `~/.claude/skills/` inside WSL.
- **Git Bash:** Run `./install.sh` from Git Bash. Paths map to your Windows home directory.
- **Skills are platform-agnostic:** The skill files themselves are plain markdown and work on any OS. Only the install script requires a bash-compatible shell.

Native Windows CMD or PowerShell users should use WSL or Git Bash for the install script.

### Update Existing Installation

```bash
cd ~/.claude/skills/data-engineering-skills
git pull
```

Or use the installer:

```bash
./install.sh --update
```

---

## Role-Based Presets

Not sure which skills to install? We've created presets for common roles:

| Role | Skills Installed | Description |
|------|-----------------|-------------|
| **analytics-engineer** | dbt-transforms, python-data-engineering | Transform and model data using SQL and Python |
| **data-platform-engineer** | All skills | Full toolkit for building and maintaining data platforms |
| **integration-engineer** | data-integration, event-streaming, data-pipelines | Connect systems, orchestrate pipelines, handle real-time data |
| **ml-engineer** | python-data-engineering, ai-data-integration, tsfm-forecast | Python-first workflows, AI/ML pipelines, time-series forecasting |
| **data-consultant** | dbt-transforms, duckdb, client-delivery, dlt-extract, data-pipelines, data-testing | End-to-end data cleaning engagements |
| **microsoft-data-engineer** | microsoft-data-stack, data-pipelines, data-governance, dbt-transforms, data-observability | Azure/SQL Server data platform engineering |
| **productivity** | automation-master, file-automation, browser-automation, data-analysis, design-automation, research-agent, email-automation | General automation, research, and communication tasks |

**Example**:
```bash
./install.sh --role analytics-engineer
```

This installs:
- dbt-transforms (modeling, testing, CI/CD, performance)
- python-data-engineering (dbt-py, Pandas, PySpark, API scripts)
- Shared references (data-quality-patterns, warehouse-comparison)

---

## How Skills Work

Skills are **prompt templates** that give Claude deep domain knowledge. Here's how they work:

1. **Auto-activation**: When you mention keywords like "dbt", "Fivetran", "Kafka", or "Airflow", Claude automatically loads the relevant skill.
2. **Progressive disclosure**: Skills provide core guidance first, then offer references for deep dives (e.g., "See dbt-testing-guide.md for 30+ test examples").
3. **No manual activation needed**: You don't need to explicitly invoke skills—just start asking questions.
4. **Context-aware**: Skills know when to activate based on your conversation, file context, and project structure.

**Example conversation**:

> **You**: "Help me write a dbt staging model for Stripe charges"
>
> **Claude** (dbt-transforms auto-activates): "I'll help you create a staging model following dbt best practices. Here's a model that handles Stripe's nested JSON structure and adds data quality tests..."

---

## Suite Overview

### Available Skills

| Skill | Description | Lines | Files |
|-------|-------------|-------|-------|
| [dbt-transforms](dbt-transforms/) | dbt modeling, testing, incremental strategies, CI/CD, performance, governance | 3,000 | 7 |
| [data-integration](data-integration/) | Fivetran, Airbyte, API extraction, CDC, Reverse ETL, enterprise connectors | 3,650 | 7 |
| [event-streaming](event-streaming/) | Kafka, Flink, Spark Streaming, warehouse streaming, event architectures | 2,500 | 6 |

| [data-pipelines](data-pipelines/) | Dagster, Airflow, Prefect, scheduling, monitoring, consulting orchestration | 2,500 | 6 |
| [python-data-engineering](python-data-engineering/) | Polars, Pandas, PySpark, dbt Python models, API extraction, data validation | 2,700 | 6 |
| [ai-data-integration](ai-data-integration/) | MCP servers, NL-to-SQL, embeddings, LLM transforms | 1,750 | 5 |
| [duckdb](duckdb/) | DuckDB local analysis, CSV/Excel/Parquet/JSON ingestion, profiling, export | 690 | 7 |
| [client-delivery](client-delivery/) | Engagement lifecycle, schema profiling, deliverables, client handoff | 760 | 7 |
| [dlt-extract](dlt-extract/) | File-based DLT pipelines, destination swapping, schema contracts | 750 | 7 |
| [tsfm-forecast](tsfm-forecast/) | Zero-shot time-series forecasting with TimesFM, Chronos, MOIRAI, Lag-Llama | 730 | 10 |
| [data-testing](data-testing/) | Testing strategy, SQL assertions, pipeline validation, test-as-deliverable | 850 | 7 |
| [data-governance](data-governance/) | Data cataloging, lineage, classification, access control, compliance | 1,110 | 8 |
| [data-observability](data-observability/) | Freshness monitoring, volume anomaly detection, alerting, incident response | 1,160 | 8 |
| [microsoft-data-stack](microsoft-data-stack/) | ADF orchestration, Synapse/Fabric lakehouse, SQL Server CDC, SSIS migration, dbt-sqlserver | 1,200 | 4 |
| [automation-master](automation-master/) | Recurring task decomposition into re-runnable workflows | ~90 | 2 |
| [file-automation](file-automation/) | Bulk rename/sort/organize with manifest, dry run, undo | ~90 | 2 |
| [browser-automation](browser-automation/) | Web navigation, form filling, data extraction (browser tool or Playwright) | ~95 | 2 |
| [data-analysis](data-analysis/) | Spreadsheet cleaning, trend analysis, findings reports | ~90 | 2 |
| [design-automation](design-automation/) | Design briefs: dimensions, typography, color, layout, assets | ~85 | 2 |
| [research-agent](research-agent/) | Sourced research with Verified/Recalled/Unresolved labels | ~85 | 2 |
| [email-automation](email-automation/) | Thread summaries, reply drafts, templates with send approval | ~85 | 2 |

### Shared References

| Reference | Description | Lines |
|-----------|-------------|-------|
| [data-quality-patterns](shared-references/data-engineering/data-quality-patterns.md) | Tool-agnostic quality frameworks (four pillars, anomaly detection, alerting) | 300 |
| [warehouse-comparison](shared-references/data-engineering/warehouse-comparison.md) | Snowflake vs BigQuery vs Databricks vs DuckDB decision matrix | 300 |
| [security-compliance-patterns](shared-references/data-engineering/security-compliance-patterns.md) | Three-tier security framework, credential management, data classification | 300 |
| [security-tier-model](shared-references/data-engineering/security-tier-model.md) | Consulting security tiers, ENGAGEMENT.yaml schema, tier transitions | 300 |
| [dlt-vs-managed-connectors](shared-references/data-engineering/dlt-vs-managed-connectors.md) | DLT vs Fivetran vs Airbyte decision matrix | 300 |

See full details in [CATALOG.md](CATALOG.md).

---

## Examples

### Example 1: Setting up Fivetran for Salesforce

```bash
./install.sh --skills data-integration
```

Then ask Claude:

> "How do I set up Fivetran to sync Salesforce to Snowflake with incremental updates?"

**Skill activates**: data-integration

**Claude provides**:
1. Fivetran connector setup steps
2. Schema mapping guidance
3. Incremental sync configuration
4. Data quality checks for Salesforce data
5. Common gotchas (API limits, field changes)

### Example 2: Writing dbt Models with Tests

```bash
./install.sh --skills dbt-transforms
```

Then ask Claude:

> "Help me write a dbt mart model that calculates customer lifetime value with data quality tests"

**Skill activates**: dbt-transforms

**Claude provides**:
1. Mart model structure following best practices
2. LTV calculation logic
3. Data quality tests (uniqueness, not-null, ranges)
4. Performance optimization (incremental strategy if needed)
5. Documentation template

### Example 3: Building a Kafka Pipeline

```bash
./install.sh --skills event-streaming
```

Then ask Claude:

> "How do I stream orders from PostgreSQL to BigQuery using Kafka Connect?"

**Skill activates**: event-streaming

**Claude provides**:
1. Kafka Connect source connector config (Debezium for PostgreSQL CDC)
2. Kafka Connect sink connector config (BigQuery)
3. Schema evolution handling
4. Monitoring and alerting setup
5. Error handling and dead letter queue configuration

---

## Contributing

We welcome contributions! Here's how you can help:

### Report Issues or Request Features

Open an issue at [github.com/dtsong/data-engineering-skills/issues](https://github.com/dtsong/data-engineering-skills/issues)

**Examples**:
- "Add Prefect guidance to data-pipelines"
- "Include Polars examples in python-data-engineering"
- "Bug: dbt incremental strategy example has incorrect syntax"

### Submit Pull Requests

See [CONTRIBUTING.md](CONTRIBUTING.md) for guidelines on:
- Adding new reference files
- Improving existing skills
- Creating new skills
- Updating documentation

**PR requirements**:
- Link to a GitHub issue (required)
- Clear description of changes
- Examples/tests if applicable
- Follow existing structure and style

### Share Feedback

Tell us how these skills are working for you! Open a discussion at [github.com/dtsong/data-engineering-skills/discussions](https://github.com/dtsong/data-engineering-skills/discussions)

---

## Roadmap

### Phase 1: Core Skills (Complete)
- dbt-transforms
- data-integration
- event-streaming
- Shared references (data-quality-patterns, warehouse-comparison)

### Phase 2: Orchestration & Python (Complete)
- data-pipelines (Airflow, Dagster, Prefect)
- python-data-engineering (dbt-py, Pandas, PySpark, API scripts)

### Phase 3: AI Data Integration (Complete)
- ai-data-integration (AI agents, MCP, embeddings)

### Phase 4: Data Consulting Extension (Complete)
- duckdb (local data analysis, file ingestion, profiling)
- client-delivery (engagement lifecycle, deliverables, client handoff)
- dlt-extract (file-based DLT pipelines, destination swapping)
- Consulting security tier model, scripts, and templates

### Phase 5: Data Quality, Governance & Observability (Complete)
- data-testing (testing strategy, SQL assertions, pipeline validation, test-as-deliverable)
- data-governance (cataloging, lineage, classification, access control, compliance)
- data-observability (freshness monitoring, volume anomaly detection, alerting, incident response)

---

## License

Apache License 2.0

Copyright 2026 Daniel Song

Licensed under the Apache License, Version 2.0 (the "License"); you may not use this file except in compliance with the License. You may obtain a copy of the License at:

http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software distributed under the License is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied. See the License for the specific language governing permissions and limitations under the License.

See [LICENSE](LICENSE) for full text.

---

## Related Resources

- **Claude Code**: [github.com/anthropics/claude-code](https://github.com/anthropics/claude-code)
- **dbt**: [docs.getdbt.com](https://docs.getdbt.com/)
- **Fivetran**: [fivetran.com/docs](https://fivetran.com/docs)
- **Airbyte**: [docs.airbyte.com](https://docs.airbyte.com/)
- **Apache Kafka**: [kafka.apache.org/documentation](https://kafka.apache.org/documentation/)
- **Apache Airflow**: [airflow.apache.org/docs](https://airflow.apache.org/docs/)
- **Dagster**: [docs.dagster.io](https://docs.dagster.io/)
- **Snowflake**: [docs.snowflake.com](https://docs.snowflake.com/)
- **BigQuery**: [cloud.google.com/bigquery/docs](https://cloud.google.com/bigquery/docs)
- **Databricks**: [docs.databricks.com](https://docs.databricks.com/)
- **Azure Data Factory**: [learn.microsoft.com/azure/data-factory](https://learn.microsoft.com/azure/data-factory/)
- **Azure Synapse**: [learn.microsoft.com/azure/synapse-analytics](https://learn.microsoft.com/azure/synapse-analytics/)
- **Microsoft Fabric**: [learn.microsoft.com/fabric](https://learn.microsoft.com/fabric/)

---

## Questions?

- **Issues/Features**: [github.com/dtsong/data-engineering-skills/issues](https://github.com/dtsong/data-engineering-skills/issues)
- **Discussions**: [github.com/dtsong/data-engineering-skills/discussions](https://github.com/dtsong/data-engineering-skills/discussions)
- **Email**: Available in GitHub profile

---

**Happy data engineering!** 🚀
