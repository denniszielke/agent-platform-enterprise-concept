---
layout: default
title: Observability Model
parent: Federated Operating Model
nav_order: 5
---

# Observability Model

Observability in the federated operating model serves two distinct audiences simultaneously: the **central platform team**, which needs enterprise-wide visibility to operate shared services, detect systemic issues, and produce compliance artefacts; and **domain teams**, which need deep, contextual insight into their own agents to debug problems, optimise performance, and manage their SLAs. The federated observability model provides both, using a layered telemetry architecture that aggregates domain signals into a central plane while preserving domain-scoped isolation and autonomy.

---

## Observability Architecture

The federated observability model builds on the [centralized observability foundations](../03a-Centralized-Operating-Model/Observability-Model.md) with an additional domain-local tier:

```
Domain Agent Workloads (OpenTelemetry SDK — mandatory, via platform SDK)
  → Domain OpenTelemetry Collector (domain spoke — domain team operates)
    ├── Domain Log Analytics / Metrics Store (domain-scoped — domain team reads)
    └── Central Collector (aggregation, enrichment, cross-domain joins)
          ├── Central Log Analytics Workspace (platform team — full visibility)
          ├── Central Metrics Aggregation (cross-domain metrics, SLA monitoring)
          ├── AI Interaction Store (access-restricted, compliance retention)
          └── Compliance Audit Export (immutable, tamper-evident)
```

{: .highlight }
> Mandatory instrumentation via the platform SDK is the single most important guardrail in the federated observability model. Domain teams own their dashboards and can add custom signals — but they cannot remove or modify the platform-required signals that flow to the central collector. This non-negotiable floor ensures that the central team always has sufficient visibility to operate safely.

---

## Signal Tiers

### Tier 1 — Mandatory Platform Signals (Non-Negotiable)

These signals are emitted by the platform SDK automatically. Domain teams cannot suppress them.

| Signal | Description | Destination |
|---|---|---|
| `agent.invoke` trace span | Start and end of every agent invocation: session ID, agent ID, domain tag, user ID hash, outcome | Central + domain |
| `llm.chat_completion` span | Every model call: model ID, tokens in/out, latency, finish reason, content filter outcome | Central + domain |
| `tool.execute` span | Every tool invocation: tool server ID, domain, latency, success/failure | Central + domain |
| Identity events | Token issuance, RBAC checks, delegation events | Central only (security) |
| Policy violation events | Any policy gate trigger | Central only (compliance) |
| Cost metering events | Token consumption per agent per call, infrastructure cost tags | Central (FinOps aggregation) + domain |

### Tier 2 — Platform-Recommended Signals (Defaults On, Domain-Configurable)

These signals are enabled by default in the platform SDK. Domain teams may configure sampling rates, retention, and alerting thresholds.

| Signal | Default Behaviour | Domain Configuration Options |
|---|---|---|
| Retrieval spans | Logged at 100% in staging, 10% sampling in production | Domain can increase to 100% for debugging windows |
| Memory operation spans | Logged at 10% sampling | Domain can adjust sampling rate |
| Evaluation scores | Logged for each model version change | Domain can add additional evaluation triggers |
| Human-in-the-loop events | Always logged | No suppression permitted |

### Tier 3 — Domain Custom Signals

Domain teams may add custom telemetry using the platform's OpenTelemetry SDK extension points:

- **Custom spans** — Domain-specific workflow steps (e.g., business rule evaluation, domain system API calls).
- **Custom metrics** — Domain KPIs derived from agent outputs (e.g., invoice extraction accuracy, customer satisfaction score from structured agent output).
- **Custom logs** — Domain-specific structured log events for domain debugging purposes.

Custom signals flow into the domain-scoped collector and are visible in the domain's observability view. They are not forwarded to the central collector unless the domain team explicitly configures forwarding.

---

## Domain Observability Autonomy

Domain teams have full read access to the observability data their agents produce, via scoped views in the developer portal:

| Capability | Domain Team Access |
|---|---|
| Live trace explorer | Yes — domain-scoped; cannot see other domains' traces |
| Domain metrics dashboards | Yes — full self-service dashboard configuration |
| Domain alert management | Yes — can add, edit, and silence domain alerts within platform-defined severity floor |
| Historical log query | Yes — domain logs for configured retention period |
| Cross-domain trace | No — cross-domain trace joins visible only to central platform SRE during incidents |
| AI interaction store | No — access requires security team approval for specific incident investigation |

Domain teams are encouraged to maintain their own operational dashboards. The platform provides a **domain starter dashboard template** that pre-populates key agent health metrics, saving teams from building from scratch.

---

## Central Platform Observability

The central platform team maintains enterprise-wide observability capabilities that domain teams cannot access:

### Cross-Domain Visibility

- **Enterprise agent inventory** — All registered agents across all domains, with deployment status, version, and health.
- **Model gateway aggregate view** — Total token consumption, rejection rates, and latency across all domains.
- **Cross-domain call graph** — Visualisation of agent-to-agent invocations across domain boundaries.
- **Policy violation heatmap** — Which domains and agents are triggering policy gates most frequently.

### Incident Response

During incidents affecting domain agents, the central SRE team has elevated read access to domain telemetry streams for the duration of the incident. This access is:

- Logged and time-limited (default 4-hour window, renewable).
- Notified to the domain team lead in real time.
- Recorded in the incident audit trail.

This ensures that the central team can assist with cross-domain incidents and systemic issues without routinely browsing domain data.

---

## Observability Governance

### Telemetry Coverage SLA

Domain teams must maintain a minimum telemetry coverage level as a condition of autonomous deployment rights:

| Metric | Minimum Coverage | Measurement Method |
|---|---|---|
| Tier 1 trace coverage | 100% of production requests | Central collector validation |
| Agent health check response | 99.5% uptime | Central monitoring heartbeat |
| Cost metering completeness | 99% of token consumption tagged | FinOps reconciliation |

Teams falling below the Tier 1 coverage minimum are placed in supervised federation until coverage is restored.

### Quarterly Telemetry Review

The central platform SRE team conducts quarterly reviews of each domain's telemetry health:

- Coverage completeness against the mandatory signal set.
- Alert policy coverage (are there alerts for all production agents?).
- Observability-driven incident improvements (did the team act on signals?).
- Custom signal quality (are domain custom signals well-formed and useful?).

---

## Alerting Model

### Central Alerts (Platform Team Owned)

| Alert | Scope | Severity |
|---|---|---|
| Model gateway availability | All domains | Critical |
| Cross-domain latency spike | All domains | High |
| Enterprise token quota 80% | Aggregate | Warning |
| Policy violation surge | Any domain | High |
| Identity anomaly | Any domain | Critical |

### Domain Alerts (Domain Team Owned)

Each domain team configures and owns their agent-level alerts within the platform's alert framework:

- Minimum required alerts per agent: error rate, p95 latency, health check.
- Domain teams configure thresholds appropriate to their use case SLAs.
- Domain alert firing routes to the domain team's on-call schedule.
- Escalation to central SRE is triggered automatically if a domain alert fires and remains unacknowledged beyond the domain team's configured escalation window.

{: .note }
> The platform provides a **default alert template** for each new agent registration, pre-populated with sensible thresholds derived from the agent's first two weeks of staging traffic. Domain teams review and adjust before production promotion.

---

## Compliance Observability

Compliance telemetry flows through the central pipeline regardless of federation:

- All Tier 1 signals are retained in the central audit store for the regulatory retention period applicable to the domain's data classification.
- Cross-domain delegation chains are preserved as immutable audit records.
- Content filter decisions and prompt injection detections are retained in the security audit store with restricted access.
- Domain teams can request compliance reports for their agents from the central platform team via the developer portal.
