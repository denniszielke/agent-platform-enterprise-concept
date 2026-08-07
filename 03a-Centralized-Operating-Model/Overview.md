---
layout: default
title: Overview
parent: Centralized Operating Model
nav_order: 1
---

# Overview

The **Centralized Operating Model** places a single, dedicated platform team at the centre of all agent development and operations. That team builds the platform, defines the paved roads, and typically owns the full lifecycle — from provisioning AI runtimes and managing model access, through to deploying, monitoring, and retiring agents on behalf of every consuming business unit.

This model is a viable choice where consistent governance, direct accountability and concentrated operational ownership best fit the organisation's risk profile, delivery demand and available skills. It can serve as either an initial model or a durable operating model when the central team has sufficient capacity.

![Centralised operating model]({{ site.baseurl }}/assets/diagrams/centralized-operating-model.svg)

*Figure 1 — Centralised operating model: a single platform team owns build and run for all agents across the enterprise.*

---

## Core Principles

The centralized model rests on four guiding principles:

1. **Single plane of control** — All agent workloads run on a shared infrastructure stack managed by one team. Configuration drift is minimised and every change is traceable.
2. **Consistency over autonomy** — Policies, patterns, and toolchains are standardised. Business units consume the platform through well-defined interfaces rather than building their own stacks.
3. **Central accountability** — A single team owns SLAs, security posture, and cost. Escalation paths are unambiguous.
4. **Progressive capability release** — The platform team gates access to new capabilities (e.g., multi-agent orchestration, long-term memory, external tool integrations) until they meet enterprise readiness criteria.

---

## Capability Coverage

All 24 platform capabilities are owned and operated by the central team, organised across six domains:

| Domain | Capabilities | Central Team Responsibility |
|---|---|---|
| **Enterprise Platform Foundations** | Billing & commercial management, resource organisation, roles & access, network topology, platform management, resilience | Full ownership — provision, configure, operate |
| **Governance & Security** | Identity & trust, AI runtime protection, governance & compliance, lifecycle automation | Policy definition, enforcement tooling, audit |
| **Runtime & Experience** | AI runtimes, workflow orchestration, agent memory, user experience integration | Runtime hosting, version management, UX SDKs |
| **Intelligence** | Model gateway, semantic foundation, knowledge & data platform, evaluation engineering | Model onboarding, RAG pipelines, eval harness |
| **Interoperability** | Tool & MCP connectivity, agent & MCP registry, enterprise capability marketplace | Integration patterns, registry operations |
| **Operations** | Observability, AI FinOps, enterprise AI enablement operating model | Dashboards, cost allocation, enablement programmes |

---

## Organisational Structure

A typical centralized platform team comprises the following roles:

- **Platform Engineering** — Infrastructure, CI/CD pipelines, runtime management, and resilience engineering.
- **AI/ML Engineering** — Model gateway, evaluation engineering, semantic foundation, and knowledge platform.
- **Security & Compliance** — Identity and trust, AI runtime protection, governance policy authoring.
- **FinOps** — Cost allocation, showback/chargeback, quota management.
- **Developer Experience** — SDK maintenance, documentation, internal developer portal, enablement programmes.
- **Operations & SRE** — Observability pipelines, incident response, capacity planning.

Business units interact with this team through a formal intake process: they submit agent requirements, collaborate on design reviews, and receive deployments managed by the platform team.

---

## When This Model Fits

Use the table below to evaluate whether the centralized model is right for your organisation's current context.

| Decision Factor | Centralized | Federated |
|---|---|---|
| AI adoption maturity | Early (0–12 months) | Intermediate to advanced |
| Number of domain teams | Few (< 5) | Many (5+) |
| Regulatory environment | Highly regulated (FSI, healthcare, government) | Mixed regulation across domains |
| Engineering capability in domain teams | Limited | Strong, with dedicated platform engineers |
| Speed-to-first-agent priority | Secondary to governance | Primary driver |
| Toolchain standardisation preference | Single opinionated stack | Multiple stacks, governed by guardrails |
| Risk appetite for inconsistency | Very low | Moderate, managed via policy |
| Available central platform headcount | Small, focused team sufficient | Must scale guardrail automation as domain teams grow |

{: .highlight }
> The centralized model is a strong fit when the organisation needs concentrated control,
> operates in a tightly regulated context, or has enough central capacity to meet demand
> without transferring operational ownership to domain teams.

---

## Trade-offs

Every architectural choice involves trade-offs. The centralized model offers strong benefits but carries real risks that must be managed explicitly.

### Benefits

- **Governance by default** — Controls are embedded in the platform, not bolted on later. Audit logs, policy enforcement, and identity management are consistent across every agent.
- **Faster initial compliance** — Security and compliance reviews are centralised, reducing per-agent overhead for regulated workloads.
- **Economies of scale** — Shared infrastructure reduces total cost of ownership when agent volumes are low to moderate.
- **Reduced duplication** — A single team builds capabilities once; every business unit benefits without reinventing the wheel.
- **Clear escalation path** — Incident response and capacity decisions have one accountable owner.

### Risks and Mitigations

| Risk | Impact | Mitigation |
|---|---|---|
| **Delivery bottleneck** | Business units queue behind a single team, slowing time-to-market | Invest in self-service interfaces and automation; adopt a [federated model](../03b-Federated-Operating-Model/Overview.md) when demand outstrips capacity |
| **Platform team knowledge gap** | Central team may lack domain context, producing generic agents that miss business nuance | Embed domain liaisons; run structured requirements workshops |
| **Single point of failure** | Outages or team changes affect the entire enterprise | Resilience architecture, documented runbooks, and succession planning |
| **Innovation lag** | Centralised gating can slow adoption of new models or techniques | Maintain a fast-track sandbox environment; define clear promotion criteria |
| **Scalability ceiling** | Central teams have finite capacity; agent demand grows faster than headcount | Define a migration strategy to the federated model before hitting ceiling |

{: .warning }
> Without explicit capacity planning and automation investment, the centralized model can
> become a constraint on enterprise AI velocity. Review its fit at phase boundaries; move
> to the [federated model](../03b-Federated-Operating-Model/Overview.md) only when the
> evidence supports changing ownership.

---

## Relationship to the Federated Model

The centralized and federated models are two viable ownership patterns on the same architecture. The formal choice is made at the end of Phase 2 using evidence from production scenarios. An organisation can retain centralised ownership as a durable model or later redistribute ownership if demand, domain capability and guardrail maturity justify it. The model gateway, identity, observability and governance capabilities remain stable in either case.

See [Federated Operating Model — Overview](../03b-Federated-Operating-Model/Overview.md) for detail on the federated pattern and the decision factors that trigger a transition.
