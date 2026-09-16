---
layout: default
title: Assumptions and Boundaries
parent: Vision
nav_order: 6
---

# Assumptions and design boundaries

The vision is only actionable if the assumptions behind it are explicit. These are the
design boundaries the rest of this concept is written against; an enterprise with
different constraints should restate them before reusing the architecture.

## Regional foundation with multi-region consumption

The cloud platform is assumed to be available across multiple regions, while individual
workloads are deployed and consumed as regional resources. For model consumption, the
[model gateway]({{ site.baseurl }}/05-Implementation-Patterns/Model-Gateway.html) can
abstract model hosting across regions where capacity and redundancy require it.

## Private enterprise network as the default

Private network consumption is assumed for most enterprise resources — models, gateways,
agents, MCP servers, data stores and platform services — unless a specific scenario
requires public exposure for user access or external integration.

## Container-hosted agents are in scope

Application hosting for custom agents and MCP servers primarily uses containers, with
Azure Kubernetes Service and Azure Container Apps as the main runtime options. Prompt-only
agents in a managed AI service are not treated as the primary scoped runtime where current
limitations make container-hosted agents the required pattern.

## Managed identities over shared secrets

Key-based authentication for models, storage accounts and other resources is assumed to
be denied by policy where possible. User-assigned managed identities, workload identities
and Entra-based authentication are the preferred control model.

## Hub-and-spoke topology with shared control points

The network topology follows a hub-and-spoke model. Centrally managed components such as
firewalls and shared egress live in the hub, while application, data and AI platforms are
implemented as spokes with defined ingress and egress patterns.

## Gateway-mediated access where it creates control value

Model gateway and API/MCP gateway patterns are the supported path for many model, tool
and MCP consumption scenarios. Exceptions can exist, but they should be deliberate
architecture decisions with explicit control, telemetry and cost trade-offs.

{: .warning }
> Assumptions age faster than principles. Review this page at every phase boundary of the
> [roadmap]({{ site.baseurl }}/07-Implementation-Roadmap/); an assumption that is no
> longer true usually invalidates a pattern rather than a principle.
