---
layout: default
title: Success Metrics
parent: Implementation Roadmap
nav_order: 4
---

# Success metrics

Metrics should answer three questions: is the platform being used, is it safe, and is it
worth the money. Vanity metrics — number of pilots, number of capabilities delivered —
answer none of them.

## Adoption

| Metric | Why it matters |
| --- | --- |
| Onboarding time for a new domain team | The paved road's real speed |
| Share of production agents built on golden paths | Whether the governed route is the chosen route |
| Number of domains with an agent in production | Breadth of platform value |
| Components consumed outside their owning domain | Whether reuse is actually happening |
| Deviation requests per quarter | Where the paved road does not fit |

## Delivery

| Metric | Why it matters |
| --- | --- |
| Lead time from approved idea to production agent | The headline objective |
| Change failure rate for agent releases | Whether speed is costing stability |
| Time to restore after an agent incident | Operational readiness |
| Share of releases passing evaluation gates first time | Quality engineering maturity |

## Quality and safety

| Metric | Why it matters |
| --- | --- |
| Task success rate per scenario | Whether agents do their job |
| Groundedness and citation coverage | Whether answers are traceable |
| Escalation to human rate | Whether the agent is trusted in context |
| Safety filter trigger rate and false-positive rate | Whether controls are calibrated |
| Share of agents with current evaluation suites | Whether change is safe |

## Economics

| Metric | Why it matters |
| --- | --- |
| Cost per completed task or conversation | The core unit economic |
| Trend of unit cost as volume grows | Whether the scenario scales |
| Attribution accuracy (share of spend mapped to a scenario) | Whether cost reporting is trustworthy |
| Budget variance per domain | Whether controls are working |
| Value measure per scenario (time saved, revenue influenced) | Whether spend is justified |

{: .highlight }
> Pair every cost metric with a value metric. Cost alone always argues for doing less.

## Platform health

Track capability maturity scores per area, evidence coverage for governance, the count of
registered but unowned agents, and the age of the oldest unreviewed access assignment.
These are leading indicators: they deteriorate before incidents happen.

## Cadence

| Cadence | Audience | Content |
| --- | --- | --- |
| Weekly | Platform team | Delivery, incidents, quality regressions |
| Monthly | Domain leads | Adoption, cost, quality per scenario |
| Quarterly | Sponsor and governance forum | Objectives, unit economics, capability maturity, risks |
| Per phase gate | All | Gate criteria from the [phased plan](Phased-Plan.md) |
