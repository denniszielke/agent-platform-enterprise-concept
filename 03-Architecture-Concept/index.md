---
layout: default
title: Architecture Concept
nav_order: 4
has_children: true
permalink: /03-Architecture-Concept/
---

# Architecture concept

The architecture concept translates the capability model into six concrete building
blocks with explicit ownership boundaries and service contracts. Together they form a
shared control plane and a set of prepared delivery environments in which project teams
compose approved models, tools, data products, skills and agents into production
solutions. Both the
[centralised]({{ site.baseurl }}/03a-Centralized-Operating-Model/) and
[federated]({{ site.baseurl }}/03b-Federated-Operating-Model/) models implement this
architecture with different ownership boundaries. The formal choice is made at the end
of Phase 2; neither option requires a different capability model or a re-architecture.

## Six building blocks

| Building block | Architectural responsibility | Detailed page |
| --- | --- | --- |
| **Cloud Platform** | Multi-region landing zone, network, identity, policy, security, cost and resilience foundation | [Cloud Platform](Cloud-Platform.md) |
| **Application Platform** | Regional container runtimes, integration, ingress, messaging and workload operations | [Application Platform](Application-Platform.md) |
| **AI Platform** | Governed model access, Foundry resources, model routing, safety policy and AI usage evidence | [AI Platform](AI-Platform.md) |
| **Data Platform** | Governed data products, semantic models, search, memory and data agents | [Data Platform](Data-Platform.md) |
| **Agent Control Plane** | Agent identity, inventory, policy, publication, risk and security governance | [Agent Control Plane](Agent-Control-Plane.md) |
| **Agent Project** | Regional delivery boundary in which a team builds and operates a business solution | [Agent Project](Agent-Project.md) |

The blocks are logical ownership boundaries, not a requirement for six isolated
deployments. Hub-and-spoke remains the preferred Azure topology, while the building
blocks identify who supplies each capability and what an Agent Project consumes.

## Lifecycle overlay

Build, Scale, Govern and Optimize are cross-cutting lifecycle concerns rather than
separate products or layers:

| Lifecycle pillar | Intent | Primary building blocks |
| --- | --- | --- |
| **Build** | Reusable frameworks, models, data grounding, templates and development environments | Agent Project, AI Platform, Data Platform, Application Platform |
| **Scale** | Reliable execution, sessions, memory, workflow and isolated processing | Application Platform, Agent Project, Data Platform |
| **Govern** | Identity, inventory, mediated access, policy enforcement and threat detection | Agent Control Plane, Cloud Platform, AI Platform, Application Platform |
| **Optimize** | Evaluation, tracing, production quality, cost and continuous improvement | Agent Project, AI Platform, Agent Control Plane |

This is an architectural adaptation of the lifecycle, not a claim of product
equivalence. Azure services implement the contracts and can be replaced when an
alternative satisfies the same controls.

## Explore the concept

| Page | What it answers |
| --- | --- |
| [Architecture Principles](Architecture-Principles.md) | Which rules constrain the design? |
| [Cloud Platform](Cloud-Platform.md) | Which multi-region Azure foundations and guardrails does every project inherit? |
| [Application Platform](Application-Platform.md) | How are agents, MCP servers, APIs and workflows hosted and operated? |
| [Shared Platform Services](Shared-Platform-Services.md) | What does every team consume from the platform? |
| [Agent Control Plane](Agent-Control-Plane.md) | How are identity, inventory, policy and lifecycle enforced? |
| [AI Platform](AI-Platform.md) | How are models, prompts and evaluations delivered? |
| [Data Platform](Data-Platform.md) | How does the platform ground agents and govern session, workflow and memory state? |
| [Agent Project](Agent-Project.md) | How does a delivery team compose and operate a production solution? |
| [Hub and Spoke Concept](Hub-and-Spoke-Concept.md) | How are hub and spoke responsibilities split? |
| [Capability to Architecture Mapping](Capability-to-Architecture-Mapping.md) | Where does each of the 24 capabilities live? |
| [Enterprise Agent Platform Architecture Concept](Enterprise-Agent-Platform-Architecture-Concept.md) | What are the complete service contracts, scenarios, lifecycle gates and readiness criteria? |
