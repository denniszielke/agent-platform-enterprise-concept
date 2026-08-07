---
layout: default
title: Shared Platform Services
parent: Architecture Concept
nav_order: 8
---

# Shared platform services

Shared platform services are the concrete things a domain team consumes on day one. They
are the realisation of the [capability model]({{ site.baseurl }}/02-Enterprise-Capabilities/)
as offerings with interfaces, accountable building blocks and service levels. The
catalogue spans the six building blocks; "shared" describes how a service is consumed,
not a seventh architecture block.

## Service catalogue

| Service | Providing building block | What it provides | Capabilities realised |
| --- | --- | --- | --- |
| Landing zone service | Cloud Platform | Pre-configured environment with networking, identity, policy and telemetry wired up | 2, 3, 4, 5 |
| Model gateway | AI Platform | Governed model access with routing, quota, safety and metering | 15, 8, 23 |
| Identity service | Agent Control Plane and Cloud Platform | Agent and tool identity provisioning, delegation patterns and revocation | 7 |
| Policy service | Agent Control Plane and Cloud Platform | Policy definitions, evaluation and evidence generation | 9 |
| Registry service | Agent Control Plane | Inventory of agents, tools and MCP servers with ownership and status | 20 |
| Tool broker | Application Platform and Agent Control Plane | Governed connection and permission scoping for tools and MCP servers | 19 |
| Knowledge service | Data Platform | Governed grounding content and retrieval endpoints | 16, 17 |
| Evaluation service | AI Platform and Agent Project | Shared harness, common tests, project datasets and release gates | 18 |
| Observability service | Application Platform and Agent Control Plane | Trace collection, dashboards, alerting and quality signals | 22 |
| FinOps service | Cloud Platform and AI Platform | Consumption attribution, budgets, quotas and reporting | 23, 1 |
| Delivery templates | Cloud Platform and Application Platform | Pipelines, infrastructure modules and reference implementations | 10, 11 |
| Marketplace | Agent Control Plane | Discovery and self-service consumption of reusable components | 21 |

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
