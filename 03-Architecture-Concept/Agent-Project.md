---
layout: default
title: Agent Project
parent: Architecture Concept
nav_order: 7
---

# Agent project

The Agent Project is the accountable, regional delivery boundary in which a team composes
shared platform services into a production business solution. It owns the user
experience, agent behavior, project tools and MCP servers, workflow, project knowledge,
evaluation assets and workload operations. It realises capabilities 11-14 and consumes
capabilities 15-23 through published platform contracts.

The project team controls its code and configuration but cannot alter central model
deployments, enterprise gateways, registry policy, hub networking or inherited policy.
Production support remains shared: the project owns application behavior, while platform
teams own the service commitments of the building blocks they provide.

## Project responsibilities

| Project area | Responsibility | Typical Azure implementation |
| --- | --- | --- |
| Experience and API | Channel experience, authentication, conversation controls and user handoff | Web or Teams experience, API container and ingress configuration |
| Agent runtime | Reasoning, orchestration, tool selection and policy-aware behavior | Agent container on AKS or ACA using an approved framework |
| Tools and MCP servers | Bounded capabilities with deterministic validation and explicit authorization | MCP or API containers, APIM publication and workload identities |
| Project knowledge | Retrieval, indexing, memory and state under data governance | AI Search, Redis, Cosmos DB, SQL or storage |
| Workflow | Durable process state, approvals, retries and transaction boundaries | Logic Apps, Durable Functions, Functions and Service Bus |
| Quality and operations | Evaluation datasets, release thresholds, SLOs, dashboards, alerts and runbooks | Foundry evaluations, test harness, Application Insights and Log Analytics |
| Delivery automation | Build, scan, deploy, register and promote immutable releases | GitHub Actions or Azure Pipelines with Bicep or Terraform |

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

Container-hosted agents and MCP servers are the default custom-runtime pattern. Azure
Container Apps is preferred for stateless APIs, event-driven workers and agents that do
not need Kubernetes-specific controls. Azure Kubernetes Service is selected when a
project needs advanced network policy, custom operators, specialized scheduling, service
mesh, extensive sidecars or stronger tenancy control.

Managed agent services and serverless components remain valid where their hosting,
networking, identity and observability characteristics satisfy the project contract.
Foundry prompt agents are not the default for workloads requiring direct private network
access. Every runtime integrates with the same Agent Control Plane, model and tool
gateways, and telemetry contract; that integration, rather than one runtime product, is
what the platform standardizes.

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
changes or is withdrawn. Read tools and state-changing tools use different scopes and
policies. High-impact writes require deterministic validation, idempotency and, where
appropriate, human approval outside model reasoning.

## Marketplace

The marketplace turns the registry from an inventory into a reuse mechanism: teams
publish agents, tools, prompts and evaluation datasets with documentation, quality
signals and a cost profile, and other teams consume them self-service. Publication does
not grant tool or data access; consumer authorization remains a separate decision.
Marketplace value appears in Phase 3 of the
[roadmap]({{ site.baseurl }}/07-Implementation-Roadmap/), once there is enough built to
reuse.

## Experience integration

Agents reach users through collaboration surfaces, line-of-business applications, portals
and contact centres. The platform supplies consistent patterns for authentication,
citation display, confidence and limitation messaging, escalation to a human, and
feedback capture — because inconsistent behaviour across channels erodes trust faster
than occasional wrong answers.

## Prepared project package

Project onboarding should produce a usable environment rather than an empty subscription.
At minimum, the project receives its resource boundary, network attachment, groups and
managed identities, selected runtime tenancy, private logical model endpoint, telemetry
destination, approved data and messaging access, CI/CD path, budget and draft inventory
records. Development, test and production remain separate authorization boundaries, and
promotion moves immutable images and versioned configuration rather than rebuilding code
per environment.
