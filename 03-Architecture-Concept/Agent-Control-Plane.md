---
layout: default
title: Agent Control Plane
parent: Architecture Concept
nav_order: 6
---

# Agent control plane

The Agent Control Plane makes the enterprise agent estate identifiable, discoverable,
governable and auditable across runtime products and project boundaries. It is where the
enterprise expresses what is allowed, records what exists and proves what happened. It
is deliberately separated from the runtimes that execute agents so that governance
survives changes in runtime technology and cannot be bypassed by a single team's choices.

For the Microsoft Cloud realisation, this building block aligns Microsoft Agent 365
registry concepts with Microsoft Entra identity, Microsoft Purview governance, Microsoft
Defender protection and security operations. Product availability can change; the
durable contract remains the identity, metadata, lifecycle, publication, policy and
evidence requirements defined below.

## Functions

| Function | Responsibility | Reference implementation |
| --- | --- | --- |
| Agent inventory | Record every production agent with immutable version, owner, sponsor, risk, data classes, dependencies and lifecycle state | Agent 365 registry or an interim enterprise registry |
| Tool and MCP inventory | Record callable tools and MCP servers with operation risk, authentication mode, scopes, owner and schema version | Agent 365 tool registry, API Center or internal catalogue |
| Agent identity | Issue and govern identities and link each environment binding to its registered agent | Microsoft Entra Agent ID, applications and managed identities as applicable |
| Runtime policy | Evaluate caller, agent, tool, operation and context at deterministic enforcement points | Entra authorization, APIM policy and application authorization |
| Governance and publication | Require ownership, evaluation, threat model, data classification, support and retirement evidence | Purview, risk workflow and Agent 365 governance |
| Threat operations | Correlate suspicious prompts, anomalous tool use, identity events and resource access | Defender, Sentinel and security telemetry |

## Registry as the source of truth

The registry answers questions that become urgent during incidents: which agents are in
production, who owns them, which models and tools they use, what data they can reach,
when they were last evaluated, and what their cost profile looks like.

Registration is not paperwork; it is an automated pipeline step. Every production agent
is inventoried, while agents and MCP servers intended for reuse are additionally
published to the enterprise catalogue. Internal helper components can remain hidden from
general consumers, but they still appear as dependencies of their owning agent.

Each registry record holds one logical agent identifier and version history. Environment
bindings map that record to the corresponding identity, runtime endpoint and deployment
version. Publication and authorization remain separate: listing an agent does not grant
access to its tools, models or data.

{: .highlight }
> Tie identity issuance, deployment and gateway policy to registry entries, but do not
> treat the registry as a security boundary. Containment separately disables the
> affected identity, gateway route or deployment.

## Policy distribution

Policies are authored centrally, versioned and distributed to three enforcement points:

1. **Pipeline** — pre-deployment evaluation: is the model approved, is an evaluation
   suite present and passing, are tags and owners set, is the data tier permitted?
2. **Runtime** — admission and configuration: which tools may be attached, which memory
   stores may be used, which safety thresholds apply.
3. **Gateway and service boundary** — per-call enforcement: caller and agent identity,
   operation allow-list, quota, safety filters, routing restrictions and metering.

Distributing the same policy to several enforcement points is what allows the federated
model to work without weakening control.

## Evidence

The control plane produces evidence automatically: deployment records, approvals,
evaluation results, policy evaluation outcomes, identity assignments and change history.
Evidence assembled by hand at audit time is expensive and usually incomplete; evidence
emitted by the platform is a by-product of normal operation.

## Operating model differences

| Aspect | Centralised | Federated |
| --- | --- | --- |
| Policy authoring | Central | Central with domain extensions |
| Enforcement | Central pipeline and gateway | Domain pipelines plus central gateway |
| Registry writes | Central team | Domain pipelines, central schema |
| Exception handling | Central approval | Delegated within pre-approved categories |

The Agent Governance team remains accountable for policy and publication in both models.
Control-plane engineering and operations can sit with a dedicated team or a joint team
drawn from Entra, security and platform engineering. What changes between operating
models is who operates distributed enforcement points, not who defines enterprise rules.
