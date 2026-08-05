---
layout: default
title: Capability Maturity Model
parent: Enterprise Capabilities
nav_order: 9
---

# Capability maturity model

Maturity levels give the capability model a scale, so that a baseline assessment produces
a decision rather than a list. The goal is never "level 5 everywhere" — it is a
deliberate target level per capability, justified by the scenarios in scope.

## Levels

| Level | Name | Description |
| --- | --- | --- |
| 1 | Ad hoc | Handled per project, if at all. No shared definition or owner. |
| 2 | Defined | Documented approach and named owner. Applied manually and inconsistently. |
| 3 | Standardised | A single supported approach used by all new work, with templates. |
| 4 | Automated | Enforced by policy, pipelines and platform services; evidence is generated. |
| 5 | Optimised | Measured, continuously improved, and adapted from production telemetry. |

Level 4 is the point at which a capability stops depending on individual diligence. For
guardrail capabilities this is the minimum acceptable target; for others, level 3 is
often sufficient.

## Suggested target levels

| Capability area | Target before first production agent | Target at scale |
| --- | --- | --- |
| Enterprise Platform Foundations (1-6) | 3-4 | 4 |
| Governance & Security (7-10) | 4 | 4-5 |
| Runtime & Experience (11-14) | 3 | 4 |
| Intelligence (15-18) | 3-4 | 4-5 |
| Interoperability (19-21) | 2 | 4 |
| Operations (22-24) | 3 | 4-5 |

{: .highlight }
> Interoperability can safely start low. A registry and marketplace only pay back once
> there is enough built to reuse — investing there before Phase 3 usually produces empty
> catalogues.

## Assessment method

1. Score each of the 24 capabilities against the levels, with evidence for the score.
2. Record the accountable owner and the realising services.
3. Compare with the target level for the current roadmap phase.
4. Convert each gap into a backlog item with an owner and a phase.
5. Re-assess at every phase boundary and after any significant incident.

Scores must be evidence-based. "We have a policy document" is level 2; "the pipeline
fails when the policy is violated" is level 4.

## Maturity and the operating model

Federation depends on maturity. Delegating delivery to domain teams is safe only when the
guardrail capabilities are automated rather than review-based.

| Signal | Implication |
| --- | --- |
| Governance & Security below level 4 | Stay centralised; automate guardrails first |
| Operations below level 3 | Federation will produce invisible incidents and cost |
| Interoperability below level 3 with many domains | Duplication is already occurring |
| All areas at level 4 with central backlog growing | Federate now |

Use this table at the Phase 1 and Phase 3 gates described in the
[implementation roadmap]({{ site.baseurl }}/07-Implementation-Roadmap/).
