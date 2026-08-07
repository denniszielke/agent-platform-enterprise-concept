---
layout: default
title: Interoperability Capabilities
parent: Enterprise Capabilities
nav_order: 7
---

# Interoperability capabilities

Interoperability capabilities make enterprise functions discoverable and safely reusable
by agents and applications. They cover the governed connection itself (19), the
authoritative inventory of reusable assets (20), and the experience through which
consumers discover and adopt those assets (21). Keeping these capabilities separate
prevents a protocol endpoint, a metadata store, or a portal from being mistaken for a
complete interoperability platform.

## Tool, API & MCP Connectivity Platform (19)

Agents need standard, governed paths to enterprise APIs, tools, MCP servers, SaaS
services, data platforms and business systems. The connectivity platform mediates those
paths so that each workload does not create a bespoke integration with its own identity,
network, throttling and audit behaviour.

The platform must support API mediation, MCP exposure, protocol translation,
authentication and delegated authorisation, traffic management, connector lifecycle,
and end-to-end telemetry. It applies controls according to the consequence of an
operation rather than treating every tool call alike.

| Operation class | Required control posture |
| --- | --- |
| Read public or low-sensitivity data | Authenticated caller, schema validation and standard telemetry |
| Read sensitive enterprise data | User or workload authorisation, permission trimming and audit evidence |
| Change business state | Explicit action scope, input validation, idempotency and durable audit trail |
| High-impact or irreversible action | Human approval or equivalent policy gate, strong attribution and recovery procedure |

Every invocation must be attributable to the user, agent, workload and service identity
involved. Tool descriptions and schemas are part of the security boundary: they are
versioned, validated and reviewed alongside the implementation they expose.

**Evidence it exists:** approved connectivity patterns are published; production tools
use governed authentication and policy enforcement; and correlated traces show the
caller, selected operation, policy decision, outcome and owning service.

## Agent & MCP Registry (20)

The registry is the authoritative inventory for agents, MCP servers, APIs, tools and
other reusable AI assets. It answers who owns an asset, what it can do, which data and
systems it reaches, whether it is approved, and who may consume it. Registration is a
lifecycle requirement, not an optional publishing step.

| Required metadata | Purpose |
| --- | --- |
| Stable identifier, type and version | Distinguish assets and resolve compatible contracts |
| Owner and operational contact | Establish accountability and an escalation path |
| Capability description and operation risk | Support safe selection by people and agents |
| Authentication and authorisation pattern | Make the trust boundary explicit |
| Data classification and dependencies | Expose access, privacy and resilience implications |
| Approved consumers and environments | Constrain where the asset may be used |
| Evaluation, health and lifecycle state | Prevent discovery of unverified or retired assets |

Deployment automation registers or updates assets and prevents unregistered production
endpoints from becoming shared dependencies. Retirement changes discovery state before
an endpoint is removed, giving consumers a controlled migration path.

**Evidence it exists:** the registry is the source of truth for production agents and
MCP servers; required metadata is enforced in deployment pipelines; and ownership,
approval, health and lifecycle state can be queried without manual reconciliation.

## Enterprise Capability Marketplace (21)

The marketplace is the consumption experience over registered assets. The registry
stores authoritative metadata; the marketplace adds search, documentation, onboarding,
approval workflows, consumer guidance and usage insight. This distinction allows the
registry to remain a dependable control-plane service while the user experience evolves.

Marketplace entries explain the approved scenarios, supported versions, service levels,
cost or quota implications, data constraints, integration steps and support route. Access
requests and onboarding are connected to the registry and identity platform so that
discovery never implies automatic permission.

The marketplace should be introduced when reusable assets exist and teams are beginning
to duplicate them. Its success is measured by adoption outside the owning domain,
reduced onboarding time and avoided duplicate integrations rather than by the number of
published entries.

**Evidence it exists:** consumers can discover, assess, request and onboard a capability
through a documented path; approved metadata comes from the registry; and reuse is
measured across domain boundaries.

## Capability relationship

| Capability | System responsibility | It is not |
| --- | --- | --- |
| Connectivity platform (19) | Securely executes and observes interactions | The inventory of available assets |
| Registry (20) | Records authoritative metadata, ownership and lifecycle | The runtime traffic path or user portal |
| Marketplace (21) | Helps consumers discover and adopt approved assets | A separate source of truth |

## Operating model differences

| Capability | Centralised | Federated |
| --- | --- | --- |
| Tool, API & MCP connectivity (19) | Central broker and centrally delivered integrations | Central standards and broker, domain-owned tools |
| Agent & MCP registry (20) | Central team registers and governs assets | Central registry, domain-published metadata with central policy |
| Enterprise capability marketplace (21) | Central team curates and onboards consumers | Central experience, domain-contributed entries and support |

In both operating models, the registry remains authoritative and policy remains
enterprise-wide. Federation changes who builds and operates the connected assets, not
whether they are registered, governed and observable.

See the [integration patterns]({{ site.baseurl }}/05-Implementation-Patterns/Integration-Patterns.html)
for implementation options and the
[capability-to-architecture mapping]({{ site.baseurl }}/03-Architecture-Concept/Capability-to-Architecture-Mapping.html)
for hub-and-spoke placement.