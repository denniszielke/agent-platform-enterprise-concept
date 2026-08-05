---
layout: default
title: Operating Model Decision
parent: Implementation Roadmap
nav_order: 3
---

# Operating model decision

Choosing between the [centralised]({{ site.baseurl }}/03a-Centralized-Operating-Model/)
and [federated]({{ site.baseurl }}/03b-Federated-Operating-Model/) model is the most
consequential architecture choice in the concept — and the one most often made
implicitly. This page makes it explicit.

## When to decide

The decision is normally taken at the **end of Phase 1** and revisited during **Phase 3**.
Deciding earlier means deciding on assumptions; deciding later means the platform has
already drifted into whichever model its automation happened to support.

Many enterprises deliberately start centralised to reach a compliant production scenario
quickly, then federate as guardrail automation and domain capability mature. Designing
for that transition — see the [hub and spoke concept]({{ site.baseurl }}/03-Architecture-Concept/Hub-and-Spoke-Concept.html)
— keeps the move an ownership change rather than a re-architecture.

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

Score each criterion honestly. A single strong signal — for example guardrails that still
depend on human review — should override a general preference for federation.

## Readiness gate for federation

Do not federate until all of the following are true:

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
scores, the chosen model, the capabilities whose ownership changes, the compensating
controls introduced, and the review date. The ownership column of the
[capability to architecture mapping]({{ site.baseurl }}/03-Architecture-Concept/Capability-to-Architecture-Mapping.html)
is updated at the same time — that table is the operational expression of the decision.

## Hybrid outcomes

A hybrid outcome is legitimate and common: high-risk scenarios remain centrally built and
operated, while lower-risk domains deliver on the paved road. The requirement is that the
split is defined by an explicit rule — usually the risk tier — rather than by which team
argued most persuasively.
