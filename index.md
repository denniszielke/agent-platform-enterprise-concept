---
layout: default
title: Home
nav_order: 1
permalink: /
---

<div class="hero" markdown="1">

# Enterprise Agent Platform

An open, scalable ecosystem in which business and technology teams can compose agents,
models, tools, data products and enterprise services — across multiple runtimes and
delivery channels, under one set of enterprise controls.

</div>

This site guides a reader from **vision** to **enterprise capabilities**, then to
**architecture choices** and finally to an **implementation roadmap**. Centralised and
federated operating models are treated as first-class architecture patterns, and the
common capability model is agreed *before* branching into implementation choices.

![Journey from vision to roadmap]({{ site.baseurl }}/assets/diagrams/journey.svg)
*The journey. Each stage reuses the same capability model.*

## Start here

<div class="card-grid" markdown="1">

<div class="card" markdown="1">
### 1. Vision
Why the platform exists, which outcomes justify it, and who it serves.

[Read the vision]({{ site.baseurl }}/01-Vision/){: .btn .btn-primary }
</div>

<div class="card" markdown="1">
### 2. Enterprise capabilities
The 24 committed capabilities that define the scope and the value delivered.

[See the capability model]({{ site.baseurl }}/02-Enterprise-Capabilities/){: .btn }
</div>

<div class="card" markdown="1">
### 3. Architecture choices
Six building blocks, their service contracts, hub-and-spoke topology, and the path from
centralised foundations to federated delivery.

[Explore the architecture]({{ site.baseurl }}/03-Architecture-Concept/){: .btn }
</div>

<div class="card" markdown="1">
### 4. Implementation roadmap
Five phases from alignment to continuous operation, with gates and metrics.

[View the roadmap]({{ site.baseurl }}/07-Implementation-Roadmap/){: .btn }
</div>

</div>

## The 24 committed capabilities

![Capability model]({{ site.baseurl }}/assets/diagrams/capability-model.svg)

| Capability area | # | Contents |
| --- | --- | --- |
| Enterprise Platform Foundations | 1-6 | Billing and commercial management, resource organisation, roles and access, network topology, platform management, resilience |
| Governance & Security | 7-10 | Identity and trust, AI runtime protection, governance and compliance, lifecycle automation |
| Runtime & Experience | 11-14 | AI runtimes, workflow orchestration, agent memory, user experience integration |
| Intelligence | 15-18 | Model gateway, semantic foundation, knowledge and data platform, evaluation engineering |
| Interoperability | 19-21 | Tool and MCP connectivity, agent and MCP registry, enterprise capability marketplace |
| Operations | 22-24 | Observability, AI FinOps, enterprise AI enablement operating model |

## Two viable operating models, one architecture

The formal choice is made at the end of Phase 2, after the first production agents have
tested the platform under real conditions. Both models remain viable and use the same
capability model and architecture; they differ primarily in delivery ownership.

<div class="card-grid" markdown="1">

<div class="card" markdown="1">
### Centralised
One platform team owns build and run. This maximises consistency and direct control but
requires enough central capacity to meet enterprise demand.

[Centralised operating model]({{ site.baseurl }}/03a-Centralized-Operating-Model/){: .btn }
</div>

<div class="card" markdown="1">
### Federated
The central team owns the paved road; domain teams build and operate their own agents.
This enables parallel domain delivery and demands mature guardrails and telemetry.

[Federated operating model]({{ site.baseurl }}/03b-Federated-Operating-Model/){: .btn }
</div>

</div>

{: .highlight }
> The architecture stays stable whichever model is selected; the ownership column changes.
> See the [capability to architecture mapping]({{ site.baseurl }}/03-Architecture-Concept/Capability-to-Architecture-Mapping.html).

## How to use this material

- **Executives** — read the [vision]({{ site.baseurl }}/01-Vision/Vision.html) and the
  [roadmap overview]({{ site.baseurl }}/01-Vision/Roadmap-Overview.html).
- **Architects** — start with the [capability model]({{ site.baseurl }}/02-Enterprise-Capabilities/Capability-Model.html),
  then the [architecture concept]({{ site.baseurl }}/03-Architecture-Concept/).
- **Platform and domain engineers** — go to the
  [implementation patterns]({{ site.baseurl }}/05-Implementation-Patterns/) and the
  [cross-cutting topics]({{ site.baseurl }}/04-Cross-Cutting-Topics/).
- **Everyone** — baseline your own capabilities with the
  [maturity model]({{ site.baseurl }}/02-Enterprise-Capabilities/Capability-Maturity-Model.html)
  before choosing products.
