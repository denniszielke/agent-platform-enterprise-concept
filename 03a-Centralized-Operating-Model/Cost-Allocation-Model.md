---
layout: default
title: Cost Allocation Model
parent: Centralized Operating Model
nav_order: 6
---

# Cost Allocation Model

Sustainable enterprise AI requires transparent cost management from the first agent deployment. In the centralized operating model, the platform team owns the full cost allocation machinery: tagging standards, chargeback and showback mechanisms, unit economics reporting, and quota enforcement. This document defines how AI platform costs are measured, attributed, and governed.

---

## Cost Allocation Philosophy

AI workloads have a cost profile that differs significantly from traditional software:

- **Token-based pricing** — Model API costs scale with consumption volume, not just with the number of running instances.
- **Latency-cost trade-offs** — Higher-capability models cost more per token but may reduce total tokens needed by completing tasks in fewer turns.
- **Storage-intensive retrieval** — Vector databases and embedding pipelines introduce storage and compute costs that are less predictable than relational database costs.
- **Bursty demand** — Agents can spike token consumption dramatically during peak periods, making budget forecasting challenging.

The cost allocation model addresses these characteristics by combining **resource tagging** (to attribute cost to the right owner), **unit economics** (to express cost in business-meaningful terms), and **budgets and quotas** (to prevent runaway spend).

{: .highlight }
> The primary goal of cost allocation in the centralized model is not to create a billing bureaucracy — it is to make AI costs visible and understandable to every stakeholder, from the CFO reviewing an enterprise technology budget to the domain team lead evaluating whether an agent is delivering value.

---

## Tagging Standard

Every resource provisioned on the platform carries a mandatory tag set. Tags are enforced at deployment time by the CI/CD pipeline and by Azure Policy; resources without required tags cannot be created.

| Tag | Example Value | Purpose |
|---|---|---|
| `platform` | `agent-platform` | Identifies all resources belonging to the AI agent platform |
| `environment` | `production`, `staging`, `sandbox` | Environment segmentation for cost reporting |
| `domain` | `finance`, `hr`, `supply-chain` | Attributes cost to the requesting business domain |
| `agent-id` | `invoice-processing-v2` | Attributes cost to a specific agent |
| `cost-centre` | `CC-10042` | Maps to finance system for chargeback |
| `data-classification` | `confidential`, `internal`, `public` | Supports data-aware cost and security reporting |
| `capability` | `model-gateway`, `vector-store`, `orchestration` | Enables capability-level cost breakdown |

Tags are propagated from the infrastructure layer to the billing export and to the observability pipeline, enabling joined reporting across operational metrics and financial data.

---

## Showback and Chargeback

The platform supports two financial allocation modes:

### Showback

In showback mode, the platform team reports AI costs to domain teams for awareness and planning purposes — but costs are not recharged to domain budgets. This is the recommended starting point:

- Low administrative overhead — no inter-department billing processes required.
- Builds cost awareness and encourages sensible design choices without creating political friction.
- Appropriate while the platform is still maturing and cost attribution accuracy is being validated.

### Chargeback

In chargeback mode, AI platform costs are recharged to domain cost centres based on measured consumption. Moving to chargeback requires:

1. **Attribution accuracy validation** — Tag coverage must be near 100% and cost attribution must be audited against actual model API invoices.
2. **Agreed unit rates** — The platform team and finance agree on the internal transfer price per unit of consumption (see Unit Economics below).
3. **Dispute resolution process** — A lightweight process for domain teams to query allocations and request corrections.
4. **Finance system integration** — Automated export of monthly chargeback data to the enterprise finance system.

{: .note }
> Most organisations begin with showback and transition to chargeback as tagging coverage improves and domain teams gain confidence in the attribution methodology. A 6–12 month showback period before chargeback is typical.

---

## Unit Economics

Expressing AI costs in per-unit terms makes them interpretable to non-technical stakeholders and enables ROI analysis at the agent level.

### Primary Unit Metrics

| Unit Metric | Definition | Example |
|---|---|---|
| **Cost per conversation** | Total platform cost (model, retrieval, orchestration, infra) attributed to one agent session | A customer service agent costs £0.18 per resolved conversation |
| **Cost per task** | Total cost for a completed agent task (e.g., document review, data extraction) | An invoice processing agent costs £0.04 per processed invoice |
| **Cost per 1K tokens** | Blended cost per 1,000 tokens including infrastructure overhead | Effective cost after overhead: £0.004 per 1K tokens on standard tier |
| **Cost per query (RAG)** | Combined embedding, retrieval, and reranking cost per search operation | £0.0008 per RAG query including vector store amortisation |
| **Infrastructure cost per agent/day** | Amortised compute, storage, and networking cost per active agent per day | £1.20/day for a continuously running orchestration agent |

### Unit Economics Dashboard

The platform publishes a unit economics dashboard refreshed daily, visible to:

- **Platform team** — Full breakdown across all agents and domains.
- **Domain team leads** — Their domain's agents only.
- **Finance and FinOps** — Aggregated view for budget management and forecasting.

Unit cost trends are tracked over time to identify inefficiencies (e.g., model over-provisioning, low cache-hit rates inflating retrieval costs) and to measure the cost impact of optimisation initiatives.

---

## Budgets and Quotas

### Token Quotas

Each registered agent and each business domain is assigned token quotas that are enforced at the model gateway:

| Quota Type | Scope | Enforcement Mechanism |
|---|---|---|
| Per-agent token budget (daily) | Individual agent | Model gateway rejects requests when daily budget exhausted |
| Per-agent token budget (monthly) | Individual agent | Warning at 80%, block at 100% (configurable) |
| Per-domain token budget (monthly) | Business domain aggregate | Warning at 80%, escalation to domain lead |
| Platform total budget (monthly) | All agents | Finance alert, executive escalation |

Quota breaches trigger a structured response:

1. **Warning (80%)** — Automated notification to domain lead and platform FinOps team.
2. **Soft limit (100% of budget)** — New requests for non-critical agents are queued or rejected with a clear error; critical agents continue with enhanced monitoring.
3. **Hard limit** — Set at 120% of budget; all requests for the agent are rejected until a budget increase is approved or the period resets.

### Azure Cost Budgets

In addition to token quotas, Azure cost budgets are set at multiple levels:

- **Subscription level** — Enterprise AI platform total monthly spend.
- **Resource group level** — Per-domain resource groups with domain-specific budgets.
- **Resource tag filter** — Per-agent cost budgets using tag-based budget scopes.

Budget alerts trigger notification emails and, for critical thresholds, PagerDuty alerts to the platform FinOps team.

---

## Cost Optimisation Practices

The platform team operates a continuous cost optimisation programme:

| Practice | Description | Typical Saving |
|---|---|---|
| **Model right-sizing** | Route tasks to the minimum-capable model that meets quality requirements | 40–70% on model spend for simple tasks |
| **Response caching** | Cache identical or near-identical prompts and responses | 10–30% for high-repetition use cases |
| **Semantic caching** | Cache responses for semantically similar queries using embedding similarity | 15–40% for FAQ-style agents |
| **Batch processing** | Non-real-time tasks processed in asynchronous batches at lower model tier rates | 50% on batch-eligible model costs |
| **Embedding amortisation** | Reuse embeddings across agents rather than re-embedding shared documents | Significant savings at knowledge platform scale |
| **Reserved capacity** | Commit to throughput units in advance for predictable workloads | 20–40% vs pay-as-you-go |

Cost optimisation recommendations are surfaced automatically in the unit economics dashboard based on consumption patterns. Domain teams receive monthly cost optimisation suggestions with estimated savings.

---

## Reporting Cadence

| Report | Audience | Frequency | Delivery |
|---|---|---|---|
| Real-time cost dashboard | Platform FinOps, domain leads | Live | Internal portal |
| Daily token consumption summary | Domain leads | Daily | Email + portal |
| Monthly chargeback statement | Domain finance leads, CFO office | Monthly | Finance system + portal |
| Quarterly unit economics review | Platform leadership, domain leads | Quarterly | Presentation + report |
| Annual AI FinOps maturity review | CTO, CFO, platform team | Annual | Report |
