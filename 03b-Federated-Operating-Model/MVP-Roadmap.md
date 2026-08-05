---
layout: default
title: MVP Roadmap
parent: Federated Operating Model
nav_order: 7
---

# MVP Roadmap

The MVP Roadmap for the federated operating model defines the phased journey from a mature centralized platform — where foundations and governance tooling are already proven — through to a fully federated state where multiple domain teams build and operate agents autonomously within enforced guardrails. Unlike the [centralized MVP roadmap](../03a-Centralized-Operating-Model/MVP-Roadmap.md), the federated roadmap assumes that the platform foundations (Phase 0–2 of the centralized model) are already in place or are being built in parallel. The federated roadmap is about extending proven foundations to domain teams safely.

{: .highlight }
> Federation is a maturity milestone, not a starting point. Before executing this roadmap, the organisation must have completed at least Phase 2 of the [centralized roadmap](../03a-Centralized-Operating-Model/MVP-Roadmap.md) — meaning a production-grade governance pipeline, identity model, observability pipeline, and model gateway are all operational.

---

## Roadmap Overview

| Phase | Name | Duration (estimate) | Primary Focus |
|---|---|---|---|
| 0 | Align | 4–6 weeks | Federation mandate, guardrail readiness, first domain selection |
| 1 | Golden Paths | 8–10 weeks | Paved roads, self-service tooling, domain onboarding framework |
| 2 | First Domain | 6–8 weeks | Pilot domain team deploys autonomously; guardrail validation |
| 3 | Scale Federation | 12–16 weeks | Multi-domain rollout, cross-domain capabilities, FinOps maturity |
| 4 | Operate | Ongoing | Continuous guardrail improvement, community of practice, maturity |

---

## Phase 0 — Align

**Objective:** Establish organisational mandate for federation, assess guardrail readiness, select the pilot domain team, and define the governance framework for autonomous deployment rights.

### Entry Criteria
- Centralized platform Phase 2 exit criteria met (or equivalent foundations in place).
- Demand from multiple domain teams that cannot be served by the central team's current capacity.
- Executive sponsor endorsement of the federated model as the next operating model evolution.

### Deliverables

| Deliverable | Description |
|---|---|
| Federation mandate | Executive-endorsed decision to federate, including scope, timeline, success criteria, and accountability model |
| Guardrail readiness assessment | Audit of existing governance tooling: are policy gates automated? Is runtime enforcement in place? Is observability covering all mandatory signals? |
| Autonomous deployment rights framework | Documented criteria for a domain team to receive, maintain, and lose autonomous deployment rights |
| First domain selection | Pilot domain team selected based on capability maturity, motivation, and representative use case |
| Domain onboarding checklist | Draft checklist for domain team certification (identity inventory, FinOps sign-off, runbook, evaluation capability) |
| Transfer pricing model | Draft intra-platform transfer prices for shared services, reviewed with central and domain finance leads |

### Exit Criteria
- [ ] Federation mandate signed by executive sponsor and at least two domain leads.
- [ ] Guardrail readiness assessment completed — all critical gaps remediated or time-bound remediation plan agreed.
- [ ] Pilot domain team identified and their engineering lead confirmed as the domain AI practice lead.
- [ ] Autonomous deployment rights framework approved by central governance committee.

---

## Phase 1 — Golden Paths

**Objective:** Build and publish the self-service tooling, templates, and documentation that domain teams need to build and operate agents independently. Validate the tooling with the pilot domain team before wider rollout.

### Entry Criteria
- Phase 0 exit criteria met.
- Central platform engineering bandwidth confirmed for golden path development.
- Pilot domain team engineers available for feedback sessions.

### Deliverables

| Area | Deliverables |
|---|---|
| **Agent project template** | Scaffolded agent project (Python, TypeScript, .NET variants) with platform SDK pre-integrated, mandatory OpenTelemetry instrumentation, identity hooks, and evaluation harness integration |
| **GitOps pipeline template** | Reusable CI/CD pipeline configuration that domain teams fork; includes all policy gates, security scan, evaluation gate, and registry update |
| **Domain spoke IaC module** | Terraform/Bicep module that provisions a compliant domain spoke: network peering, Key Vault, managed identity scaffolding, resource groups, RBAC assignments |
| **Developer portal domain view** | Domain-scoped portal experience: agent catalogue, cost dashboard, identity inventory, exception request form, evaluation history |
| **Observability starter kit** | Pre-built domain dashboard template, default alert policies, telemetry coverage check automation |
| **Domain onboarding certification** | Formal certification process: training materials, self-assessment, central team sign-off checklist |
| **Evaluation harness domain extension** | Documentation and tooling for domain teams to author domain-specific evaluation datasets and benchmarks |

### Exit Criteria
- [ ] Pilot domain team completes the full onboarding certification using the draft materials.
- [ ] Pilot domain team successfully deploys a test agent to staging using only golden path tooling — without a platform team ticket.
- [ ] All mandatory telemetry signals confirmed flowing from the pilot domain's test agent to the central collector.
- [ ] Onboarding checklist validated as complete and accurate by the pilot domain team.
- [ ] Golden path library published in the developer portal.

---

## Phase 2 — First Domain

**Objective:** The pilot domain team deploys their first production agent(s) autonomously. The central team validates that guardrails work as designed and that the support model scales.

### Entry Criteria
- Phase 1 exit criteria met.
- Pilot domain team has completed onboarding certification.
- Pilot domain team has an approved budget, quota allocation, and agreed domain SLA.
- At least one production-ready use case approved and scoped.

### Deliverables

| Area | Deliverables |
|---|---|
| **First autonomous agent deployment** | Pilot domain team deploys 1–2 production agents through the full lifecycle gate without central team involvement — confirmed by deployment log audit |
| **Guardrail validation report** | Central governance team reviews all gate outcomes from the pilot deployments: did the gates fire correctly? Were any bypassed? Are there gaps? |
| **Domain FinOps baseline** | Pilot domain's first month chargeback statement reconciled; unit economics measured and published to domain lead |
| **Cross-domain invocation pilot** | If applicable — pilot domain agent calls a central platform service or another registered agent; delegation token flow validated end-to-end |
| **Incident response drill** | Simulated production incident for the pilot domain's agent; domain team responds; escalation to central SRE tested |
| **Retrospective and lessons learned** | Joint retrospective between pilot domain and central platform team; golden path improvements identified and prioritised |

### Exit Criteria
- [ ] At least one pilot domain agent in production with domain team as accountable operator.
- [ ] Zero guardrail bypasses identified in the deployment audit.
- [ ] Domain team on-call rotation operational and tested via incident drill.
- [ ] All mandatory telemetry signals at 100% coverage in production.
- [ ] Chargeback activated for pilot domain with no unresolved allocation disputes.
- [ ] Guardrail gaps identified in the validation report assigned to central platform backlog with remediation timelines.

---

## Phase 3 — Scale Federation

**Objective:** Roll out federation to multiple domain teams. Mature cross-domain capabilities, the community of practice, and the FinOps discipline. Resolve guardrail gaps identified in Phase 2.

### Entry Criteria
- Phase 2 exit criteria met.
- Guardrail gaps from Phase 2 remediated.
- At least two additional domain teams have completed or are actively completing onboarding certification.
- Central platform team has automation in place to onboard a new domain in ≤ 3 weeks end-to-end.

### Deliverables

| Area | Deliverables |
|---|---|
| **Multi-domain rollout** | 3–5 additional domains onboarded using the validated golden path; each completes certification and deploys at least one production agent |
| **Cross-domain capability marketplace** | Domain teams contribute tools and MCP servers to the registry; at least 3 cross-domain capability consumption relationships established |
| **Community of practice launch** | Regular cadence established (bi-weekly or monthly); shared pattern library growing; at least 2 community-contributed golden path extensions merged |
| **Advanced orchestration patterns** | Multi-agent (cross-domain orchestration) pattern documented, governed, and demonstrated end-to-end by at least one domain team |
| **Federated FinOps maturity** | All federated domains in chargeback mode; transfer pricing reviewed and adjusted based on 3 months of actuals; cross-domain unit economics benchmark published |
| **Governance maturity uplift** | Governance maturity at Level 4 (continuous compliance) as defined in the centralized [Governance Model](../03a-Centralized-Operating-Model/Governance-Model.md); automated drift detection operational |
| **Supervised federation exit** | Any domain teams previously in supervised federation assessed and either granted full autonomous rights or provided a clear remediation path |

### Exit Criteria
- [ ] 4+ domains in full autonomous federation with production agents.
- [ ] Community of practice active with at least 5 domain teams contributing.
- [ ] Cross-domain capability marketplace with at least 10 registered tools/agents discoverable enterprise-wide.
- [ ] Mean time to onboard a new domain ≤ 3 weeks end-to-end.
- [ ] All federated domains in chargeback mode with monthly reconciliation.
- [ ] Guardrail bypass rate (measured in deployment audit) < 0.5% of all deployment events.
- [ ] No critical governance findings open from the most recent quarterly review.

---

## Phase 4 — Operate

**Objective:** Sustain, improve, and evolve the federated platform continuously. This phase is the permanent steady state for a mature federated enterprise AI platform.

### Ongoing Central Team Activities

| Activity | Cadence | Owner |
|---|---|---|
| Guardrail review and improvement | Quarterly (+ post-incident) | Governance & platform engineering |
| Golden path library updates | Continuous via community PRs | DevEx + community of practice |
| Model catalogue review (new models, deprecations) | Quarterly + as needed | Platform AI/ML team |
| Transfer pricing review | Quarterly | Platform FinOps |
| Domain capability maturity assessment | Annual per domain | Platform team |
| Supervised federation reviews (new or regressed domains) | Monthly | Platform governance |
| Community of practice facilitation | Regular cadence | Platform DevEx |
| Enterprise AI risk register review | Quarterly | Security & compliance |

### Ongoing Domain Team Activities

| Activity | Cadence | Owner |
|---|---|---|
| Agent operational review (quality, cost, SLA) | Monthly | Domain AI practice lead |
| Domain FinOps review | Monthly | Domain lead + FinOps contact |
| Domain AI risk register update | Quarterly | Domain AI practice lead |
| Evaluation harness refresh | Quarterly | Domain AI engineering |
| Community of practice contribution | Regular cadence | Domain engineers |
| Exception register review | Quarterly | Domain lead |

### Maturity Targets (12 months post-Phase 3)

- 8+ domains in full autonomous federation.
- Mean time to deploy a new agent (from committed configuration to production): ≤ 2 business days for standard patterns.
- Guardrail bypass rate < 0.1% of all deployment events.
- 90% of domain teams self-sufficient for evaluation harness maintenance.
- Community of practice golden path library covering 10+ canonical agent patterns.
- Enterprise AI FinOps maturity at Level 4: predictive cost modelling, automated optimisation recommendations, domain budget forecasting accuracy within 10%.

---

## Relationship to the Centralized Roadmap

The federated roadmap is a continuation of the [centralized MVP roadmap](../03a-Centralized-Operating-Model/MVP-Roadmap.md), not a replacement. Organisations that have completed the centralized model's Phase 3 will find that many of the federation enablers — governance tooling, model gateway, identity model, observability pipeline — are already built. The federated roadmap invests in the **distribution** of those capabilities: self-service tooling, golden paths, domain onboarding, and the community of practice that makes distributed ownership sustainable.
