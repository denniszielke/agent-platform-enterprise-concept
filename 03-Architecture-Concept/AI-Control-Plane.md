---
layout: default
title: AI Control Plane
parent: Architecture Concept
nav_order: 3
---

# AI control plane

The control plane is where the enterprise expresses what is allowed, records what exists,
and proves what happened. It is deliberately separated from the runtimes that execute
agents so that governance survives changes in runtime technology and cannot be bypassed
by a single team's choices.

## Functions

| Function | Responsibility | Capability |
| --- | --- | --- |
| Policy | Define, distribute and evaluate guardrails as code | 9 |
| Identity and trust | Issue and govern agent, tool and platform identities | 7 |
| Registry | Record every agent, tool and MCP server with owner and status | 20 |
| Lifecycle | Drive onboarding, release, change and decommission | 10 |
| Safety configuration | Own safety thresholds and content controls applied at runtime | 8 |
| Observability control | Define required telemetry and enforce its emission | 22 |
| Cost control | Own quotas, budgets and attribution metadata | 23 |

## Registry as the source of truth

The registry answers questions that become urgent during incidents: which agents are in
production, who owns them, which models and tools they use, what data they can reach,
when they were last evaluated, and what their cost profile looks like.

Registration is not paperwork — it is a pipeline step. An agent that is not registered
cannot obtain an identity, cannot be granted gateway quota, and therefore cannot reach a
model. This makes registration self-enforcing rather than dependent on discipline.

{: .highlight }
> Tie identity issuance and gateway quota to registry entries. Governance that is a
> prerequisite for functioning is the only governance that stays current.

## Policy distribution

Policies are authored centrally, versioned, and distributed to three enforcement points:

1. **Pipeline** — pre-deployment evaluation: is the model approved, is an evaluation
   suite present and passing, are tags and owners set, is the data tier permitted?
2. **Runtime** — admission and configuration: which tools may be attached, which memory
   stores may be used, which safety thresholds apply.
3. **Gateway** — per-call enforcement: identity, quota, safety filters, routing
   restrictions and metering.

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

The control plane itself remains centrally owned in both models. What changes is who
operates the enforcement points, not who defines the rules.
