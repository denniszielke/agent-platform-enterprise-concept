---
layout: default
title: Operations Capabilities
parent: Enterprise Capabilities
nav_order: 8
---

# Operations capabilities

Operations capabilities cover observability (22), AI FinOps (23) and the enterprise AI
enablement operating model (24). They determine whether the platform survives the
transition from an exciting programme to routine enterprise infrastructure.

## Observability (22)

Agent observability extends classic telemetry with quality and behaviour signals.

| Signal type | Examples | Primary consumer |
| --- | --- | --- |
| Traces | End-to-end span per interaction across agent, tools and model calls | Engineers |
| Metrics | Latency, throughput, error rate, token consumption, tool success rate | Operations |
| Logs | Prompt and response records with redaction, tool arguments and results | Investigation |
| Quality | Groundedness, citation coverage, evaluation scores, refusal rate | Product owner |
| Business | Task completion, escalation to human, user satisfaction | Sponsor |

One correlated trace per user interaction is the minimum bar. Without correlation across
the agent, its tools and its model calls, incidents cannot be diagnosed and cost cannot
be attributed.

{: .note }
> Prompt and response logging carries privacy obligations. Define redaction, retention
> and access rules for telemetry with the same rigour as for agent memory.

## AI FinOps (23)

Consumption-based AI spend needs the same discipline as cloud spend, with an additional
twist: cost per interaction is influenced by prompt design, retrieval strategy, model
choice and agent loop behaviour, all of which change frequently.

Core practices:

- **Attribution** — every model and tool call carries scenario, owner and cost-centre
  metadata, emitted by the gateway rather than reconstructed later.
- **Unit economics** — cost per completed task or conversation, tracked as a trend and
  compared to the value the scenario delivers.
- **Budgets and quotas** — enforced at the gateway so overspend is prevented, not merely
  reported.
- **Optimisation** — model right-sizing, caching, prompt and context trimming, retrieval
  tuning, and batch processing for non-interactive work.
- **Forecasting** — capacity and commitment planning based on observed growth.

## Enterprise AI enablement operating model (24)

The operating model is the human side of the platform: the roles, forums, funding and
skills that keep it running.

| Element | Purpose |
| --- | --- |
| Platform team | Owns the paved road, guardrails and shared services |
| Architecture review | Approves patterns and exceptions, keeps designs coherent |
| AI governance forum | Owns policy, risk classification and high-risk approvals |
| Enablement and community | Documentation, training, office hours, reusable examples |
| Funding model | How platform cost is funded and how consumption is recharged |

Enablement is a delivery capability, not a communications activity. Documentation quality,
working examples and responsive support determine whether teams use the paved road or
build around it.

## Operating model differences

| Aspect | Centralised | Federated |
| --- | --- | --- |
| On-call | Single central rotation | Domain on-call, central platform rotation |
| Telemetry | One central workspace | Domain workspaces with central aggregation |
| Budgets | Central budget with showback | Domain budgets with chargeback |
| Standards | Enforced by central delivery | Enforced by policy and review |
| Enablement | Training on consuming agents | Training on building and operating agents |

See [Observability]({{ site.baseurl }}/04-Cross-Cutting-Topics/Observability.html) and
[FinOps]({{ site.baseurl }}/04-Cross-Cutting-Topics/FinOps.html) for the cross-cutting
detail.
