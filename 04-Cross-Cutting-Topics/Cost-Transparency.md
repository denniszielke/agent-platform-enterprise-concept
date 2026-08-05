---
layout: default
title: Cost Transparency
parent: Cross-Cutting Topics
nav_order: 4
---

# Cost Transparency

Cost transparency is the discipline of making AI workload expenditure visible, attributable, and understandable to all stakeholders—from individual developers to finance leadership. Without it, organisations face budget surprises, cannot perform meaningful ROI analysis, and lack the evidence needed to govern model selection decisions. Cost transparency is a prerequisite for FinOps maturity (see [FinOps](./FinOps.md)) and directly informs enterprise billing and commercial management (Capability 1).

## Supported Capabilities

| Capability | Cost Transparency Role |
|---|---|
| **1 – Billing & Commercial Management** | Commercial contracts, invoice reconciliation, commitment management |
| **23 – AI FinOps** | Optimisation actions that cost transparency enables |
| **2 – Resource Organisation** | Subscription and resource hierarchy that carries cost tags |
| **15 – Model Gateway** | The metering point for all token consumption |
| **22 – Observability** | Telemetry pipeline that delivers cost signals |
| **7 – Identity & Trust** | Attribution of spend to an authenticated agent principal |

## What Needs to Be Visible

Enterprise AI cost has several components, not all of which appear on a single invoice:

| Cost Component | Description | Attribution Approach |
|---|---|---|
| **Model inference tokens** | Per-token charges for input and output from hosted models | Agent ID + model endpoint from gateway telemetry |
| **Embedding tokens** | Vector embedding generation for knowledge ingestion | Pipeline job ID, owning team |
| **Fine-tuning compute** | GPU/TPU time for model adaptation | Training job metadata |
| **Vector store storage** | Indexed document storage and query compute | Index name, owning domain |
| **Workflow orchestration** | Compute for orchestration engine steps | Workflow ID, triggering agent |
| **Memory persistence** | Storage for agent short- and long-term memory | Session ID, agent ID |
| **Tool call compute** | MCP server execution, code interpreter sandboxes | Tool ID, calling agent |
| **Egress and networking** | Data transfer for model API calls and retrieval | Network flow tags |

## Tagging Strategy

Consistent resource tagging is the foundation of cost attribution. The platform must enforce a minimum tag set on all resources:

- `environment` – production, staging, development
- `team` – owning domain or product team
- `agent_id` – for resources dedicated to a single agent
- `cost_centre` – finance code for charge-back
- `product` – the business product or use case the agent supports
- `criticality` – used to prioritise cost vs. reliability trade-offs

{: .note }
> Tags should be enforced at resource creation time through policy (e.g., Azure Policy deny-if-missing-tags). Retroactive tagging is expensive and error-prone.

## Metering Architecture

The Model Gateway (Capability 15) is the primary metering point for inference costs. Every request passing through the gateway is enriched with agent identity claims and written to the observability pipeline as a cost event:

```
Agent Request → Model Gateway
                  ├─ Route to model endpoint
                  ├─ Capture: agent_id, model, input_tokens, output_tokens, latency
                  └─ Emit cost event → Observability Pipeline → Cost Datastore
```

Cost events should include:

- `timestamp`
- `agent_id`, `team`, `cost_centre`
- `model_name`, `model_version`, `model_provider`
- `input_tokens`, `output_tokens`, `cached_tokens`
- `request_duration_ms`
- `unit_price_input`, `unit_price_output` (populated by a price catalogue service)
- `estimated_cost_usd` (calculated at event time)

A **price catalogue service** maintains current list prices and committed rates for each model and provider, enabling accurate real-time cost estimation even before invoices arrive.

## Cost Allocation Models

| Model | Description | When to Use |
|---|---|---|
| **Direct allocation** | Each team's agents consume from a dedicated capacity or subscription | Clear boundaries; simple governance; slightly higher overhead |
| **Shared pool with charge-back** | All agents share capacity; costs allocated by metered usage | Efficient utilisation; requires robust metering |
| **Show-back** | Costs are reported to teams but not formally charged | Early maturity stages; builds awareness without process change |
| **Commitment sharing** | Reserved capacity shared across teams; savings allocated pro-rata | Large, stable workloads with finance-approved commitments |

## Centralized vs. Federated Cost Transparency

| Dimension | Centralized | Federated |
|---|---|---|
| Billing Account Structure | Single billing account; finance team manages charge-back | Domain billing accounts under an enterprise management group |
| Metering Ownership | Platform team owns all metering infrastructure | Domain teams meter their own workloads; reports federated to centre |
| Cost Report Access | Central finance dashboard; teams see their allocation | Each domain sees their own costs; summary available to centre |
| Budget Enforcement | Platform team sets and enforces budgets | Domain teams set budgets within centre-defined limits |
| Anomaly Alerting | Central FinOps team receives all alerts | Domain teams own their alerts; escalate cross-domain anomalies |

See [Centralized Operating Model](../03a-Centralized-Operating-Model/Overview.md) and [Federated Operating Model](../03b-Federated-Operating-Model/Overview.md).

## Cost Transparency Dashboards

Effective dashboards for cost transparency serve multiple audiences:

**For development teams:**
- Cost per agent invocation (helps identify expensive prompts or workflows)
- Token breakdown: input vs. output vs. cached (highlights optimisation opportunities)
- Cost trend by day/week (surfaces regressions from code changes)

**For finance and leadership:**
- Total AI spend by product, team, and environment
- Month-over-month variance with annotations for major deployments
- Committed vs. consumed capacity utilisation
- Projected spend vs. budget

**For platform operations:**
- Cross-team cost concentration (identifies teams that need FinOps guidance)
- Model cost comparison (supports model selection decisions)
- Idle or orphaned resource spend

## Connecting Cost to Value

Cost transparency without a corresponding value signal is incomplete. Where possible, pair cost metrics with outcome metrics:

- *Cost per successful task completion*
- *Cost per automated decision*
- *Cost per document processed*

This framing shifts the conversation from "how much are we spending?" to "what are we getting for it?"—a prerequisite for confident investment decisions and budget negotiations.

## Summary

Cost transparency requires deliberate instrumentation, a consistent tagging strategy, a real-time metering pipeline through the Model Gateway, and dashboards tailored to each stakeholder's needs. Establishing this foundation early enables the organisation to move from reactive cost management to proactive FinOps optimisation.
