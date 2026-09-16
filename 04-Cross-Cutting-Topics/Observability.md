---
layout: default
title: Observability
parent: Cross-Cutting Topics
nav_order: 3
---

# Observability

Observability is the capability to understand the internal state of a system from its external outputs. For an enterprise agent platform, this means being able to answer: *Which agent ran, what did it receive, what did it decide, what did it call, and what was the outcome?* Traditional application observability (metrics, logs, traces) is necessary but not sufficient—agent platforms require a fourth dimension: **reasoning traces** that capture the chain of thought and tool-use decisions an agent made on the path to a response.

## Supported Capabilities

| Capability | Observability Role |
|---|---|
| **22 – Observability** | Core: defines the telemetry pipeline, instrumentation standards, and dashboards |
| **7 – Identity & Trust** | Telemetry is attributed to verified agent and user principals |
| **23 – AI FinOps** | Cost signals originate from observability instrumentation |
| **8 – AI Runtime Protection** | Anomaly detection and guardrail violation alerts feed from observability |
| **9 – Governance & Compliance** | Audit logs are a compliance artefact produced by the observability layer |
| **11 – AI Runtimes** | Runtime health metrics (latency, error rate, queue depth) surfaced here |
| **15 – Model Gateway** | Token usage, latency, and model routing decisions emitted as telemetry |

## The Four Pillars of Agent Observability

### 1. Metrics

Quantitative signals sampled over time. Key agent platform metrics:

| Metric | Description |
|---|---|
| `agent.invocation.count` | Total agent invocations per time period |
| `agent.latency.p99` | 99th-percentile end-to-end latency |
| `model.tokens.input` / `output` | Token consumption per model and agent |
| `tool.call.count` | MCP tool invocations per tool and agent |
| `tool.call.error_rate` | Fraction of tool calls returning errors |
| `guardrail.block.count` | Input/output blocks by guardrail type |
| `queue.depth` | Pending workflow tasks awaiting execution |
| `memory.retrieval.latency` | Vector store retrieval time |

### 2. Logs

Structured, event-based records. Every log entry from an agent should include:

- `trace_id` and `span_id` (OpenTelemetry-compatible)
- `agent_id`, `agent_version`
- `user_subject` (hashed or pseudonymised as required by data governance policy)
- `session_id`, `turn_id`
- `timestamp` (UTC, nanosecond precision)
- `event_type` (e.g., `tool_call`, `model_request`, `guardrail_block`, `memory_read`)
- Severity level

Prompt content and completion text require special handling: they may contain PII or sensitive business data. The platform should support configurable **log redaction policies** that apply before logs leave the agent runtime.

### 3. Traces

Distributed traces capture the causal graph of an agent's execution:

```
User Request
  └─ Orchestrator Agent (span: plan)
       ├─ Memory Retrieval (span: vector_search)
       ├─ Sub-Agent: DataAnalyst (span: agent_invoke)
       │    └─ MCP Tool: sql_query (span: tool_call)
       └─ Model Request: gpt-4o (span: model_call)
```

OpenTelemetry is the recommended standard. The platform trace context must propagate across:

- HTTP and gRPC calls between agents
- Message queue boundaries (include `traceparent` header in messages)
- MCP tool calls (add trace context to MCP request metadata)
- Workflow engine steps

### 4. Reasoning Traces

{: .note }
> Reasoning traces are the agent-specific addition to the classic three-pillar model. They record the agent's chain-of-thought, tool selection rationale, and decision points—essential for debugging unexpected behaviour and demonstrating governance compliance.

Reasoning traces should be stored separately from operational logs due to their size and sensitivity. They support:

- **Debugging**: understanding why an agent took a particular action
- **Evaluation**: comparing reasoning quality across model versions
- **Compliance**: demonstrating that the agent followed approved decision logic
- **Audit**: providing a human-readable narrative for auditors reviewing automated decisions

## Telemetry Pipeline Architecture

```
Agent Runtimes → OpenTelemetry Collector → Fan-out
                                           ├─ Metrics → Prometheus / Azure Monitor
                                           ├─ Logs → Log Analytics / OpenSearch
                                           ├─ Traces → Jaeger / Tempo / Application Insights
                                           └─ Reasoning Traces → Blob Storage (encrypted)
```

The OpenTelemetry Collector acts as the telemetry router and provides:

- Batching and compression to reduce egress volume
- Redaction processors for PII before data leaves the agent network
- Sampling policies (always-sample errors; probabilistic-sample successes)
- Routing to multiple backends without changes to agent code

## Centralized vs. Federated Observability

| Dimension | Centralized | Federated |
|---|---|---|
| Telemetry Backend | Single shared SIEM and metrics store | Domain-owned backends federated to central platform |
| Dashboard Access | Central SRE team creates and manages dashboards | Domain teams create dashboards within permitted namespaces |
| Alerting | Centralised alert routing and on-call | Domain on-call with escalation to central NOC |
| Data Retention | Centrally managed retention tiers | Domains own retention; minimum standards enforced by policy |
| PII Handling | Central redaction policy applied by collector | Domain-specific policies; central policy as minimum baseline |

See [Centralized Operating Model](../03a-Centralized-Operating-Model/Overview.md) and [Federated Operating Model](../03b-Federated-Operating-Model/Overview.md).

## Alerting and Anomaly Detection

Observability without actionable alerting is instrumentation without value. Key alert categories:

- **Availability**: agent error rate exceeds threshold; workflow queue depth growing unboundedly.
- **Latency**: P99 latency degrades beyond SLO threshold.
- **Cost**: token spend rate exceeds per-agent budget (feeds into FinOps—see [FinOps](./FinOps.md)).
- **Security**: guardrail block rate spikes; unusual tool call patterns; authentication failures.
- **Quality**: evaluation scores (from Capability 18) drop below acceptance threshold.

Anomaly detection for AI workloads benefits from adaptive baselines because prompt complexity and workload patterns vary more than traditional services. Consider ML-based anomaly detection for token consumption and tool call patterns.

## Dashboards and Personas

Different stakeholders need different views:

| Persona | Dashboard Focus |
|---|---|
| Platform SRE | Infrastructure health, error rates, queue depths, resource saturation |
| Agent Developer | Per-agent latency, tool call success rates, reasoning traces for debugging |
| Product Owner | User session counts, task completion rates, quality scores |
| FinOps Analyst | Token spend by agent, model, and team; budget utilisation |
| Compliance Officer | Guardrail block counts, audit log completeness, PII detection events |
| Security Analyst | Authentication anomalies, unusual egress, prompt injection attempts |

## Instrumentation Standards

To achieve consistent observability across a diverse agent estate:

- Adopt **OpenTelemetry SDKs** for all agent frameworks (LangChain, Semantic Kernel, custom runtimes).
- Define a **platform semantic conventions** document extending OTel's GenAI semantic conventions with enterprise-specific attributes (`cost_centre`, `environment`, `owning_team`).
- Require all registered agents to emit the defined minimum attribute set before promotion to production.
- Provide instrumented base libraries or sidecars to lower the integration burden for domain teams.

## Summary

Comprehensive observability is the operational backbone of the enterprise agent platform. By combining standard metrics, logs, and distributed traces with agent-specific reasoning traces, the platform provides the visibility needed to operate agents reliably, debug failures efficiently, attribute costs accurately, and satisfy compliance requirements at scale.
