---
layout: default
title: MVP Roadmap
parent: Centralized Operating Model
nav_order: 7
---

# MVP Roadmap

The MVP Roadmap for the centralized operating model defines a phased sequence of investment, from initial organisational alignment through to a stable, fully operated platform serving multiple business domains. Each phase has specific entry criteria, a defined set of deliverables, and measurable exit criteria. Progress through the phases is gated — a phase must meet its exit criteria before the next phase begins, ensuring that foundations are solid before complexity is added.

{: .highlight }
> The centralized model's roadmap is deliberately front-loaded with governance and infrastructure work. This investment is what makes the platform trustworthy at scale and enables an eventual transition to the [federated model](../03b-Federated-Operating-Model/MVP-Roadmap.md) if demand outpaces central capacity.

---

## Roadmap Overview

| Phase | Name | Duration (estimate) | Primary Focus |
|---|---|---|---|
| 0 | Align | 4–6 weeks | Mandate, team, architecture decisions |
| 1 | Foundations | 8–12 weeks | Core infrastructure and governance |
| 2 | First Agents | 6–8 weeks | First production agents and observability |
| 3 | Scale | 12–16 weeks | Multi-domain, self-service, optimisation |
| 4 | Operate | Ongoing | Continuous improvement and maturity |

---

## Phase 0 — Align

**Objective:** Establish organisational mandate, assemble the platform team, and make key architecture decisions before writing any infrastructure code.

### Entry Criteria
- Executive sponsor identified and confirmed.
- Initial AI use cases from at least one business domain documented.
- Preliminary budget allocated for platform build and initial run costs.

### Deliverables

| Deliverable | Description |
|---|---|
| Platform charter | Mandate, scope, operating model choice (centralized), funding model, and success criteria signed off by sponsor |
| Team structure | Platform team roles filled or resourced plan in place (platform engineering, AI/ML, security, FinOps, DevEx) |
| Architecture decision records (ADRs) | Cloud provider, identity provider, model provider(s), IaC toolchain, observability stack, network topology decisions documented |
| Regulatory analysis | Data classification, applicable frameworks (ISO 27001, GDPR, sector-specific), and control mapping initiated |
| Use case backlog | At least 3 candidate agent use cases from domain teams, prioritised by value and technical risk |
| Tagging standard | Draft tagging taxonomy for cost allocation and governance approved by finance |

### Exit Criteria
- [ ] Platform charter signed by executive sponsor and at least one domain lead.
- [ ] All critical platform team roles filled (or contracted).
- [ ] ADRs approved for infrastructure, identity, model access, and observability.
- [ ] At least one use case approved and scoped for Phase 2 piloting.

---

## Phase 1 — Foundations

**Objective:** Deploy and validate the core infrastructure, governance, identity, and observability capabilities that every subsequent agent will depend on.

### Entry Criteria
- Phase 0 exit criteria met.
- Cloud subscription(s) and budget approved.
- IaC repository initialised with access controls and CI/CD pipeline.

### Deliverables

| Capability Area | Deliverables |
|---|---|
| **Platform Foundations** | Hub-spoke network topology deployed; resource hierarchy (management groups, subscriptions, resource groups) created; RBAC roles and assignments defined and applied |
| **Identity** | Managed identity strategy implemented; Key Vault deployed with secrets rotation policy; PIM activated for platform team roles; workload identity federation configured for target external services |
| **Governance** | Azure Policy baseline deployed (tagging enforcement, allowed regions, prohibited resource types); CI/CD pipeline with security scanning and policy gates live; governance wiki published |
| **Model Gateway** | Central model gateway (APIM + Azure OpenAI Service or equivalent) deployed with authentication, rate limiting, content filtering, and structured logging |
| **Observability** | OpenTelemetry collector cluster deployed; Log Analytics workspace and metrics store configured; baseline dashboards and alert policies live; AI interaction store provisioned (access-restricted) |
| **Cost Management** | Tagging enforced on all resources; cost budgets set at subscription and resource group levels; showback dashboard live |
| **Developer Experience** | Internal developer portal seeded with platform documentation, ADRs, and SDK onboarding guide |

### Exit Criteria
- [ ] All foundation infrastructure deployed via IaC and reviewed by security team.
- [ ] At least one test agent deployed end-to-end using platform SDK, producing traces in the observability pipeline.
- [ ] Identity model review completed — no static secrets in any platform component.
- [ ] Governance policy baseline enforced in the production subscription.
- [ ] Cost dashboard showing real spend against tag taxonomy.
- [ ] Penetration test or architecture review of foundation components completed; critical findings remediated.

---

## Phase 2 — First Agents

**Objective:** Deploy the first production agent(s) serving a real business use case, validate the end-to-end platform, and demonstrate measurable business value.

### Entry Criteria
- Phase 1 exit criteria met.
- At least one approved use case with a named domain lead and defined success metric.
- Domain team liaisons identified and briefed on the intake and deployment process.

### Deliverables

| Area | Deliverables |
|---|---|
| **First agent deployments** | 1–3 production agents deployed following the full lifecycle gate (intake → design review → security review → evaluation gate → staging → production) |
| **Evaluation harness** | Automated evaluation pipeline running quality, safety, and latency benchmarks on each model change or system prompt update |
| **Knowledge platform** | At least one governed data source ingested into the RAG pipeline with row-level security and lineage tracking |
| **Human-in-the-loop** | At least one agent with a configured human approval step for high-consequence actions |
| **Operational runbooks** | Incident response runbooks for all deployed agents; on-call rotation established |
| **Unit economics baseline** | Cost-per-conversation or cost-per-task measured for each deployed agent and published to stakeholders |
| **Lessons learned review** | Retrospective with domain teams; platform improvements prioritised for Phase 3 |

### Exit Criteria
- [ ] At least one agent in production serving real users with a defined SLA.
- [ ] Success metric for the use case measured and reported (e.g., task automation rate, user satisfaction score).
- [ ] Full observability trace confirmed for all production agent interactions.
- [ ] Unit economics for deployed agents published to domain leads.
- [ ] Zero critical security findings open from the Phase 2 security review.
- [ ] Intake process documented and confirmed as fit for purpose by at least one domain team.

---

## Phase 3 — Scale

**Objective:** Onboard multiple business domains, introduce self-service capabilities to reduce the delivery bottleneck, and optimise platform efficiency.

### Entry Criteria
- Phase 2 exit criteria met.
- Demand from at least two additional business domains documented.
- Platform team capacity confirmed (headcount or tooling automation) to support additional domains.

### Deliverables

| Area | Deliverables |
|---|---|
| **Multi-domain onboarding** | 2–4 additional domains onboarded; domain-specific views in the developer portal and observability dashboards |
| **Self-service agent configuration** | GitOps-based agent configuration flow where domain teams submit prompt, tool binding, and memory settings via pull request without requiring direct platform team involvement |
| **MCP tool registry** | At least 5 enterprise tool servers registered and discoverable in the capability marketplace |
| **Advanced orchestration** | Multi-agent (orchestrator/sub-agent) pattern validated and published as a reusable template |
| **Cost optimisation** | Model right-sizing, semantic caching, and batch processing implemented and measured; unit costs improved vs Phase 2 baseline |
| **Chargeback transition** | If showback has been in place for 6+ months, chargeback mode activated with finance system integration |
| **Transition readiness assessment** | Platform team conducts assessment of domain team capability maturity and documents criteria for [federated model transition](../03b-Federated-Operating-Model/MVP-Roadmap.md) |

### Exit Criteria
- [ ] 3+ domains served with production agents.
- [ ] Self-service agent configuration flow handling at least 50% of new agent deployments without a platform team ticket.
- [ ] Unit economics improved by at least 15% vs Phase 2 baseline through optimisation measures.
- [ ] Agent & MCP registry live with active usage.
- [ ] No SLA breaches across all production agents in the most recent calendar month.
- [ ] Federated model transition criteria documented and reviewed with leadership.

---

## Phase 4 — Operate

**Objective:** Sustain, improve, and govern the platform continuously. This phase has no defined end — it is the steady-state operating mode.

### Ongoing Activities

| Activity | Cadence | Owner |
|---|---|---|
| Platform capability roadmap review | Quarterly | Platform product lead |
| Governance controls and regulatory mapping review | Quarterly | Security & compliance |
| Unit economics and cost optimisation review | Monthly | FinOps |
| AI model version management (deprecations, upgrades) | As needed + quarterly planned | AI/ML engineering |
| Evaluation harness refresh (new benchmarks, red-team exercises) | Quarterly | AI/ML engineering |
| Developer experience and portal improvements | Continuous | DevEx team |
| Incident retrospectives | Post-incident | Platform SRE |
| Capacity planning review | Quarterly | Platform SRE |
| Federated model readiness review | Annual or when triggered by demand | Platform leadership |

### Maturity Targets (12 months post-Phase 3)

- Governance maturity at Level 3 (runtime enforcement) as defined in the [Governance Model](./Governance-Model.md).
- 90% of new agent deployments via self-service GitOps flow.
- Mean time to onboard a new domain: ≤ 4 weeks.
- All production agents with full distributed trace coverage.
- Chargeback active and reconciled monthly with zero disputed allocations exceeding 2% of total.

---

## Relationship to the Federated Roadmap

The centralized MVP roadmap delivers a complete operating model and also builds foundations that can support federation. The formal operating-model choice is made at the end of Phase 2 of the [enterprise phased plan](../07-Implementation-Roadmap/Phased-Plan.md), using evidence from production scenarios. If the centralised model is selected, its delivery capacity and services are scaled; if the [federated model](../03b-Federated-Operating-Model/MVP-Roadmap.md) is selected, the same guardrails, golden paths, governance tooling and observability pipelines support redistributed ownership. Neither choice requires a technical rewrite.
