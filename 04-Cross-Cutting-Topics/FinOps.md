---
layout: default
title: FinOps
parent: Cross-Cutting Topics
nav_order: 5
---

# FinOps

FinOps for enterprise AI extends the cloud financial management discipline to address the unique economics of model inference, training, and agent workloads. Where cloud FinOps focuses on compute and storage unit costs, AI FinOps must also account for token economics, model selection trade-offs, and the coupling between prompt design and cost. The goal is to maximise the business value delivered per unit of AI spend.

## Supported Capabilities

| Capability | FinOps Role |
|---|---|
| **23 – AI FinOps** | Core: budgeting, optimisation, commitment management, reporting |
| **1 – Billing & Commercial Management** | Enterprise agreements, committed use discounts, invoice reconciliation |
| **15 – Model Gateway** | Enforcement point for budgets, routing optimisation, caching |
| **22 – Observability** | Source of the cost telemetry that drives FinOps decisions |
| **18 – Evaluation Engineering** | Quality metrics that balance cost-quality trade-offs |
| **2 – Resource Organisation** | Subscription hierarchy that structures financial reporting |

## The AI FinOps Maturity Model

FinOps maturity for AI workloads typically progresses through three phases:

| Phase | Characteristics | Key Activities |
|---|---|---|
| **Inform** | Costs are visible but not yet controlled | Tagging, dashboards, show-back reporting |
| **Optimise** | Active steps taken to reduce waste and improve efficiency | Caching, model routing, prompt optimisation, commitment purchasing |
| **Operate** | Cost management is embedded in development and operational workflows | Automated budget enforcement, FinOps reviews in sprint ceremonies, cost-as-code policies |

## Token Economics

Model inference costs are driven by token consumption. Understanding the components enables targeted optimisation:

| Token Type | Cost Driver | Optimisation Levers |
|---|---|---|
| **System prompt tokens** | Repeated on every request | Cache static system prompts; reduce verbosity; move stable content to retrieval |
| **User input tokens** | Variable; driven by application design | Input validation; chunking strategies; summarisation of long contexts |
| **Retrieved context tokens** | RAG retrieval size | Tune retrieval top-k; use semantic reranking to select fewer, better chunks |
| **Completion tokens** | Model verbosity | Constrain output length; use structured outputs; chain shorter calls |
| **Cached tokens** | Prompt prefix caching | Design prompts with stable prefixes to maximise cache hit rates |

{: .note }
> Prompt prefix caching (available on leading hosted model providers) can reduce effective input token costs substantially for agents with stable system prompts. The Model Gateway should be configured to use provider-side caching where available.

## Model Routing and Selection

Not every agent task requires the most capable—and most expensive—model. A tiered routing strategy deployed in the Model Gateway (Capability 15) can route requests to the appropriate tier:

| Tier | Use Cases | Relative Cost |
|---|---|---|
| **Lightweight / fast** | Classification, intent detection, simple extraction | Low |
| **Mid-tier** | Summarisation, structured data generation, tool selection | Medium |
| **Frontier** | Complex reasoning, multi-step planning, creative generation | High |
| **Specialised** | Code generation, domain-specific fine-tuned models | Variable |

Routing rules may be:
- **Static** – developers declare the required tier in agent configuration.
- **Quality-based** – evaluation scores from earlier tiers determine whether to escalate.
- **Budget-based** – when a team's budget is nearly exhausted, the gateway falls back to lower tiers.

## Commitment Management

Hosted model providers offer committed-use pricing in several forms:

- **Provisioned throughput / reserved capacity**: pay a fixed hourly rate for guaranteed tokens-per-minute; optimal for predictable, high-volume production workloads.
- **Committed use discounts**: multi-month commitments at reduced per-token rates.
- **Prepaid credits**: bulk credit purchase at a discount; consumed as workloads run.

The FinOps practice must align commitment purchasing with forecasted demand:

1. Use observability data to establish 90-day rolling average token consumption by model and environment.
2. Apply a buffer for growth (typically 20–30%) to set the commitment target.
3. Reserve only production workloads; keep development and evaluation on pay-as-you-go.
4. Review and right-size commitments quarterly.

## Budget Enforcement

Budgets should be enforced at multiple levels:

```
Enterprise budget
  └─ Product area budget
       └─ Team budget
            └─ Agent budget (enforced at Model Gateway)
                 └─ Per-session soft limit
```

The Model Gateway enforces agent-level budgets in real time. When an agent approaches its budget:

1. **Warning threshold (80%)** – alert sent to owning team; no action on requests.
2. **Soft limit (100%)** – requests routed to lower-cost fallback model.
3. **Hard limit** – requests rejected with a structured error; human intervention required.

Budget limits should be managed as configuration in the Agent & MCP Registry (Capability 20) and versioned alongside agent deployments.

## Centralized vs. Federated FinOps

| Dimension | Centralized | Federated |
|---|---|---|
| Budget Ownership | Central finance and platform team set all budgets | Domain teams own budgets; centre sets guardrails |
| Commitment Purchasing | Central team manages all provider contracts | Centre negotiates enterprise agreements; domains allocate their share |
| Optimisation Decisions | Central FinOps team drives optimisation campaigns | Domain teams optimise independently; share learnings via community of practice |
| Reporting Cadence | Monthly cross-organisation FinOps review | Domain reviews + quarterly enterprise roll-up |
| Tooling | Single FinOps tooling instance | Domain tooling federated to central reporting |

See [Centralized Operating Model](../03a-Centralized-Operating-Model/Overview.md) and [Federated Operating Model](../03b-Federated-Operating-Model/Overview.md).

## FinOps Optimisation Patterns

### Prompt Caching
Enable semantic or prefix caching at the gateway. Cache hits reduce costs significantly for repetitive queries such as FAQ bots or report generation agents.

### Output Caching
For deterministic queries (same input, same output), cache the full model response at the application layer. Suitable for classification, lookup, and templated generation tasks.

### Asynchronous Batching
Group non-real-time requests (e.g., overnight document processing) into batch inference jobs where providers offer lower batch pricing.

### Fine-Tuning for Cost Reduction
A smaller fine-tuned model can match frontier model quality for a specific, well-defined task at a fraction of the inference cost. Evaluate rigorously against quality baselines before substituting.

### Context Compression
Use summarisation or compression passes to reduce long conversation histories before feeding them back into the model context. Semantic Kernel and LangChain both provide memory management utilities for this.

### Evaluation-Driven Downgrades
Run evaluation harnesses (Capability 18) on both the current model and a cheaper alternative. If the cheaper model meets quality thresholds, migrate.

## FinOps Governance Reviews

FinOps should be a standing agenda item in the enterprise AI platform operating model (Capability 24):

- **Weekly** – anomaly review; any budget alerts from the prior week.
- **Monthly** – team-level spend vs. budget; optimisation actions and their impact.
- **Quarterly** – commitment right-sizing; model tier strategy review; cross-team sharing of optimisation learnings.

## Summary

AI FinOps is a continuous practice that requires tooling (Model Gateway metering, observability dashboards, budget enforcement), process (regular reviews, sprint integration), and culture (teams treat cost as a first-class quality attribute). Starting with visibility, moving to optimisation, and embedding cost discipline into engineering workflows is the proven path to sustainable AI investment.
