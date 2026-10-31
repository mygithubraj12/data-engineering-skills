# Data Engineering Skills Catalog

> A curated library of Claude Code skills for data engineering. Each skill provides expert-level guidance for a specific domain.

---

## How to Use This Catalog

1. **Find the skill** that matches your problem (see table below)
2. **Install** the full suite or individual skills using [install.sh](install.sh)
3. **Skills auto-activate** when Claude detects relevant keywords in your conversation
4. **Progressive disclosure**: Skills provide core guidance first, then reference deep-dive materials on request
5. **Security-first**: All skills follow the [Security & Compliance Patterns](shared-references/data-engineering/security-compliance-patterns.md) framework with three security tiers

---

## Skills by Category

### Data Transformation

| Skill | Type | Description | Status |
|-------|------|-------------|--------|
| [dbt-transforms](dbt-transforms/) | Standalone | dbt modeling, testing, incremental strategies, CI/CD, performance optimization, governance | **Available** |

**Use when**: Writing SQL transformations, building data models, implementing data quality tests, setting up dbt projects.

### Data Integration

| Skill | Type | Description | Status |
|-------|------|-------------|--------|
| [data-integration](data-integration/) | Standalone | iPaaS platforms (Fivetran, Airbyte), DLT (dlthub), API extraction, CDC, webhooks, Reverse ETL, enterprise connectors (Salesforce, NetSuite, Stripe) | **Available** |
| [event-streaming](event-streaming/) | Standalone | Kafka, Flink, Spark Streaming, warehouse streaming (Snowpipe, BigQuery streaming), event architectures | **Available** |

**Use when**: Connecting SaaS tools to warehouses, building data ingestion pipelines with DLT or iPaaS, implementing CDC, handling event streams.

### Data Orchestration

| Skill | Type | Description | Status |
|-------|------|-------------|--------|
| [data-pipelines](data-pipelines/) | Standalone | Dagster (assets, resources, sensors), Airflow (DAGs, operators, TaskFlow), Prefect, scheduling, monitoring, dagster-dbt and dagster-dlt integrations | **Available** |

**Use when**: Scheduling pipelines, designing asset-based workflows (Dagster) or DAGs (Airflow), orchestrating dbt + DLT + streaming, implementing retries and alerts.

### Data Engineering with Python

| Skill | Type | Description | Status |
|-------|------|-------------|--------|
| [python-data-engineering](python-data-engineering/) | Standalone | Polars/Pandas/PySpark DataFrames, dbt Python models, API extraction (httpx, async), data validation (Pydantic, Pandera, Great Expectations) | **Available** |

**Use when**: Writing Python transformations, building custom extractors, processing data with Pandas/Polars/PySpark.

### AI & Data

| Skill | Type | Description | Status |
|-------|------|-------------|--------|
| [ai-data-integration](ai-data-integration/) | Standalone | MCP server patterns for warehouses, NL-to-SQL, embeddings pipelines, LLM-powered transformations, progressive AI integration with security tiers | **Available** |
| [tsfm-forecast](tsfm-forecast/) | Standalone | Zero-shot time-series forecasting pipelines using TimesFM, Chronos, MOIRAI, and Lag-Llama; DuckDB-based data preparation, backtesting harnesses, multi-model comparison, client forecast deliverables | **Available** |

**Use when**: Building MCP servers for data tools, implementing NL-to-SQL, using LLMs for data enrichment, designing AI-data interaction patterns, generating time-series forecasting pipelines with foundation models.

### Data Quality & Governance

| Skill | Type | Description | Status |
|-------|------|-------------|--------|
| [data-testing](data-testing/) | Standalone | Testing strategy for data pipelines — dbt test patterns, SQL assertions, pipeline validation, test coverage targets, test-as-deliverable packaging | **Available** |
| [data-governance](data-governance/) | Standalone | Data governance as engineering — cataloging (dbt docs, external tools), lineage, data classification (PII/PHI), access control (RBAC), compliance frameworks | **Available** |
| [data-observability](data-observability/) | Standalone | Pipeline monitoring and incident response — freshness, volume anomaly detection, schema change detection, alerting patterns, incident triage | **Available** |

**Use when**: Designing testing strategies, writing SQL assertions, implementing data governance, cataloging data assets, monitoring pipeline freshness, detecting anomalies, building incident response runbooks.

### Data Consulting

| Skill | Type | Description | Status |
|-------|------|-------------|--------|
| [duckdb](duckdb/) | Standalone | DuckDB for local data analysis, file ingestion (CSV/Excel/Parquet/JSON), data profiling, cleaning transformations, export patterns | **Available** |
| [client-delivery](client-delivery/) | Standalone | Consulting engagement lifecycle — discovery, schema profiling, security tier selection, project scaffolding, deliverable generation, client handoff | **Available** |
| [dlt-extract](dlt-extract/) | Standalone | DLT pipelines for file-based extraction, Excel/CSV/SharePoint ingestion, destination swapping (DuckDB dev → warehouse prod), schema contracts | **Available** |

**Use when**: Running data cleaning engagements, profiling client data locally with DuckDB, building portable DLT pipelines for file sources, generating client deliverables.

### General Automation & Productivity

| Skill | Type | Description | Status |
|-------|------|-------------|--------|
| [automation-master](automation-master/) | Standalone | Decomposes recurring multi-step tasks into re-runnable workflows with validation gates and approval steps | **Available** |
| [file-automation](file-automation/) | Standalone | Bulk rename, sort, and organize files via a reviewed manifest, dry run, and undo log | **Available** |
| [browser-automation](browser-automation/) | Standalone | Web navigation, form filling, and page data extraction via a browser tool or Playwright script | **Available** |
| [data-analysis](data-analysis/) | Standalone | Spreadsheet cleaning with audit log, trend analysis, and evidence-vs-interpretation reports | **Available** |
| [design-automation](design-automation/) | Standalone | Design briefs: dimensions, typography, color (WCAG contrast), layout, asset lists | **Available** |
| [research-agent](research-agent/) | Standalone | Sourced research with every claim labeled Verified / Recalled / Unresolved | **Available** |
| [email-automation](email-automation/) | Standalone | Thread summaries, reply drafts, and templates; human approval before any send | **Available** |

**Use when**: Systematizing repetitive work, organizing files, extracting data from web UIs, analyzing ad-hoc spreadsheets, briefing designers, researching with citations, handling email.

---

## Shared References

| Resource | Description | Referenced By |
|----------|-------------|---------------|
| [data-quality-patterns](shared-references/data-engineering/data-quality-patterns.md) | Tool-agnostic quality frameworks (four pillars, anomaly detection, alerting matrix) | dbt-transforms, python-data-engineering, event-streaming |
| [warehouse-comparison](shared-references/data-engineering/warehouse-comparison.md) | Decision matrix: Snowflake vs BigQuery vs Databricks vs DuckDB | All skills |
| [security-compliance-patterns](shared-references/data-engineering/security-compliance-patterns.md) | Three-tier security framework, credential management, data classification, AI-specific risks, compliance patterns (SOC2, HIPAA, PCI, GDPR) | **All skills** |
| [security-tier-model](shared-references/data-engineering/security-tier-model.md) | Consulting security tiers (Schema-Only / Sampled / Full Access), per-tier tool config, client conversation guidance | client-delivery, dbt-transforms, dlt-extract, duckdb |
| [dlt-vs-managed-connectors](shared-references/data-engineering/dlt-vs-managed-connectors.md) | DLT vs Fivetran vs Airbyte decision matrix, consulting context factors, hybrid patterns | data-integration, dlt-extract, client-delivery |

**Use when**: Choosing a warehouse platform, implementing data quality checks, understanding security requirements, managing credentials across tools, selecting consulting engagement data access tiers, choosing between DLT and managed connectors.

---

## Skill Sizing

| Skill | Core Lines | Reference Lines | Total Lines | File Count | Status |
|-------|------------|-----------------|-------------|------------|--------|
| dbt-transforms | 1,200 | 1,800 | 3,000 | 7 | Available |
| data-integration | 850 | 2,800 | 3,650 | 7 | Available |
| event-streaming | 1,000 | 1,500 | 2,500 | 6 | Available |
| data-pipelines | 900 | 1,600 | 2,500 | 6 | Available |
| python-data-engineering | 1,100 | 1,600 | 2,700 | 6 | Available |
| ai-data-integration | 700 | 1,050 | 1,750 | 5 | Available |
| tsfm-forecast | ~180 | ~550 | ~730 | 10 | Available |
| duckdb | 180 | 510 | 690 | 7 | Available |
| client-delivery | 190 | 570 | 760 | 7 | Available |
| dlt-extract | 180 | 570 | 750 | 7 | Available |
| data-testing | 150 | 700 | 850 | 7 | Available |
| data-governance | 160 | 950 | 1,110 | 8 | Available |
| data-observability | 160 | 1,000 | 1,160 | 8 | Available |
| **Shared references** | - | 2,500 | 2,500 | 5 | Available |
| **Total** | 7,570 | 17,450 | 25,020 | 81 | - |

**Sizing explanation**:
- **Core lines**: SKILL.md prompt content (always included)
- **Reference lines**: Deep-dive reference files (loaded on demand via progressive disclosure)
- **Total lines**: Sum of core + references
- **File count**: Total files per skill (SKILL.md + references)

---

## Security Tiers

All skills support three security tiers. Choose based on your organization's requirements:

| Tier | Description | AI Can Do | AI Cannot Do |
|------|-------------|-----------|-------------|
| **Tier 1: Cloud-Native** | Standard cloud security | Execute against dev/staging, read sample data | Access production, modify IAM |
| **Tier 2: Regulated** | SOC2/HIPAA/PCI environments | Read schemas/metadata, generate code for review | Execute against production, see row-level data |
| **Tier 3: Air-Gapped** | Maximum restriction | Generate code, SQL, YAML, configs | Connect to any data system |

See [Security & Compliance Patterns](shared-references/data-engineering/security-compliance-patterns.md) for full details.

---

## Role-Based Recommendations

Not sure which skills to install? Here's what we recommend by role:

| Role | Skills to Install | Why |
|------|------------------|-----|
| **Analytics Engineer** | dbt-transforms, python-data-engineering | Transform and model data using SQL and Python |
| **Data Platform Engineer** | All skills | Full toolkit for building and maintaining data platforms |
| **Integration Engineer** | data-integration, event-streaming, data-pipelines | Connect systems, orchestrate pipelines, handle real-time data |
| **ML Engineer** | python-data-engineering, ai-data-integration, tsfm-forecast | Python-first workflows, AI/ML pipelines, zero-shot time-series forecasting |
| **Data Scientist** | dbt-transforms, python-data-engineering | Model data for analysis, write Python transformations |
| **Data Consultant** | dbt-transforms, duckdb, client-delivery, dlt-extract, data-pipelines, data-testing | End-to-end data cleaning engagements, from profiling to deliverables |

**Install by role**:
```bash
./install.sh --role analytics-engineer
./install.sh --role data-platform-engineer
./install.sh --role integration-engineer
./install.sh --role ml-engineer
./install.sh --role data-consultant
```

---

## Quick Start Examples

### "I want to ingest data from a REST API using DLT"

```bash
./install.sh --skills data-integration
```

Then ask Claude:
> "Help me build a DLT pipeline to ingest data from the GitHub API into Snowflake"

**Skill activates**: data-integration detects keywords "DLT", "pipeline", "ingest", "Snowflake"

### "I want to connect Salesforce to Snowflake"

```bash
./install.sh --skills data-integration
```

Then ask Claude:
> "How do I set up Fivetran to sync Salesforce to Snowflake?"

**Skill activates**: data-integration detects keywords "Fivetran", "Salesforce", "Snowflake"

### "I need to write dbt models with tests"

```bash
./install.sh --skills dbt-transforms
```

Then ask Claude:
> "Help me write a dbt staging model for Stripe charges with data quality tests"

**Skill activates**: dbt-transforms detects keywords "dbt", "staging model", "tests"

### "A client sent me messy Excel files to clean up"

```bash
./install.sh --role data-consultant
```

Then ask Claude:
> "I need to set up a data cleaning engagement — the client sent me 5 Excel files with customer and order data"

**Skill activates**: client-delivery detects keywords "cleaning engagement", "client", "Excel files"

### "I'm building a real-time pipeline with Kafka"

```bash
./install.sh --skills event-streaming
```

Then ask Claude:
> "How do I set up Kafka Connect to stream orders to BigQuery?"

**Skill activates**: event-streaming detects keywords "Kafka", "stream", "BigQuery"

---

## What's Next?

### Phase 0: Security Foundation (Complete)
- Security & Compliance Patterns shared reference
- Security Posture sections added to all existing skills
- Credential management best practices across all code examples

### Phase 1: Orchestration + DLT (Complete)
- data-pipelines (Dagster-first, Airflow secondary, dagster-dbt + dagster-dlt integrations)
- DLT reference module added to data-integration
- DLT section and decision matrix added to data-integration SKILL.md

### Phase 2: Python Data Engineering (Complete)
- python-data-engineering (Polars-first, Pandas, PySpark, dbt Python models, API extraction, data validation)

### Phase 3: AI Data Integration (Complete)
- ai-data-integration (MCP servers, NL-to-SQL, embeddings, LLM transforms)
- 4-level maturity model with security tier integration

### Phase 4: Data Consulting Extension (Complete)
- duckdb (local data analysis, file ingestion, profiling)
- client-delivery (engagement lifecycle, profiling, deliverables)
- dlt-extract (file-based DLT pipelines, destination swapping)
- Consulting security tier model (Schema-Only / Sampled / Full Access)
- Scripts (schema_profiler.py, sample_extractor.py)
- Templates (engagement, dbt, DLT)

### Phase 5: Data Quality, Governance & Observability (Complete)
- data-testing (testing strategy, SQL assertions, pipeline validation, test-as-deliverable)
- data-governance (cataloging, lineage, classification, access control, compliance frameworks)
- data-observability (freshness monitoring, volume anomaly detection, schema change detection, alerting, incident response)

### Community Expansion
- Additional skills via community PRs
- Quality bar: must match existing skill depth and security conventions

---

## Contributing

We welcome contributions! See [CONTRIBUTING.md](CONTRIBUTING.md) for guidelines.

**Ways to contribute**:
- Report issues or request features via [GitHub Issues](https://github.com/dtsong/data-engineering-skills/issues)
- Submit pull requests for new skills or reference materials
- Share feedback on skill quality and usefulness

---

## License

Apache License 2.0

Copyright 2026 Daniel Song

See [LICENSE](LICENSE) for full text.

---

## Related Resources

- [Claude Code Documentation](https://github.com/anthropics/claude-code)
- [dbt Documentation](https://docs.getdbt.com/)
- [DLT Documentation](https://dlthub.com/docs/)
- [Dagster Documentation](https://docs.dagster.io/)
- [Apache Airflow Documentation](https://airflow.apache.org/docs/)
- [Fivetran Documentation](https://fivetran.com/docs)
- [Airbyte Documentation](https://docs.airbyte.com/)
- [Apache Kafka Documentation](https://kafka.apache.org/documentation/)
- [Snowflake Documentation](https://docs.snowflake.com/)
- [BigQuery Documentation](https://cloud.google.com/bigquery/docs)
- [Databricks Documentation](https://docs.databricks.com/)

---

**Questions?** Open an issue at [github.com/dtsong/data-engineering-skills/issues](https://github.com/dtsong/data-engineering-skills/issues)
