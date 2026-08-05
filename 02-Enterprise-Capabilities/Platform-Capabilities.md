---
layout: default
title: Platform Capabilities
parent: Enterprise Capabilities
nav_order: 3
---

# Platform capabilities

Platform capabilities cover the foundations agents run on and the runtimes that execute
them: capabilities 1-6 (Enterprise Platform Foundations) and 11-14 (Runtime &
Experience). They are the least glamorous part of the platform and the most expensive to
retrofit.

![Layered architecture]({{ site.baseurl }}/assets/diagrams/layered-architecture.svg)
*Platform capabilities span the foundation and runtime layers.*

## Foundations (1-6)

**Billing and commercial management (1).** Agent workloads consume capacity in bursts
and across several commercial constructs — model capacity, compute, search, storage and
third-party services. The platform maps each construct to an owner and an environment so
that a consumption spike has a name attached to it before the invoice arrives.

**Resource organisation (2).** A consistent hierarchy of tenants, subscriptions, resource
groups, naming and tagging is what makes every downstream capability — access control,
cost allocation, policy, observability — mechanically possible. Tags applied
inconsistently at this stage become manual reconciliation forever.

**Roles and access (3).** A small catalogue of standard roles for platform operators,
agent builders, data owners and auditors, with a defined assignment and review process.
Standard roles are what allow domain teams to be onboarded in hours rather than weeks.

**Network topology (4).** Segmentation and private connectivity for agent runtimes, model
endpoints, data stores and tool servers, with deliberate egress control. Network controls
constrain blast radius; they do not replace identity-based authorisation.

**Platform management (5).** All platform components are provisioned and changed through
code and pipelines. Manual configuration is treated as an incident to be remediated, not
as an acceptable shortcut.

**Resilience (6).** Availability and recovery objectives are defined per platform service
and per scenario tier. Agent scenarios often depend on several external services at once,
so degradation modes — cached responses, reduced tool sets, fallback models — must be
designed explicitly.

## Runtime and experience (11-14)

**AI runtimes (11).** Different scenarios need different execution characteristics:
low-latency conversational runtimes, long-running background workers, and isolated
runtimes for sensitive data. The platform publishes a catalogue of supported runtimes
with their trade-offs rather than mandating one.

| Runtime style | Suited to | Main trade-off |
| --- | --- | --- |
| Managed agent service | Fast delivery, standard scenarios | Less control over internals |
| Container-hosted runtime | Custom frameworks, portability | Team owns more operations |
| Serverless functions | Event-driven, bursty tasks | Cold start and execution limits |
| Isolated runtime | Regulated or sensitive data | Higher cost and slower onboarding |

**Workflow orchestration (12).** Multi-step and multi-agent processes need durable state,
retries, compensation and human-in-the-loop steps. Orchestration is where agent
behaviour becomes auditable business process rather than an opaque chain of calls.

**Agent memory (13).** Memory spans a working context within a session, durable user or
account memory, and shared organisational memory. Each has different retention, privacy
and isolation requirements, and each must be purgeable on request.

{: .warning }
> Memory is personal data in most jurisdictions the moment it retains user statements.
> Decide retention and deletion before the first production scenario, not after.

**User experience integration (14).** Agents must reach users where they already work —
collaboration surfaces, line-of-business systems, portals, contact centres and mobile
apps. Consistent patterns for authentication, citation display, escalation to a human and
feedback capture make agents feel like one platform rather than a collection of pilots.

## Operating model differences

| Capability | Centralised | Federated |
| --- | --- | --- |
| Foundations (1-6) | Central team owns entirely | Central team owns entirely |
| AI runtimes (11) | Shared runtimes operated centrally | Domain-operated from central templates |
| Workflow orchestration (12) | Central orchestration platform | Domain choice within supported catalogue |
| Agent memory (13) | Central store with domain partitions | Domain stores under central policy |
| UX integration (14) | Central channel integrations | Domain integrations, central patterns |
