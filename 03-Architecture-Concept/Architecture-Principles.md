---
layout: default
title: Architecture Principles
parent: Architecture Concept
nav_order: 1
---

# Architecture principles

These principles extend the [design principles]({{ site.baseurl }}/01-Vision/Design-Principles.html)
into concrete architectural constraints. Each one is stated as a rule, a rationale and an
implication that a reviewer can test a design against.

## Single governed entry point for model access

*Rule.* All model traffic flows through the model gateway.
*Rationale.* It is the only place where metering, quota, safety and routing can be
guaranteed consistently.
*Implication.* Direct endpoint access is not granted, even for prototypes; prototypes get
a development quota through the same gateway.

## Identity-first authorisation

*Rule.* Every action is authorised against the acting identity and, where applicable, the
delegated user identity.
*Rationale.* Network position is not a permission, and agents act autonomously.
*Implication.* Data access uses permission trimming at query time rather than filtering
after retrieval.

## Separate control plane from data plane

*Rule.* Policy, registry, lifecycle and observability are separate from agent execution.
*Rationale.* Control functions must survive changes in runtime technology, and must not
be bypassable by a runtime.
*Implication.* A new runtime can be onboarded by integrating with the control plane, not
by rebuilding governance.

## Contract-based composition

*Rule.* Agents consume tools, data and other agents through published contracts —
MCP for tools, data product interfaces for knowledge, documented APIs for services.
*Rationale.* Contracts make components replaceable and reusable across domains.
*Implication.* Point-to-point integrations that bypass the contract are technical debt
recorded as such.

## Stateless runtimes, externalised state

*Rule.* Agent runtimes hold no durable state; memory, conversation history and workflow
state live in managed stores.
*Rationale.* Enables scaling, failover and clean retention control.
*Implication.* Memory retention and deletion are enforced in one place rather than in
each runtime.

## Telemetry is not optional

*Rule.* A component that cannot emit correlated telemetry is not production-eligible.
*Rationale.* Without correlation there is no diagnosis, no cost attribution and no
quality measurement.
*Implication.* Trace context propagation is part of the runtime template.

## Policy as code

*Rule.* Guardrails are expressed as executable policy, not as documentation.
*Rationale.* Manual controls degrade under delivery pressure.
*Implication.* Every new control ships with its enforcement mechanism and its evidence
output.

## Progressive isolation

*Rule.* Isolation is applied proportionally to data sensitivity and blast radius, not
uniformly.
*Rationale.* Uniform maximum isolation makes onboarding slow enough that teams avoid the
platform.
*Implication.* A published tiering model defines which scenarios require dedicated
runtimes, networks or model deployments.

## Review checklist

| Question | Principle |
| --- | --- |
| Can this design reach a model without the gateway? | Single entry point |
| Whose identity performs each action? | Identity-first |
| Does governance depend on this runtime existing? | Control/data plane separation |
| Could another domain reuse this component as-is? | Contract-based composition |
| Where does memory live and when is it deleted? | Externalised state |
| Can one trace show the whole interaction? | Telemetry |
| Is any control enforced only by a human check? | Policy as code |
| Is the isolation level justified by the data tier? | Progressive isolation |
