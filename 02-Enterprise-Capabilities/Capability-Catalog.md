---
layout: default
title: Capability Catalog
parent: Enterprise Capabilities
nav_order: 2
---

# Capability catalog

The catalog lists all 24 committed capabilities with their intent and the primary
evidence that the capability is genuinely in place. Detailed treatment per area is in
the following pages.

## Enterprise Platform Foundations (1-6)

| # | Capability | Intent | Evidence it exists |
| --- | --- | --- | --- |
| 1 | Billing and commercial management | Commercial agreements, subscriptions and quotas are managed centrally and predictably | Agreements mapped to environments and owners |
| 2 | Resource organisation | Consistent tenancy, subscription, resource group, naming and tagging structure | Tagging policy enforced automatically |
| 3 | Roles and access | Standard role definitions and assignment processes for platform and workloads | Role catalogue with review cadence |
| 4 | Network topology | Segmentation, private connectivity and egress control for agent workloads | Reference topology deployed by template |
| 5 | Platform management | Provisioning, configuration and change management of platform components | Infrastructure delivered as code |
| 6 | Resilience | Availability, capacity, failover and recovery targets for platform services | Tested recovery objectives per service |

## Governance & Security (7-10)

| # | Capability | Intent | Evidence it exists |
| --- | --- | --- | --- |
| 7 | Identity and trust | Distinct identities for users, agents and tools with delegated authorisation | End-to-end identity chain in audit logs |
| 8 | AI runtime protection | Protection against prompt injection, harmful content and unsafe tool use | Safety controls enforced at the gateway and runtime |
| 9 | Governance and compliance | Policy definition, approval and evidence for regulated use | Automated policy evaluation and reporting |
| 10 | Lifecycle automation | Automated build, test, release and decommission of agents and platform | Pipelines as the only production path |

## Runtime & Experience (11-14)

| # | Capability | Intent | Evidence it exists |
| --- | --- | --- | --- |
| 11 | AI runtimes | Governed hosting options for agents with different latency and isolation needs | Runtime catalogue with published trade-offs |
| 12 | Workflow orchestration | Coordination of multi-step and multi-agent processes with state and compensation | Orchestration patterns with retry semantics |
| 13 | Agent memory | Short and long-term memory with retention, isolation and privacy controls | Memory stores with documented retention |
| 14 | User experience integration | Delivery of agents into the channels users already work in | Channel integration templates |

## Intelligence (15-18)

| # | Capability | Intent | Evidence it exists |
| --- | --- | --- | --- |
| 15 | Model gateway | Single governed entry point for model access, routing, quota and metering | All model traffic traceable through the gateway |
| 16 | Semantic foundation | Shared meaning: ontologies, business glossary, embeddings and metadata | Published semantic assets reused by agents |
| 17 | Knowledge and data platform | Governed, permission-trimmed grounding content and data products | Data products with lineage and owners |
| 18 | Evaluation engineering | Systematic quality, safety and regression evaluation of agent behaviour | Evaluation gates in release pipelines |

## Interoperability (19-21)

| # | Capability | Intent | Evidence it exists |
| --- | --- | --- | --- |
| 19 | Tool and MCP connectivity | Standardised, governed connection of agents to tools and enterprise APIs | MCP servers registered with scoped permissions |
| 20 | Agent and MCP registry | Authoritative inventory of agents, tools and their ownership and status | Registry as the source of truth for production agents |
| 21 | Enterprise capability marketplace | Discovery and self-service consumption of reusable components | Components consumed outside their owning domain |

## Operations (22-24)

| # | Capability | Intent | Evidence it exists |
| --- | --- | --- | --- |
| 22 | Observability | Traces, metrics, logs and quality signals across agents, tools and models | One correlated trace per user interaction |
| 23 | AI FinOps | Attribution, forecasting and optimisation of AI consumption | Unit cost reported per scenario |
| 24 | Enterprise AI enablement operating model | Roles, funding, skills and governance forums that sustain the platform | Named owners and an operating cadence |

{: .note }
> The evidence column matters more than the capability name. A capability that cannot be
> evidenced is an intention, not a capability.
