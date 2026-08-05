---
layout: default
title: Platform Building Blocks
parent: Vision
nav_order: 5
---

# Platform building blocks

The Enterprise Agent Platform is composed from six building blocks. They distribute
accountability across platform teams while keeping the experience coherent for agent
project teams. Each block is realised by the [capability model]({{ site.baseurl }}/02-Enterprise-Capabilities/)
and detailed in the [architecture concept]({{ site.baseurl }}/03-Architecture-Concept/).

| Building block | Role | Typical components |
| --- | --- | --- |
| Cloud Platform | Makes the agent platform inherit the enterprise-grade foundation expected for regulated cloud workloads | Subscriptions, management groups, policy, identity, Defender, cost management, DNS, routing, virtual networks, firewalls, private connectivity, resilience patterns |
| Application Platform | Gives project teams a safe place to host AI applications, agents, MCP servers and integration components inside the enterprise network | Azure Kubernetes Service, Azure Container Apps, integration and messaging services, API Management, ingress patterns |
| AI Platform | Abstracts raw model endpoints and provides stable, policy-controlled access to approved model capabilities | Foundry accounts and projects, model deployments, model capacity, model gateway integration, telemetry, evaluation artefacts |
| Data Platform | Turns enterprise data into governed context that AI systems can safely retrieve, interpret and use | Microsoft Fabric, data products, streaming and storage services, semantic models, ontologies, vector and search indexes, data agents and MCP servers |
| Agent Control Plane | Ensures agents are treated as managed enterprise entities rather than hidden project artefacts | Agent registry concepts, agent identities, metadata, lifecycle state, tool registries, MCP inventories, security and observability signals |
| Agent Project | The delivery boundary in which a team composes approved components into a real agent-enabled solution | Business outcome, user journey, agent behaviour, solution logic, evidence that the solution is safe, useful and measurable |

## What changes for agent project teams

Project teams should no longer have to answer basic platform questions from scratch.
They should not need to invent a model endpoint strategy, decide independently how
secrets are handled, create unmanaged telemetry, design their own cost attribution,
register tools in isolated documents or negotiate network patterns for every workload.

In the target state a project starts from a known platform pattern and receives an
approved runtime option, a managed identity model, private connectivity patterns, access
to the model gateway, approved data and tool interfaces, logging and tracing
requirements, deployment automation and clear ownership metadata. The team's real work
then becomes the high-value work: selecting the business process, designing the user
experience, composing agents or workflows, grounding the solution in trusted data,
validating quality and proving impact.

{: .note }
> The building blocks are owned by different teams, but the project team should
> experience them as one paved road. Where a block is not yet available, the
> [roadmap]({{ site.baseurl }}/07-Implementation-Roadmap/) should say who provides the
> interim path.
