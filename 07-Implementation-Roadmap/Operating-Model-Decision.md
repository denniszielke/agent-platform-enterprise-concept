---
layout: default
title: Operating Model Decision
parent: Implementation Roadmap
nav_order: 3
---

# Operating model decision

The [centralised]({{ site.baseurl }}/03a-Centralized-Operating-Model/) and
[federated]({{ site.baseurl }}/03b-Federated-Operating-Model/) models are both viable
outcomes built on the same architecture. The decision determines who builds and operates
agents; it does not change the 24 capabilities the platform must provide.

## When to decide

Make the formal decision at the **end of Phase 2**. Phase 1 establishes the foundations;
Phase 2 tests them through production scenarios and supplies evidence about demand, risk,
central capacity, domain capability, cost and control maturity. Deciding earlier relies on
assumptions, while delaying the choice into Phase 3 leaves scaling ownership unclear.

The selected model can be reviewed at later phase boundaries as conditions change, but
neither option is provisional. The [hub and spoke concept]({{ site.baseurl }}/03-Architecture-Concept/Hub-and-Spoke-Concept.html)
keeps a later ownership change from becoming a re-architecture.

## Decision criteria

| Criterion | Favours centralised | Favours federated |
| --- | --- | --- |
| Number of active domains | Few | Many |
| Domain engineering capability | Limited | Strong, with platform experience |
| Regulatory intensity | Very high, uniform | High but domain-specific |
| Guardrail automation maturity | Below level 4 | Level 4 or above |
| Observability maturity | Below level 3 | Level 3 or above |
| Demand vs central capacity | Demand fits capacity | Backlog is growing |
| Scenario diversity | Homogeneous | Heterogeneous |
| Speed expectation | Consistency valued over speed | Domain speed is the priority |

Score each criterion honestly. A single strong constraint — for example insufficient
central delivery capacity or guardrails that still depend on human review — can outweigh
the overall balance.

## Readiness gates

### Centralised model

Choose the centralised model only when all of the following are true:

1. The central team has sustained funding and enough engineering capacity for forecast demand.
2. Intake, prioritisation and service-level expectations are explicit and measurable.
3. Domain experts can participate without transferring operational ownership.
4. Central ownership meets the required delivery speed and risk profile.
5. Cost and usage can still be attributed to consuming scenarios and domains.

### Federated model

Choose the federated model only when all of the following are true:

1. Governance and Security capabilities (7-10) are at maturity level 4.
2. Observability (22) provides correlated traces across all domains centrally.
3. Golden paths exist and are demonstrably faster than building from scratch.
4. Policy is enforced by pipelines and the gateway, not by review meetings.
5. Cost attribution is automatic and accurate enough to charge back.
6. At least one domain team has operated an agent in production successfully.

{: .warning }
> Federating with manual guardrails does not distribute delivery — it distributes risk
> while keeping the central team accountable for it.

## Recording the decision

Record the decision as an architecture decision record containing the date, the criteria
scores, the selected model or explicit hybrid boundary, the accountable owners, any
compensating controls, and the review date. The ownership column of the
[capability to architecture mapping]({{ site.baseurl }}/03-Architecture-Concept/Capability-to-Architecture-Mapping.html)
is updated at the same time — that table is the operational expression of the decision.

## Hybrid outcomes

A hybrid outcome is legitimate and common: high-risk scenarios remain centrally built and
operated, while lower-risk domains deliver on the paved road. The requirement is that the
split is defined by an explicit rule — usually the risk tier — rather than by which team
argued most persuasively.
