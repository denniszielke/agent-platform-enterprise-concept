---
layout: default
title: Observability Model
parent: Centralized Operating Model
nav_order: 5
---

# Observability Model

Observability in the centralized operating model is a first-class platform capability, not an afterthought. Because the platform team owns all agent workloads and infrastructure, it is uniquely positioned to instrument every layer of the stack uniformly and to provide consumers with rich, contextual telemetry without requiring them to implement their own monitoring. This document defines the signals collected, the pipelines that process them, and the dashboards and alerts that make the platform operationally excellent.

---

## Observability Philosophy

Traditional software observability (logs, metrics, traces) is necessary but insufficient for AI workloads. Agents introduce additional signals that are specific to the AI domain: token consumption, model latency distributions, retrieval quality scores, content filter decisions, tool call success rates, and hallucination indicators. The centralized observability model captures all of these signals in a unified pipeline and makes them available to both operational and governance consumers.

{: .highlight }
> Every agent interaction must produce a complete, linked trace from the user's initial prompt to the final response — including every tool call, model completion, retrieval operation, and human-in-the-loop event. This trace is the single source of truth for debugging, compliance, and cost attribution.

---

## The Four Signal Types

### 1. Traces (Distributed Tracing)

Every agentic request generates a distributed trace using OpenTelemetry. The trace spans the full call graph:

| Span Type | Example | Attributes Captured |
|---|---|---|
| User request | `agent.invoke` | User ID (hashed), agent ID, session ID, timestamp, input token estimate |
| LLM completion | `llm.chat_completion` | Model ID, prompt tokens, completion tokens, latency, finish reason |
| Tool invocation | `tool.execute` | Tool name, tool server ID, input hash, output hash, latency, success/failure |
| Retrieval | `retrieval.search` | Index name, query embedding hash, top-K, scores, latency |
| Memory read/write | `memory.get` / `memory.set` | Memory tier (short/long), key hash, hit/miss |
| Delegation | `agent.delegate` | Parent agent ID, child agent ID, delegated scope token hash |
| Human approval | `human.approve` | Approval request ID, timeout, outcome |

Traces are correlated by a `trace_id` and `session_id` that flow through every span. The platform's SDK injects these automatically; agent developers do not need to manage correlation IDs manually.

### 2. Metrics

Key metrics are pre-aggregated and available in real time:

| Metric | Unit | Aggregation | Use |
|---|---|---|---|
| `agent.request_rate` | req/min | Per agent, per domain | Capacity planning, rate limit alerting |
| `agent.p50_latency` / `p95` / `p99` | ms | Per agent | SLA monitoring |
| `llm.token_consumption` | tokens | Per model, per agent, per domain | Cost attribution, quota management |
| `llm.prompt_rejection_rate` | % | Per model, per content filter | Safety monitoring |
| `tool.success_rate` | % | Per tool server | Tool reliability monitoring |
| `retrieval.mean_reciprocal_rank` | 0–1 | Per index | RAG quality monitoring |
| `agent.error_rate` | % | Per agent | Reliability alerting |
| `memory.cache_hit_rate` | % | Per memory tier | Memory efficiency |

### 3. Logs

Structured JSON logs are emitted by every platform component. Log levels and sensitive data handling:

- **DEBUG** — Disabled in production by default; can be enabled per agent for a time-limited debugging window.
- **INFO** — Request/response summaries, configuration changes, lifecycle events.
- **WARN** — Degraded performance, rate limit approach, non-fatal errors.
- **ERROR** — Failed requests, tool errors, policy violations.

{: .warning }
> Actual prompt and completion text are **not** stored in standard logs due to data sensitivity. They are captured in a separate, access-controlled **AI interaction store** with stricter retention and access policies. Only the security & compliance team and designated incident responders can query raw interaction content.

### 4. AI-Specific Signals

Beyond the standard three pillars, the platform captures signals specific to AI workloads:

| Signal | Source | Purpose |
|---|---|---|
| Content filter decisions | Model gateway | Safety monitoring, policy effectiveness |
| Evaluation scores (automated) | Evaluation harness | Continuous quality monitoring in production |
| Grounding citations | RAG pipeline | Hallucination risk assessment |
| Prompt injection detections | AI runtime protection layer | Security monitoring |
| Model version distribution | Model gateway | Rollout monitoring, deprecation tracking |

---

## Telemetry Pipeline

The centralized observability pipeline processes all signals through a common path:

```
Agent SDK (OpenTelemetry) 
  → OpenTelemetry Collector (per-node sidecar) 
    → Central Collector Cluster (buffering, sampling, enrichment) 
      → Log Analytics Workspace (logs + traces) 
      → Prometheus / Azure Monitor Metrics (metrics) 
      → AI Interaction Store (restricted, encrypted at rest)
        → Dashboards, Alerts, Compliance Exports
```

Key pipeline properties:

- **Sampling** — All error traces are sampled at 100%. Success traces are sampled at a configurable rate (default 10%) to manage storage costs at scale.
- **Enrichment** — The central collector adds resource metadata (agent version, deployment environment, business domain tag, cost centre tag) to every span and log record.
- **Retention** — Operational telemetry is retained for 90 days by default. Compliance-relevant audit logs are retained for the duration required by the applicable regulation (e.g., 7 years for financial services).

---

## Dashboards and Alerting

### Operational Dashboards

| Dashboard | Primary Audience | Key Content |
|---|---|---|
| Platform health | Platform SRE | Overall request rates, error rates, p95 latency, infra health |
| Agent inventory | Platform team | All registered agents, deployment status, last active, version |
| Model gateway | AI/ML engineering | Token consumption by model, rejection rates, latency by model |
| Tool ecosystem | Platform engineering | Tool server availability, call rates, error rates |
| Domain view | Domain teams | Their agents only — usage, errors, cost |

### Alert Policies

Alerts are configured for the following conditions, with defined escalation paths:

| Alert | Threshold | Severity | Escalation |
|---|---|---|---|
| Agent error rate spike | > 5% in 5-minute window | High | PagerDuty → on-call SRE |
| Model gateway p99 latency | > 10 s sustained 5 min | High | On-call SRE |
| Content filter rejection surge | > 10× baseline in 15 min | Critical | Security on-call + platform SRE |
| Token quota 80% consumed | Per agent / per domain | Warning | Platform team + domain owner notification |
| Prompt injection detected | Any | Critical | Security on-call, incident opened automatically |
| Agent not responding to health check | 3 consecutive failures | High | On-call SRE |

---

## Compliance and Audit Observability

The observability pipeline feeds directly into the governance model:

- **Audit log export** — All identity events, policy violations, and lifecycle changes are exported to an immutable audit log store (append-only, tamper-evident).
- **Compliance reports** — The platform generates weekly compliance posture reports summarising policy violations, exception activations, and anomalous access events.
- **Chain-of-custody for AI decisions** — For high-consequence agent actions (financial approvals, content publication), the full trace — including the specific model version, system prompt hash, retrieved document IDs, and completion text — is preserved as a chain-of-custody record for the regulated retention period.

---

## Developer Observability Experience

Domain teams consuming the centralized platform receive observability benefits automatically:

- Their agents emit traces from Day 1, using the platform SDK's built-in OpenTelemetry instrumentation.
- A scoped, read-only view of their domain's observability data is available in the internal developer portal.
- Alerting for their agents is pre-configured using the platform's standard alert policies; teams can add domain-specific alerts via self-service.

This eliminates the observability setup burden that domain teams would otherwise face, and ensures a consistent signal format that the platform SRE team can act on during incidents. See the [Federated Operating Model — Observability](../03b-Federated-Operating-Model/Observability-Model.md) for how this changes when domain teams own their own observability stacks.
