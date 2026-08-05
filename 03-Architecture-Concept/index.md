---
layout: default
title: Architecture Concept
nav_order: 4
has_children: true
permalink: /03-Architecture-Concept/
---

# Architecture concept

The architecture concept translates the capability model into a layered structure of
shared services, control planes and runtimes. It deliberately stops short of choosing
an operating model: both the [centralised]({{ site.baseurl }}/03a-Centralized-Operating-Model/)
and the [federated]({{ site.baseurl }}/03b-Federated-Operating-Model/) pattern implement the
same architecture with different ownership boundaries.

![Layered architecture]({{ site.baseurl }}/assets/diagrams/layered-architecture.svg)
*Layers from enterprise platform foundations up to experience channels.*

| Page | What it answers |
| --- | --- |
| [Architecture Principles](Architecture-Principles.md) | Which rules constrain the design? |
| [Shared Platform Services](Shared-Platform-Services.md) | What does every team consume from the platform? |
| [AI Control Plane](AI-Control-Plane.md) | How are policy, identity and lifecycle enforced? |
| [AI Platform](AI-Platform.md) | How are models, prompts and evaluations delivered? |
| [Data Platform Integration](Data-Platform-Integration.md) | How does the platform ground agents in enterprise data? |
| [Agent Platform](Agent-Platform.md) | How are agents built, hosted, registered and consumed? |
| [Hub and Spoke Concept](Hub-and-Spoke-Concept.md) | How are hub and spoke responsibilities split? |
| [Capability to Architecture Mapping](Capability-to-Architecture-Mapping.md) | Where does each of the 24 capabilities live? |
