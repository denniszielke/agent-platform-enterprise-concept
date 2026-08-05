---
layout: default
title: MVP Definition
parent: Implementation Roadmap
nav_order: 2
---

# MVP definition

The MVP is the smallest platform that can safely run a real production agent for real
users. Anything that can be added later without rework is deliberately excluded.

## In scope for the MVP

| Capability | MVP scope | Deferred to later phases |
| --- | --- | --- |
| Resource organisation (2) | One landing zone template with enforced tagging | Multi-region, multi-tenant variants |
| Roles and access (3) | Four standard roles | Fine-grained custom roles |
| Network topology (4) | Private connectivity and controlled egress | Full micro-segmentation |
| Identity and trust (7) | Agent identity plus on-behalf-of delegation | Autonomous agent patterns |
| AI runtime protection (8) | Default safety filters and prompt shielding | Custom classifiers |
| Governance and compliance (9) | Use-case register plus pipeline policy gates | Automated regulatory reporting |
| Lifecycle automation (10) | One pipeline template as the only production path | Blue/green and canary strategies |
| AI runtimes (11) | One supported runtime | Runtime catalogue |
| Agent memory (13) | Session memory with defined retention | Durable and organisational memory |
| UX integration (14) | One channel | Multi-channel parity |
| Model gateway (15) | Routing, quota, safety, metering | Advanced caching, fine-tuning workflows |
| Knowledge platform (17) | One governed data product with permission trimming | Broad corpus onboarding |
| Evaluation engineering (18) | Offline suite as a release gate | Continuous online evaluation |
| Observability (22) | Correlated traces plus core dashboards | Predictive alerting |
| AI FinOps (23) | Tagging, metering and showback | Chargeback and forecasting |

## Explicitly out of scope

Registry beyond a simple inventory, marketplace, multi-agent orchestration, autonomous
write actions, third-party agent ecosystem integration and multi-tenant governance. Each
of these solves a problem the enterprise does not yet have at MVP stage.

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
