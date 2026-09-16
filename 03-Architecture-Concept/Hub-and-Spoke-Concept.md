---
layout: default
title: Hub and Spoke Concept
parent: Architecture Concept
nav_order: 9
---

# Hub and spoke concept

Hub and spoke is the topology that lets the enterprise hold consistency and autonomy at
the same time. The hub owns what must be identical everywhere; spokes own what must be
different to deliver value quickly.

![Hub and spoke topology]({{ site.baseurl }}/assets/diagrams/hub-and-spoke.svg)
*A shared platform hub surrounded by domain spokes.*

## What lives in the hub

| Hub component | Why it belongs there |
| --- | --- |
| Model gateway | Consistent metering, quota, routing and safety for all traffic |
| Control plane (policy, registry, lifecycle) | Governance must not be bypassable |
| Shared identity services | One trust model across all agents and tools |
| Central observability and FinOps | Enterprise-wide correlation and attribution |
| Shared network services and egress control | Consistent segmentation and inspection |
| Templates, golden paths and marketplace | Reuse and paved-road delivery |

## What lives in a spoke

| Spoke component | Why it belongs there |
| --- | --- |
| Agent runtimes and workloads | Scenario-specific scaling and release cadence |
| Domain data products and indexes | Ownership sits with the data owner |
| Domain-specific tools and MCP servers | Business logic changes at domain speed |
| Scenario configuration and prompts | Iterated frequently by the domain team |
| Domain dashboards and budgets | Accountability follows ownership |

## Boundaries

The relationship between hub and spoke is a contract, not a hierarchy:

- Spokes consume hub services through published interfaces and quotas.
- Hub services never require knowledge of spoke internals.
- Spokes cannot weaken hub-enforced controls, only add stricter ones.
- Telemetry flows from spoke to hub in a defined schema.
- Hub changes follow a published deprecation policy so spokes are not broken by surprise.

{: .note }
> The most common design error is putting scenario logic in the hub "temporarily". It
> makes the hub team a delivery dependency for every change and is the fastest route to
> the bottleneck the platform was meant to remove.

## Relationship to the operating models

Hub and spoke is a topology; centralised and federated are operating models. Both
operating models use the same topology, but assign the spokes differently.

| Aspect | [Centralised]({{ site.baseurl }}/03a-Centralized-Operating-Model/) | [Federated]({{ site.baseurl }}/03b-Federated-Operating-Model/) |
| --- | --- | --- |
| Who builds in the spoke | Central platform team | Domain team |
| Who operates the spoke | Central platform team | Domain team |
| Who owns the spoke budget | Central | Domain |
| Hub responsibility | Build, run and govern | Provide paved road and guardrails |
| Change speed limit | Central team capacity | Domain team capability maturity |

Because the topology does not change, moving from centralised to federated is primarily
an ownership and automation change rather than a re-architecture. Designing for that
transition from the start is one of the highest-value decisions in the concept.
