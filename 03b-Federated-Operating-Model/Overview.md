---
layout: default
title: Overview
parent: Federated Operating Model
nav_order: 1
---

# Overview

The **Federated Operating Model** distributes the building and operating of agents across multiple domain teams, while a central platform team retains ownership of the foundational capabilities, guardrails, and golden paths that keep the enterprise safe and consistent. Domain teams are empowered to move at their own velocity — designing, deploying, and evolving agents that reflect deep domain knowledge — within the boundaries defined and enforced by the central platform.

The federated model represents an evolution beyond the [centralized model](../03a-Centralized-Operating-Model/Overview.md). It is not a replacement; it is what becomes possible once an organisation has established robust governance tooling, validated platform capabilities, and matured domain engineering teams to the point where they can operate AI workloads responsibly.

![Federated operating model]({{ site.baseurl }}/assets/diagrams/federated-operating-model.svg)

*Figure 1 — Federated operating model: the central platform team owns the paved road and guardrails; domain teams build and operate their own agents on top.*

---

## Core Principles

The federated model rests on four guiding principles:

1. **Paved roads, not walls** — The central platform team provides golden paths: opinionated, well-documented, tooled patterns that make doing the right thing easier than doing the wrong thing. Domain teams are not locked in, but non-standard choices require deliberate effort.
2. **Guardrails over gatekeepers** — Policy is enforced by automated tooling at deployment time and runtime, not by approval queues. Domain teams can deploy autonomously as long as they comply with guardrails.
3. **Distributed ownership, shared accountability** — Domain teams own their agents' design, delivery, and operation. The central team owns the platform, the shared services, and the governance framework. Both parties are accountable for enterprise AI outcomes.
4. **Capability maturity prerequisite** — Domain teams must demonstrate a minimum level of platform engineering capability before receiving autonomous deployment rights. The central team provides enablement to accelerate maturity.

---

## Capability Coverage

In the federated model, responsibility for the 24 capabilities is split between the central platform team and domain teams:

| Domain | Capabilities | Central Team Responsibility | Domain Team Responsibility |
|---|---|---|---|
| **Enterprise Platform Foundations** | Billing & commercial, resource org, roles & access, network, platform mgmt, resilience | Shared infra, guardrails, billing aggregation | Resource provisioning within guardrails, domain-level RBAC |
| **Governance & Security** | Identity & trust, AI runtime protection, governance & compliance, lifecycle automation | Policy authoring, enforcement tooling, audit | Compliance with policies, exception requests |
| **Runtime & Experience** | AI runtimes, workflow orchestration, agent memory, UX integration | Runtime platform, approved runtime catalogue | Agent design, orchestration logic, UX integration |
| **Intelligence** | Model gateway, semantic foundation, knowledge platform, evaluation | Model gateway operation, shared embedding infra | Domain-specific indexes, evaluation suites, prompt engineering |
| **Interoperability** | Tool & MCP connectivity, agent & MCP registry, marketplace | Registry operation, integration patterns | Tool development and registration, domain marketplace entries |
| **Operations** | Observability, AI FinOps, enablement operating model | Central observability plane, cost aggregation, enablement programme | Domain dashboards, domain cost management, local enablement |

---

## Organisational Structure

The federated model requires an organisational structure that balances central and distributed roles:

### Central Platform Team

- **Platform Engineering** — Shared infrastructure, IaC, network, resilience, CI/CD pipelines.
- **Governance & Policy** — Policy authoring, enforcement tooling, regulatory mapping, exception management.
- **Platform AI/ML** — Model gateway, shared evaluation infrastructure, embedding platform.
- **Developer Experience** — Golden paths, SDK maintenance, internal developer portal, enablement programme.
- **Platform FinOps** — Aggregated cost reporting, tagging standards, chargeback framework.
- **Platform SRE** — Shared service SLAs, incident response for platform components.

### Domain Teams

Each participating domain team operates a small **domain AI practice** (2–5 engineers) that:
- Designs and builds agents for their business domain.
- Operates deployed agents, including on-call for domain-specific incidents.
- Contributes domain-specific tools and data connectors to the platform registry.
- Manages their domain's cost budget and observability views.
- Maintains a domain-level AI risk register in coordination with the central governance team.

### Community of Practice

A cross-cutting **Enterprise AI Community of Practice** connects central and domain team engineers:
- Regular knowledge sharing on new capabilities, patterns, and pitfalls.
- Coordinated contribution to the shared golden path library.
- Joint red-team exercises and evaluation benchmark reviews.

---

## When to Choose This Model

| Decision Factor | Centralized | Federated |
|---|---|---|
| AI adoption maturity | Early (0–12 months) | Intermediate to advanced (12+ months with validated foundations) |
| Number of domain teams building agents | Few (< 5) | Many (5+) with distinct delivery roadmaps |
| Domain team engineering capability | Limited | At least one platform-capable engineer per domain |
| Speed-to-production priority | Secondary | High — bottleneck avoidance is a primary driver |
| Regulatory environment | Highly regulated, uniform | Mixed; domain-specific regulations can be handled by each domain |
| Governance tooling maturity | Not yet automated | Policy-as-code, automated gates, runtime enforcement in place |
| Central platform headcount available | Small team can handle demand | Central team cannot scale to meet all domain demand centrally |
| Organisational culture | Central control preferred | Domain ownership and autonomy valued |

{: .highlight }
> The federated model pays dividends when domain teams have enough AI platform maturity to operate responsibly, and when the central team's delivery capacity has become a bottleneck to enterprise AI velocity. If neither condition is yet true, begin with the [centralized model](../03a-Centralized-Operating-Model/Overview.md).

---

## Trade-offs

### Benefits

- **Domain velocity** — Teams deploy without central approval queues. Time-to-production for new agents drops significantly.
- **Domain expertise encoded in agents** — Domain teams understand their data, workflows, and users far better than a central team can. Federated agents reflect this knowledge.
- **Parallel delivery** — Multiple domains progress simultaneously, multiplying the enterprise's rate of AI-driven value creation.
- **Resilience through diversity** — No single central team failure disrupts all AI delivery. Domain teams have operational independence.
- **Talent development** — Domain teams build deep AI platform capability, creating a distributed pool of enterprise AI engineering skill.

### Risks and Mitigations

| Risk | Impact | Mitigation |
|---|---|---|
| **Guardrail bypass** | Domain teams circumvent governance controls, introducing risk | Automated, non-bypassable policy gates in the deployment pipeline; runtime enforcement |
| **Inconsistent quality** | Agent quality varies widely across domains | Central evaluation harness available to all domains; community of practice shares benchmarks |
| **Observability gaps** | Domain-operated agents produce inconsistent or incomplete telemetry | Mandatory OpenTelemetry instrumentation via SDK; platform SRE audits telemetry coverage quarterly |
| **Cost overruns** | Domain teams without FinOps culture overspend | Domain-level quotas enforced at model gateway; monthly showback/chargeback with domain lead accountability |
| **Security incidents** | Domain-operated tool integrations introduce vulnerabilities | Central tool and MCP registry vetting; domain teams may not bypass registry for production workloads |
| **Knowledge fragmentation** | Solutions reinvented independently across domains | Community of practice, shared pattern library, golden path incentives |

{: .warning }
> Federation without mature guardrail automation is dangerous. Before allowing domain teams to deploy autonomously, the central team must have automated policy gates, runtime enforcement, and the observability coverage to detect problems quickly. Premature federation is riskier than a temporary central bottleneck.

---

## Relationship to the Centralized Model

The federated model extends rather than replaces the centralized foundations. The model gateway, identity infrastructure, governance tooling, and observability pipeline built under the [centralized model](../03a-Centralized-Operating-Model/Overview.md) become the shared services that federated domain teams depend on. The central platform team's role shifts from builder-and-operator of all agents to enabler, guardrail owner, and shared-service provider.

Most organisations transition from centralized to federated gradually — beginning by federating the most mature domain team, validating the governance mechanisms, and then progressively extending autonomous rights to additional domains as their capability matures.
