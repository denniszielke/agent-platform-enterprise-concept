---
layout: default
title: Capability to Architecture Mapping
parent: Architecture Concept
nav_order: 10
---

# Capability to architecture mapping

This mapping is the join between *what* the enterprise committed to and *where* it is
realised. It is the page to review when a new component is proposed: if it does not
realise a capability, it needs a justification; if a capability has no component, it is a
gap.

## Mapping table

| # | Capability | Primary building block | Supporting building blocks | Primary owner (centralised) | Primary owner (federated) |
| --- | --- | --- | --- | --- | --- |
| 1 | Billing, FinOps & Commercial Management | Cloud Platform | AI Platform, Agent Control Plane | Central | Central |
| 2 | Resource Organization & Platform Hierarchy | Cloud Platform | Agent Project | Central | Central |
| 3 | Identity, Roles & Access Management | Cloud Platform | Agent Control Plane, Agent Project | Central | Central definition, domain assignment |
| 4 | Network Topology & Connectivity | Cloud Platform | Application Platform, AI Platform, Data Platform | Central | Central |
| 5 | Platform Management & Operations | Cloud Platform | Application Platform, AI Platform, Data Platform | Central | Central |
| 6 | Business Continuity & Resilience | Cloud Platform | Application Platform, AI Platform, Data Platform, Agent Project | Central | Shared |
| 7 | Identity & Trust Foundation | Agent Control Plane | Cloud Platform, Agent Project | Central | Central policy, domain provisioning |
| 8 | AI Security, Trust & Runtime Protection | Agent Control Plane | AI Platform, Application Platform, Agent Project | Central | Central defaults, domain runtime |
| 9 | Governance, Risk & Compliance Platform | Agent Control Plane | Cloud Platform, AI Platform, Data Platform | Central | Central policy, domain enforcement |
| 10 | Lifecycle, Platform Engineering & Automation | Application Platform | Cloud Platform, Agent Control Plane, Agent Project | Central | Central templates, domain pipelines |
| 11 | AI Runtime & Execution Platform | Application Platform | Agent Project | Central | Domain |
| 12 | Workflow Orchestration & Agent Coordination | Agent Project | Application Platform | Central | Domain |
| 13 | Agent Memory & Context Management | Data Platform | Agent Project, Application Platform | Central | Domain |
| 14 | User Experience & Channel Integration | Agent Project | Application Platform | Central | Domain |
| 15 | Model Gateway & AI Access Platform | AI Platform | Cloud Platform, Agent Project | Central | Central |
| 16 | Enterprise Knowledge & Semantic Foundation | Data Platform | AI Platform, Agent Project | Central | Central standards, domain assets |
| 17 | Knowledge & Data Platform | Data Platform | Agent Project | Central | Domain |
| 18 | Evaluation, Benchmarking & Quality Engineering | Agent Project | AI Platform, Agent Control Plane | Central | Central harness, domain datasets |
| 19 | Tool, API & MCP Connectivity Platform | Application Platform | Agent Control Plane, Agent Project | Central | Central gateway, domain tools |
| 20 | Agent & MCP Registry | Agent Control Plane | Application Platform, Agent Project | Central | Central |
| 21 | Enterprise Capability Marketplace | Agent Control Plane | AI Platform, Data Platform, Application Platform | Central | Central platform, domain contributions |
| 22 | Observability, Telemetry & Evaluation Platform | Application Platform | Agent Control Plane, AI Platform, Agent Project | Central | Central platform, domain dashboards |
| 23 | AI FinOps & Cost Management Platform | Cloud Platform | AI Platform, Agent Control Plane, Agent Project | Central | Central platform, domain budgets |
| 24 | Enterprise Agent Enablement & Operating Model | Agent Control Plane | All building blocks | Central | Shared governance forum |

## Microsoft Cloud realisation

The architecture locations above remain capability-led. For the enterprise context in
this vision, the primary Microsoft Cloud alignment is:

| Architecture concern | Microsoft Cloud role |
| --- | --- |
| Cloud foundation | Azure landing zones, networking, policy, resilience and cost management |
| Application platform | Azure Kubernetes Service, Azure Container Apps, integration and messaging services, and API Management |
| AI platform | Microsoft Foundry accounts and projects, model deployments, capacity, telemetry and evaluation artefacts |
| Data platform | Microsoft Fabric data products and semantic models, complemented by Azure data, search, streaming and storage services |
| Agent control plane | Agent 365 registry concepts aligned with Entra, Purview and Defender controls and signals |
| Agent Project | Project-owned experience, agent, tools, state, workflow, evaluation assets and immutable delivery automation |

This alignment is not a product bill of materials. Capabilities and contracts remain the
stable design surface while individual services and implementation choices evolve.

## How to read the mapping

- **Primary building block** identifies the accountable service or delivery boundary for
  the capability.
- **Supporting building blocks** identify where the capability is enforced, consumed or
  evidenced without moving accountability.
- **Hub and spoke** remains the deployment topology: shared instances normally reside in
  platform hubs, while Agent Project resources and project-owned data or tools reside in
  spokes.

{: .highlight }
> Note how few rows actually change between the two operating models. The architecture is
> stable; the ownership column is the operating-model decision.

## Using the mapping

1. Baseline each capability's maturity in the
   [capability maturity model]({{ site.baseurl }}/02-Enterprise-Capabilities/Capability-Maturity-Model.html).
2. Confirm each capability has exactly one accountable owner in this table.
3. Flag capabilities with no realising component as roadmap items.
4. Review new components against the table before they are approved.
5. Re-check the ownership column whenever the operating model is revisited.
