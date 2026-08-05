---
layout: default
title: Governance Model
parent: Federated Operating Model
nav_order: 3
---

# Governance Model

Governance in the federated operating model solves a harder problem than in the [centralized model](../03a-Centralized-Operating-Model/Governance-Model.md): it must ensure consistent, auditable, enterprise-grade controls across multiple autonomous domain teams without creating a bottleneck that negates the speed benefits of federation. The solution is governance as infrastructure — automated, policy-as-code, runtime-enforced controls that domain teams cannot bypass, combined with a lightweight escalation path for genuine edge cases.

---

## Governance Philosophy

{: .highlight }
> In the federated model, governance is not a team — it is a system. The central governance team authors and maintains the system; domain teams comply with it automatically through the tools and pipelines they use daily.

The federated governance model distinguishes between two types of governance obligation:

- **Non-negotiable controls** — Security policies, identity requirements, data classification rules, content filtering, mandatory telemetry. These are enforced automatically and cannot be disabled by domain teams.
- **Configurable guardrails** — Quality thresholds, retention policies, rate limits, approved model tiers. These have platform-defined defaults that domain teams can adjust within permitted ranges via self-service.

This distinction gives domain teams meaningful autonomy over their agent design and operational preferences, while ensuring that the enterprise's risk appetite is never compromised.

---

## Governance Responsibilities

| Responsibility | Central Platform Team | Domain Team |
|---|---|---|
| Policy authoring | Owns all enterprise-level policies | Contributes domain-specific policy proposals; platform team reviews and ratifies |
| Policy enforcement | Operates and maintains enforcement tooling (pipeline gates, OPA, runtime sidecars) | Consumes enforcement — cannot disable or override |
| Compliance mapping | Maintains enterprise regulatory mapping | Identifies domain-specific regulatory requirements for review by central team |
| Lifecycle governance | Owns the deployment pipeline and automated gate logic | Submits agent configurations; pipeline enforces gates automatically |
| AI-specific governance | Maintains model version list, content filter policies, evaluation harness | Authors domain evaluation suites; reports anomalies and incidents |
| Exception management | Reviews and approves exceptions; maintains exception register | Submits exception requests with business justification |
| Audit and reporting | Produces enterprise-level governance reports | Produces domain-level reports; attends enterprise governance reviews |

---

## Policy Architecture

### Policy Layers

The federated governance model implements policy at four layers:

| Layer | Scope | Examples | Enforced by |
|---|---|---|---|
| **Enterprise mandatory** | All agents, all domains, non-negotiable | Data classification enforcement, identity requirements, mandatory telemetry, prohibited model types | Azure Policy, OPA, CI/CD gate — cannot be overridden |
| **Platform configurable** | All domains; defaults set centrally; domain teams may adjust within range | Quality thresholds (min 70%, configurable to 80–95%), rate limits (default 100 RPM, domain may request up to 500 RPM), retention periods | Platform configuration service — domain teams configure via portal |
| **Domain-specific** | Within a single domain; must not conflict with enterprise or platform layer | Domain-specific data source approvals, domain-specific human-in-the-loop triggers, domain-level user permission groups | Domain team configuration, validated by CI/CD gate against enterprise layer |
| **Agent-specific** | Within a single agent deployment | System prompt versioning, specific tool binding approvals, agent-level rate limits | Agent configuration in registry, validated at deployment |

### Policy-as-Code Pipeline

All enterprise and platform policies are maintained as code:

```
Policy repository (central team owns)
  → Pull request review (central governance team)
  → Automated policy testing (OPA unit tests, integration tests in staging)
  → Deployment to policy enforcement infrastructure
  → Notification to all domain teams (breaking changes require 30-day notice)
  → Domain team acknowledgement (for breaking changes)
```

Domain teams receive automated notifications when policies change. Non-breaking changes (e.g., tightened quality thresholds) take effect on the next deployment. Breaking changes (e.g., a previously approved model version being deprecated) require a 30-day notice period with a migration path.

---

## Lifecycle Governance

Domain teams own the lifecycle of their agents, but the platform's automated gates ensure consistent quality and safety standards:

### Automated Lifecycle Gates

| Gate | Trigger | Automated Check | Blocking? |
|---|---|---|---|
| Secrets scan | Every PR | No secrets in configuration or code | Yes |
| Policy compliance | Every PR | Tags, approved base images, required metadata | Yes |
| Security scan | Every PR | Dependency vulnerabilities, SAST | Yes (critical findings) |
| Evaluation gate | Every deployment to staging | Quality ≥ domain threshold, safety pass, latency ≤ SLA | Yes |
| Registry validation | Pre-production promotion | Agent metadata complete, tool bindings registered, runbook linked | Yes |
| Observability check | Post-production deployment | Traces flowing, dashboards live, alert policies active | Yes (within 15 min of deployment) |

### Domain Team Lifecycle Rights

Domain teams with validated platform capability (assessed during onboarding) have the following autonomous rights:

- **Deploy to staging** — Without central team approval, as long as all automated gates pass.
- **Promote to production** — Without central team approval for standard agent patterns; human-in-the-loop review required for novel patterns or high-risk classifications.
- **Deprecate and retire** — With notification to registered consumers (enforced by registry).
- **Rollback** — Instant rollback to previous version without central team approval, logged automatically.

---

## AI-Specific Governance in the Federated Model

### Model Governance

The central team maintains the approved model catalogue. Domain teams select from this catalogue when configuring their agents. Adding a new model to the catalogue requires:

1. Model evaluation by the central AI/ML team (quality, safety, performance, cost).
2. Security review (data handling, API terms, vendor risk assessment).
3. Governance approval (compliance, regulatory fit).
4. Publication to the catalogue with usage guidance and any restrictions.

Domain teams may request model additions via the standard exception/request process. Evaluated models are typically added within 30 days of request.

### Prompt Governance

- System prompts are stored in the agent's configuration repository, subject to the same version control and review process as code.
- Domain teams own their prompt repositories but must not remove required platform-injected prompt elements (safety instructions, citation requirements, output format constraints).
- The platform SDK injects non-overridable system context before the domain team's system prompt is applied.

### Cross-Domain Agent Calls

Cross-domain agent invocations introduce governance complexity. The federated model addresses this with:

- **Registry-mediated invocation** — Agents can only call other agents that are registered and have public accessibility set in the registry.
- **Consumer approval** — The providing domain team must approve consumers of their agent in the registry.
- **Scoped delegation** — Cross-domain calls carry scoped delegation tokens that limit the called agent's data access to what is needed for the specific task.
- **Bilateral audit** — Both the calling and called agent log the cross-domain invocation event with the full delegation token metadata.

---

## Exception Management

The exception process in the federated model is streamlined to avoid creating a bottleneck:

| Exception Type | Response SLA | Approval Authority |
|---|---|---|
| Standard model tier upgrade request | 5 business days | Platform AI/ML lead (self-service form) |
| Non-standard data source approval | 10 business days | Security & compliance team |
| Policy waiver (time-limited, compensating controls) | 15 business days | Central governance committee |
| Novel architecture pattern (not in golden path) | 20 business days | Platform engineering review board |

Exceptions are tracked in a shared register visible to all domain governance leads. Patterns of repeated exceptions for the same scenario are used to prioritise golden path extensions.

---

## Governance Maturity for Federated Operation

A domain team must achieve minimum governance maturity before receiving autonomous deployment rights:

| Criterion | Assessment Method |
|---|---|
| Passed platform onboarding certification | Training completion record |
| At least one platform-capable engineer on the team | Skills assessment |
| Domain AI risk register in place | Central team review |
| Agreed domain cost budget and quota | FinOps sign-off |
| Incident response runbook for first agent | Platform SRE review |
| Understanding of exception process | Onboarding checklist |

Domain teams that do not yet meet these criteria operate under **supervised federation**: they can build and test agents in staging but require central team review for each production promotion until criteria are met.
