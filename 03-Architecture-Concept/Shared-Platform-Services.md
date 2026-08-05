---
layout: default
title: Shared Platform Services
parent: Architecture Concept
nav_order: 2
---

# Shared platform services

Shared platform services are the concrete things a domain team consumes on day one. They
are the realisation of the [capability model]({{ site.baseurl }}/02-Enterprise-Capabilities/)
as an offering with an interface, an owner and a service level.

![Layered architecture]({{ site.baseurl }}/assets/diagrams/layered-architecture.svg)
*Shared services sit in the control plane and interoperability layers.*

## Service catalogue

| Service | What it provides | Capabilities realised |
| --- | --- | --- |
| Landing zone service | Pre-configured environment with networking, identity, policy and telemetry wired up | 2, 3, 4, 5 |
| Model gateway | Governed model access with routing, quota, safety and metering | 15, 8, 23 |
| Identity service | Agent and tool identity provisioning, delegation patterns, rotation | 7 |
| Policy service | Policy definitions, evaluation and evidence generation | 9 |
| Registry service | Inventory of agents, tools and MCP servers with ownership and status | 20 |
| Tool broker | Governed connection and permission scoping for tools and MCP servers | 19 |
| Knowledge service | Governed grounding content and retrieval endpoints | 16, 17 |
| Evaluation service | Evaluation harness, shared datasets and release gates | 18 |
| Observability service | Trace collection, dashboards, alerting and quality signals | 22 |
| FinOps service | Consumption attribution, budgets, quotas and reporting | 23, 1 |
| Delivery templates | Pipelines, infrastructure modules and reference implementations | 10, 11 |
| Marketplace | Discovery and self-service consumption of reusable components | 21 |

## Service contract

Each shared service publishes the same minimum contract, so that consuming teams can plan
against it:

- **Purpose and scope** — what it does, and explicitly what it does not do.
- **Interface** — API, template or portal entry point, with examples.
- **Onboarding path** — the self-service route and the expected elapsed time.
- **Service level** — availability target, support hours and escalation route.
- **Constraints** — quotas, supported regions, data residency and sensitivity tiers.
- **Cost model** — what is absorbed centrally and what is charged to the consumer.
- **Roadmap** — what is coming, so domains do not build a workaround for something
  arriving next quarter.

{: .note }
> A shared service without a published onboarding time is indistinguishable from a
> ticket queue. Publish the number and measure against it.

## Golden paths

Golden paths bundle services into an opinionated route for a common scenario type — for
example "retrieval-based assistant over a governed data product" or "background document
processing agent". A golden path includes a reference implementation, an infrastructure
template, a pipeline with evaluation and policy gates, dashboards, and a cost estimate.

Golden paths are the mechanism that makes the governed route the fastest route. They
should be measured by adoption: if teams routinely deviate, the path is wrong, not the
teams.

## Deviation and exception

Domains may deviate where the scenario genuinely requires it. Deviations are recorded in
the registry with a rationale, a compensating control and a review date. This keeps the
architecture honest and gives the platform team a prioritised backlog of gaps.
