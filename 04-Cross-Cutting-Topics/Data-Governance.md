---
layout: default
title: Data Governance
parent: Cross-Cutting Topics
nav_order: 6
---

# Data Governance

Data governance for the enterprise agent platform establishes who can use which data, for what purpose, under what conditions, and with what safeguards. Agents amplify existing data governance challenges: they can consume large volumes of data autonomously, blend data from multiple sources in ways that create new sensitivities, and generate synthetic data that must itself be governed. A strong data governance framework is therefore both a compliance requirement and a trust foundation for the organisation's AI programme.

## Supported Capabilities

| Capability | Data Governance Role |
|---|---|
| **9 – Governance & Compliance** | Policy framework within which data governance rules are expressed |
| **17 – Knowledge & Data Platform** | The managed data layer where governance controls are enforced |
| **16 – Semantic Foundation** | Ontology and metadata that make governance rules machine-interpretable |
| **7 – Identity & Trust** | Data access control decisions depend on verified agent and user identity |
| **8 – AI Runtime Protection** | Prevents agents from surfacing governed data inappropriately |
| **10 – Lifecycle Automation** | Governance checks embedded in data pipeline CI/CD |
| **22 – Observability** | Audit trail of data access by agents |

## Data Classification

Every dataset consumed by agents must be classified before it enters the platform. A four-tier classification is commonly used in enterprise environments:

| Tier | Examples | Agent Access Rules |
|---|---|---|
| **Public** | Publicly available documents, open datasets | Any agent may access without additional controls |
| **Internal** | Internal wikis, non-sensitive operational data | Agents must be registered and deployed in an approved environment |
| **Confidential** | Customer data, financial records, HR data | Agents must present a data access justification; outputs must pass DLP checks |
| **Restricted / Regulated** | PII under GDPR, PHI under HIPAA, PCI-DSS card data | Explicit data steward approval required; agents run in isolated environments |

Classification labels should be propagated as metadata through the entire data pipeline—from ingestion, through the vector store or knowledge platform, to the retrieval results returned to an agent.

## Data Lineage

Agents frequently synthesise information from multiple sources. The platform must track data lineage so that:

- The source of every chunk of retrieved context can be traced.
- If a source dataset is updated, modified, or revoked, affected knowledge indexes can be identified and refreshed.
- Compliance officers can answer "did any agent use data from system X between dates Y and Z?"
- AI-generated outputs can be traced back to the source data that influenced them.

Lineage metadata should be stored alongside vector embeddings in the knowledge platform and surfaced in the observability pipeline with each retrieval event.

## Consent and Purpose Limitation

For data containing personal information, the platform must enforce purpose limitation—using data only for the purpose for which consent was obtained:

- Each dataset should carry a `permitted_purposes` attribute (e.g., `["customer_support", "fraud_detection"]`).
- Agent registrations must declare their `data_purposes`.
- The data access control layer should enforce that an agent's declared purposes are a subset of the dataset's permitted purposes.
- Consent withdrawal events must propagate through the pipeline, triggering deletion or anonymisation in indexes where the affected data was embedded.

{: .note }
> Vector embeddings may encode personal information in a form that is difficult to remove. Organisations subject to "right to erasure" obligations should implement index re-generation pipelines rather than relying solely on deletion of source records.

## Access Control on Knowledge Stores

The Knowledge & Data Platform (Capability 17) must enforce access control at multiple levels:

- **Index-level control**: agent principals are granted access to specific named indexes.
- **Document-level control**: metadata filters applied at query time restrict results to documents the requesting agent is authorised to see.
- **Field-level control**: for structured data sources, column-level security masks or excludes sensitive fields from retrieval results.

Access control policies should be managed in the central policy store and consumed by the knowledge platform at runtime, rather than hardcoded in individual agent configurations.

## Centralized vs. Federated Data Governance

| Dimension | Centralized | Federated |
|---|---|---|
| Data Classification Authority | Central data governance office classifies all datasets | Domain data stewards classify their own data; central office audits |
| Policy Authorship | Central team authors and publishes all data policies | Domain teams author domain-specific policies within a central framework |
| Lineage Tracking | Central data catalogue tracks lineage across all domains | Domain catalogues federated to a central meta-catalogue |
| Consent Management | Centralised consent management platform | Domain systems record consent; synchronise to central consent store |
| Breach Response | Central team coordinates data breach response | Domain teams lead their breach response; coordinate with central privacy office |

See [Centralized Operating Model](../03a-Centralized-Operating-Model/Overview.md) and [Federated Operating Model](../03b-Federated-Operating-Model/Overview.md).

## Data Quality for AI

Poor data quality produces poor agent outputs. The platform should enforce data quality standards at ingestion:

- **Completeness**: required fields are present before data is indexed.
- **Freshness**: documents older than a defined threshold are flagged for review or expiry.
- **Accuracy**: for high-stakes domains, data should pass validation rules before ingestion.
- **Consistency**: identifiers (product codes, customer IDs) are normalised to the canonical form defined in the Semantic Foundation (Capability 16).

Data quality metrics should be surfaced in observability dashboards and trigger alerts when they degrade.

## Synthetic and AI-Generated Data

Agents may generate synthetic data (test data, summaries, derived insights). This synthetic data must itself be governed:

- Label AI-generated content with provenance metadata (`generated_by`, `generating_model`, `generation_timestamp`).
- Do not use synthetic data to train or fine-tune production models without a validation and approval workflow.
- Apply the same DLP and classification checks to synthetic outputs as to source data.
- Retain synthetic data only as long as required; apply the same deletion policies as for source data.

## Retention, Archival, and Deletion

| Data Type | Typical Retention | Deletion Trigger |
|---|---|---|
| Prompt and completion logs | 90 days hot; 1 year cold | Consent withdrawal; regulatory requirement |
| Reasoning traces | 30 days hot; 90 days cold | Post-incident analysis window |
| Vector embeddings | Duration of knowledge index | Dataset retirement; consent withdrawal |
| Agent audit logs | 7 years (regulatory minimum in many jurisdictions) | Legal hold expiry |
| Cost and telemetry data | 13 months (budget cycle + 1 month) | Platform decommission |

Retention policies must be implemented in the underlying storage platforms and verified by automated compliance checks (Capability 9).

## Summary

Data governance for the agent platform is a multi-layered discipline spanning classification, access control, lineage, consent, quality, and retention. Embedding governance controls in the data pipeline infrastructure—rather than relying on individual agent implementations—provides consistent, auditable protection that scales with the growth of the agent estate.
