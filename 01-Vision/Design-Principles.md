---
layout: default
title: Design Principles
parent: Vision
nav_order: 3
---

# Design principles

Principles are the tie-breakers used when two designs both appear reasonable. They are
intentionally few, testable and occasionally uncomfortable — a principle that never
rules anything out is not a principle.

## 1. Capabilities before products

Agree *what* must exist before deciding *how* it is built. Every architecture discussion
starts from the [capability model]({{ site.baseurl }}/02-Enterprise-Capabilities/), so
that vendor and runtime choices remain reversible decisions rather than implicit
operating-model decisions.

## 2. Open and composable by default

Agents, tools, models and data products are composed through documented interfaces —
notably the Model Context Protocol (MCP) for tool connectivity — so that a component can
be replaced without redesigning its consumers. The platform prescribes contracts, not
implementations.

## 3. Governed interoperability, not central gatekeeping

Shared standards and a shared control plane give the enterprise consistency. Delivery
autonomy stays with the domain wherever the guardrails make it safe. This principle is
what makes the [federated model]({{ site.baseurl }}/03b-Federated-Operating-Model/) viable.

## 4. Identity is the primary control

Humans, agents and tools each have a distinct identity. Authorisation is always
evaluated against the acting identity and, where relevant, the delegated user identity.
Network controls complement identity; they never substitute for it.

## 5. Everything is observable and attributable

Every model call, tool invocation and agent decision emits telemetry that can be traced
to a scenario, an owner and a cost centre. Observability is designed in at the gateway
and runtime, never retrofitted per project.

## 6. Evaluate continuously, not once

Agent behaviour drifts as models, prompts, tools and data change. Quality and safety
evaluations run as automated gates in the release pipeline and as ongoing monitoring in
production.

## 7. Automate the paved road

If a control can only be satisfied manually, it will eventually be skipped. Guardrails
are expressed as policy, templates and pipelines so compliance is the by-product of
using the platform.

## 8. Design for reversibility

Assume models, runtimes and even the operating model will change. Prefer abstractions —
a model gateway, a registry, a capability marketplace — that localise the blast radius
of a change.

{: .warning }
> Reversibility has a cost. Apply it where change is likely (models, runtimes, tool
> providers) and accept coupling where it is not, rather than abstracting everything.

## Applying the principles

| Situation | Principle applied | Typical outcome |
| --- | --- | --- |
| A domain wants a different agent framework | 1, 2, 8 | Allowed if it consumes the shared control plane and registry |
| A team requests direct model endpoint access | 4, 5 | Denied; routed through the model gateway |
| A pilot has no evaluation suite | 6 | Blocked from production promotion |
| A control exists only as a review checklist | 7 | Converted into policy or pipeline automation |
| Central team is the delivery bottleneck | 3 | Trigger to move towards the federated model |
