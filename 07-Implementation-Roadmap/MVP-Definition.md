---
layout: default
title: MVP Definition
parent: Implementation Roadmap
nav_order: 2
---

# MVP definition

The MVP is the smallest platform that can safely run a real production agent for real
users. Anything that can be added later without rework is deliberately excluded.

## Disposition of all 24 capabilities

The MVP does not mature all 24 capabilities at once, but it makes an explicit decision
about every capability. **Core MVP** capabilities are implemented for the first
production scenario. **Minimum baseline** capabilities establish the control, contract
or ownership needed to avoid later rework. A **deferred service** remains in the target
architecture but is not built until demand justifies it.

| # | Capability | MVP disposition | MVP scope | Deferred to later phases |
| --- | --- | --- | --- | --- |
| 1 | Billing, FinOps & Commercial Management | Minimum baseline | Named financial owner, budget and commercial construct mapped to the first scenario | Forecasting, reservations and broader commercial optimisation |
| 2 | Resource Organization & Platform Hierarchy | Core MVP | One landing-zone template with enforced naming and tagging | Multi-region and multi-tenant variants |
| 3 | Identity, Roles & Access Management | Core MVP | Four standard human roles with assignment and review process | Fine-grained custom roles and delegated administration |
| 4 | Network Topology & Connectivity | Core MVP | Private connectivity and controlled egress | Full micro-segmentation and cross-region topology |
| 5 | Platform Management & Operations | Core MVP | Infrastructure as code, named service owner, support route and change process | Full service catalogue and advanced operational automation |
| 6 | Business Continuity & Resilience | Minimum baseline | Availability target, dependency map, rollback and tested recovery procedure | Zone and regional redundancy by service tier |
| 7 | Identity & Trust Foundation | Core MVP | Agent identity plus on-behalf-of delegation | Autonomous delegation and broader federation patterns |
| 8 | AI Security, Trust & Runtime Protection | Core MVP | Default content safety, prompt-attack protection and tool-call policy | Custom classifiers and automated red-team coverage |
| 9 | Governance, Risk & Compliance Platform | Core MVP | AI use-case register, risk classification and pipeline policy gates | Automated regulatory reporting and recurring control evidence |
| 10 | Lifecycle, Platform Engineering & Automation | Core MVP | One versioned pipeline template as the only production path | Blue/green, canary and automated retirement strategies |
| 11 | AI Runtime & Execution Platform | Core MVP | One supported runtime with managed identity, scaling and telemetry | Runtime catalogue and additional isolation tiers |
| 12 | Workflow Orchestration & Agent Coordination | Minimum baseline | Explicit state, retry, failure and human-handoff pattern for the MVP workflow | General orchestration service and multi-agent coordination |
| 13 | Agent Memory & Context Management | Core MVP | Session memory with defined access, retention and deletion | Durable, team and organisational memory |
| 14 | User Experience & Channel Integration | Core MVP | One channel with authentication, feedback and human escalation | Multi-channel parity and reusable channel components |
| 15 | Model Gateway & AI Access Platform | Core MVP | Governed routing, quota, safety, resilience and metering | Advanced caching, provider federation and fine-tuning workflows |
| 16 | Enterprise Knowledge & Semantic Foundation | Minimum baseline | Owned business terms and metadata for the first scenario | Shared ontology, context graph and enterprise semantic discovery |
| 17 | Knowledge & Data Platform | Core MVP | One governed data product with permission trimming, lineage and freshness | Broader corpus and operational data-product onboarding |
| 18 | Evaluation, Benchmarking & Quality Engineering | Core MVP | Versioned offline quality and safety suite as a release gate | Continuous online evaluation and cross-scenario benchmarks |
| 19 | Tool, API & MCP Connectivity Platform | Minimum baseline | One governed connection pattern with identity, validation and correlated audit telemetry | Shared broker, MCP exposure and connector catalogue |
| 20 | Agent & MCP Registry | Minimum baseline | Production assets recorded in the governed AI inventory with owner, version and lifecycle state | Operational registry, automated publication and health integration |
| 21 | Enterprise Capability Marketplace | Deferred service | Consumer documentation and onboarding remain with the initial golden path | Searchable marketplace, approval workflow and reuse analytics |
| 22 | Observability, Telemetry & Evaluation Platform | Core MVP | Correlated traces, audit events and core operational and quality dashboards | Predictive alerting and cross-platform analytics |
| 23 | AI FinOps & Cost Management Platform | Core MVP | Tagging, model metering, scenario attribution and showback | Chargeback, forecasting and AI cost optimisation |
| 24 | Enterprise Agent Enablement & Operating Model | Minimum baseline | Named platform owner, responsibility model, onboarding guide and feedback route | Training programme, community of practice and mature product cadence |

## Explicitly out of scope

An operational registry beyond the governed inventory (20), a self-service marketplace
(21), general multi-agent orchestration (12), autonomous write actions, third-party agent
ecosystem integration and multi-tenant governance are implementation scope deferred from
the MVP. Their owners, contracts and control boundaries remain represented in the target
architecture so later delivery does not require redesign.

{: .highlight }
> The MVP is judged by whether a second team can use it without the platform team writing
> their code — not by how many capabilities are ticked.

## Definition of done

- A domain team onboards self-service within the published onboarding time.
- An agent reaches production only through the pipeline, with policy and evaluation gates
  passing.
- Every model call is authenticated, metered and attributable to a scenario.
- One correlated trace exists per user interaction.
- Retention and deletion rules are enforced for memory and telemetry.
- A rollback and incident procedure has been tested, not just documented.
- Cost per scenario is reported without manual reconciliation.

## Common MVP mistakes

| Mistake | Consequence |
| --- | --- |
| Building the registry and marketplace first | Empty catalogues, no reuse to capture |
| Skipping evaluation "until the agent stabilises" | The agent can never be safely changed |
| Granting direct model access for speed | Metering and safety gaps that are painful to close |
| Manual tagging | Cost attribution never becomes reliable |
| Choosing the operating model before Phase 1 ends | Decision made on assumptions instead of evidence |
