---
layout: default
title: Agent Platform
parent: Architecture Concept
nav_order: 6
---

# Agent platform

The agent platform is where business scenarios are built: runtimes that execute agents,
orchestration for multi-step work, memory, tool connectivity and the channels that deliver
agents to users. It realises capabilities 11-14 and 19-21.

## Anatomy of a platform agent

| Element | Description | Governed by |
| --- | --- | --- |
| Identity | Distinct workload identity with delegation pattern | Control plane (7) |
| Instructions | Versioned system prompt and output contract | AI platform (16) |
| Tools | Registered tools and MCP servers with scoped permissions | Tool broker (19) |
| Knowledge | Governed retrieval endpoints | Data platform (17) |
| Memory | Session and durable memory with retention rules | Runtime (13) |
| Evaluation | Dataset and thresholds gating release | Evaluation service (18) |
| Telemetry | Correlated traces, metrics and quality signals | Observability (22) |
| Registry entry | Owner, status, dependencies, risk tier | Registry (20) |

An agent missing any of these is a prototype. The distinction matters because prototypes
routinely end up serving real users.

## Runtime selection

The platform supports several runtimes rather than mandating one, and publishes the
trade-offs so teams can choose deliberately — managed agent services for speed,
container-hosted runtimes for custom frameworks and portability, serverless for
event-driven work, and isolated runtimes for regulated data. All of them integrate with
the same control plane, gateway and telemetry pipeline; that integration, not the runtime
choice, is what the platform standardises.

## Multi-agent composition

As scenarios grow, agents call other agents. Three patterns dominate:

- **Supervisor.** A coordinating agent delegates to specialists and assembles the result.
  Predictable and auditable; the supervisor can become a bottleneck.
- **Sequential pipeline.** Agents run in a defined order with explicit hand-offs. Best
  where the process is stable and compliance requires traceability.
- **Peer collaboration.** Agents interact dynamically. Powerful for open-ended problems,
  hardest to bound in cost and behaviour.

{: .warning }
> Multi-agent designs multiply cost, latency and failure modes. Require evidence that a
> single agent with better tools cannot solve the problem first.

Regardless of pattern, agent-to-agent calls are authenticated, registered and traced like
any other call. An internal agent is a service with an identity, not a function call.

## Tool connectivity and the registry

Tools are attached through the tool broker using the Model Context Protocol where
possible, with permissions scoped per agent and per user context. The registry keeps the
authoritative record of which agent uses which tool, enabling impact analysis when a tool
changes or is withdrawn.

## Marketplace

The marketplace turns the registry from an inventory into a reuse mechanism: teams
publish agents, tools, prompts and evaluation datasets with documentation, quality
signals and a cost profile, and other teams consume them self-service. Marketplace value
appears in Phase 3 of the [roadmap]({{ site.baseurl }}/07-Implementation-Roadmap/), once
there is enough built to reuse.

## Experience integration

Agents reach users through collaboration surfaces, line-of-business applications, portals
and contact centres. The platform supplies consistent patterns for authentication,
citation display, confidence and limitation messaging, escalation to a human, and
feedback capture — because inconsistent behaviour across channels erodes trust faster
than occasional wrong answers.
