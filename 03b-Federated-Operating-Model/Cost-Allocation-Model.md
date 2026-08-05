---
layout: default
title: Cost Allocation Model
parent: Federated Operating Model
nav_order: 6
---

# Cost Allocation Model

Cost allocation in the federated operating model distributes financial accountability alongside operational ownership. Domain teams that build and operate their own agents are also accountable for the costs those agents generate. The central platform team owns the billing infrastructure, tagging standards, and the shared service cost pool; domain teams manage their own budgets and are incentivised through transparent unit economics to design cost-efficient agents.

The federated cost model extends the [centralized cost allocation foundations](../03a-Centralized-Operating-Model/Cost-Allocation-Model.md) with domain-level accountability structures, intra-platform transfer pricing for shared services, and a more mature FinOps practice that domain teams participate in directly.

---

## Cost Ownership Model

In the federated model, costs are partitioned into two pools:

### Shared Platform Cost Pool

Costs for central shared services that all domain teams consume:

| Shared Service | Cost Attribution Method |
|---|---|
| Model gateway infrastructure | Pro-rated by token consumption per domain |
| Central identity plane | Pro-rated by registered managed identity count per domain |
| Central observability pipeline | Pro-rated by telemetry volume per domain |
| Agent & MCP registry | Flat per-domain allocation |
| Platform developer portal | Flat per-domain allocation |
| Central SRE (shared services) | Pro-rated by incident volume and agent count |

The shared platform cost pool is calculated monthly and allocated to domains via the intra-platform transfer pricing mechanism (see below).

### Domain Cost Pool

Direct costs incurred by a domain team's own workloads:

- Domain spoke infrastructure (compute, storage, networking).
- Domain Key Vault and domain-managed secrets operations.
- Domain knowledge indexes (vector storage, embedding compute).
- Domain agent runtime compute (containers, serverless).
- Direct model API costs routed through the central model gateway (attributed via token metering tags).
- Domain-specific tool server hosting.

Domain teams are fully accountable for their domain cost pool and manage it against their approved budget.

---

## Tagging Standard

The federated model inherits the enterprise tagging standard and adds domain-level mandatory tags:

| Tag | Example Value | Mandatory Level | Purpose |
|---|---|---|---|
| `platform` | `agent-platform` | Enterprise | Identifies all platform resources |
| `environment` | `production`, `staging` | Enterprise | Environment segmentation |
| `domain` | `finance`, `hr` | Enterprise | Domain attribution |
| `agent-id` | `contract-review-v1` | Domain | Per-agent attribution |
| `cost-centre` | `CC-20015` | Domain | Finance system chargeback |
| `data-classification` | `confidential` | Domain | Data-aware reporting |
| `capability` | `knowledge-index`, `orchestration` | Domain | Capability-level breakdown |
| `domain-owner` | `alice.smith@contoso.com` | Domain | FinOps accountability contact |

Domain teams are responsible for applying all domain-level mandatory tags to resources they provision. The CI/CD pipeline blocks deployments with missing tags. Central platform resources are tagged centrally before shared cost allocation.

---

## Intra-Platform Transfer Pricing

To allocate shared platform costs to domain teams fairly, the platform publishes an internal transfer price list, reviewed quarterly:

| Service Unit | Internal Transfer Price Basis | Example |
|---|---|---|
| Model gateway token processing | Per 1,000 tokens, by model tier | Standard tier: internal rate reflects cloud cost + 15% overhead |
| Central observability ingestion | Per GB of telemetry per month | Reflects Log Analytics ingestion cost + pipeline overhead |
| Identity plane operations | Per registered managed identity per month | Flat rate covering Entra ID, Key Vault CA, audit log storage |
| Registry operations | Per registered agent per month | Flat rate covering storage, API, portal compute |
| Platform SRE shared coverage | Per active production agent per month | Covers shared on-call coverage for platform-layer incidents |

Transfer prices are published in the developer portal. Domain teams use them for budget planning and ROI calculations. The central FinOps team reviews and updates transfer prices quarterly based on actual cloud cost data.

{: .note }
> Transfer prices include a modest overhead margin that funds the central platform team's continuous improvement work (golden path development, new capability onboarding, security reviews). This is disclosed transparently to domain finance leads.

---

## Showback and Chargeback

### Showback Phase

New domains entering the federated model begin with showback: they receive detailed cost reports but are not yet recharged. Showback for a new domain typically runs for the first 2–3 months of operation, allowing:

- Validation that tagging coverage is complete and attribution is accurate.
- Domain team education on cost drivers and optimisation opportunities.
- Baseline establishment for budget negotiation.

### Chargeback Phase

Once tagging coverage exceeds 98% and the domain team has completed FinOps onboarding, chargeback activates:

- **Domain cost pool** — Recharged directly to the domain's cost centre monthly.
- **Shared platform pool allocation** — Recharged based on the transfer pricing mechanism.
- **Dispute window** — 10 business days post-statement for domain teams to query allocations.
- **Finance system integration** — Automated export to the enterprise general ledger.

{: .highlight }
> Chargeback is a governance mechanism as much as a financial one. When domain teams pay for their own token consumption, agent design decisions change: teams choose minimum-capable models, enable caching, and challenge whether every use case genuinely requires an agentic approach. This discipline is healthy and is one of the federated model's key advantages over central cost pooling.

---

## Unit Economics

Domain teams are required to measure and report unit economics for all production agents:

### Primary Unit Metrics

| Unit Metric | Domain Team Requirement | Platform Tooling |
|---|---|---|
| **Cost per conversation** | Measured and published for all conversational agents | Automatic — derived from token tags + infra cost allocation |
| **Cost per task** | Measured for all task-automation agents | Automatic for standard patterns; domain team extends for custom logic |
| **Domain AI ROI** | Annual review against defined business value metric | Domain team calculation; platform provides cost input |
| **Cost trend vs baseline** | Month-on-month comparison | Automatic — unit economics dashboard |

### Shared Unit Economics Benchmarks

The central FinOps team publishes anonymised cross-domain unit economics benchmarks quarterly. Domain teams can compare their agents' unit costs against the enterprise range, helping identify outliers that warrant optimisation investigation.

---

## Budgets and Quotas

### Domain Budget Structure

Each domain team negotiates an annual AI budget with the central FinOps team, composed of:

1. **Domain direct costs** — Domain infrastructure, compute, storage, domain-specific services.
2. **Model API allocation** — Monthly token budget per model tier, enforced at the model gateway.
3. **Shared platform allocation** — Estimated transfer pricing charges based on planned agent count and usage.

### Quota Enforcement

Token quotas are set per domain and per agent at the model gateway:

| Quota Type | Enforcement | Response to Breach |
|---|---|---|
| Per-agent daily token budget | Model gateway rejects at 100% | Domain team receives real-time alert at 80%; gateway returns 429 with retry-after at 100% |
| Per-domain monthly token budget | Model gateway warning at 80% | Domain lead notified; central FinOps review triggered at 90% |
| Platform-wide emergency budget | Manual activation | CFO-level approval required; domains notified |

Domain teams can request quota increases via the developer portal with business justification. The central FinOps team processes standard requests within 3 business days.

### Domain FinOps Practices

Domain teams are expected to maintain basic FinOps hygiene:

- Review the domain cost dashboard weekly.
- Act on unit economics optimisation recommendations within 30 days.
- Participate in the quarterly cross-domain FinOps review.
- Maintain a domain FinOps backlog in the developer portal (visible to the central team).

---

## Reporting Cadence

| Report | Owner | Audience | Frequency |
|---|---|---|---|
| Domain cost dashboard | Platform FinOps (data) + domain team (review) | Domain team lead, domain engineers | Real-time |
| Domain monthly cost statement | Platform FinOps | Domain lead, domain finance | Monthly |
| Shared platform allocation invoice | Platform FinOps | Domain finance, CFO office | Monthly |
| Cross-domain unit economics benchmark | Platform FinOps | All domain FinOps leads | Quarterly |
| Enterprise AI FinOps review | Platform FinOps | CTO, CFO, all domain leads | Quarterly |
| Annual AI investment vs value report | Platform FinOps + domain leads | Board / ExCo | Annual |
