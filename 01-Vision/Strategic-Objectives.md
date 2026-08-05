---
layout: default
title: Strategic Objectives
parent: Vision
nav_order: 2
---

# Strategic objectives

Strategic objectives translate the vision into outcomes that an executive sponsor can
fund, measure and defend. Each objective is paired with the capability areas that must
be in place for it to be achievable.

## Objective 1 — Accelerate value delivery in core business scenarios

Reduce the elapsed time between an approved business idea and a governed agent running
in production. The platform achieves this by removing repeated foundation work:
landing zones, identity, model access, safety controls, telemetry and release
automation are consumed rather than rebuilt.

*Depends on:* Enterprise Platform Foundations (1-6), Lifecycle automation (10), AI
runtimes (11), Model gateway (15).

## Objective 2 — Make the governed route the fastest route

Guardrails must be embedded in the golden path rather than added as an approval gate at
the end. When the compliant option is also the quickest option, shadow AI becomes
unattractive without needing enforcement campaigns.

*Depends on:* Identity and trust (7), AI runtime protection (8), Governance and
compliance (9), Lifecycle automation (10).

## Objective 3 — Enable reuse across domains

Agents, tools, prompts, evaluations and data products built by one domain should be
discoverable and consumable by others. Reuse is what turns a series of successful
projects into a compounding platform investment.

*Depends on:* Tool and MCP connectivity (19), Agent and MCP registry (20), Enterprise
capability marketplace (21).

## Objective 4 — Ground agents in trusted enterprise knowledge

Agent quality is bounded by the quality and governance of the knowledge it can reach.
Grounding must respect the same access controls, retention rules and lineage
requirements as the source systems.

*Depends on:* Semantic foundation (16), Knowledge and data platform (17), Evaluation
engineering (18), Data governance.

## Objective 5 — Operate with transparency and predictable economics

Consumption-based AI spend is only defensible when it can be attributed to a scenario
and compared with the value that scenario produces. Observability and FinOps are
platform capabilities, not project add-ons.

*Depends on:* Observability (22), AI FinOps (23), Enterprise AI enablement operating
model (24).

## Measuring progress

| Objective | Example indicator | Typical owner |
| --- | --- | --- |
| Accelerate value delivery | Lead time from approved idea to production agent | Platform lead |
| Governed = fastest | Share of agents delivered on the golden path | Platform lead |
| Enable reuse | Number of registry components consumed outside their owning domain | Architecture board |
| Trusted grounding | Share of scenarios with automated evaluation gates | Data and quality owner |
| Predictable economics | Cost per completed task or conversation, per scenario | FinOps owner |

{: .note }
> Set target values per enterprise. Indicators are only useful when they are baselined
> before the platform exists, so capture the current state during Phase 0 of the
> [roadmap](Roadmap-Overview.md).

Objectives should be reviewed at every phase boundary. A capability that no longer
serves an objective is a candidate for simplification, and an objective without a
supporting capability is a gap in the [capability model]({{ site.baseurl }}/02-Enterprise-Capabilities/).

## Success criteria for the vision

The vision is successful when the platform is not perceived as extra governance
overhead, but as the fastest and safest path to production. Project teams should
experience it as an accelerator, control functions as a way to make evidence consistent
and repeatable, and sponsors as a way to turn AI investment into visible outcomes.

- There is a clear onboarding path for new AI projects, including which runtime,
  identity, network, telemetry, model gateway and data access patterns are available.
- The 24 committed capabilities are mapped to accountable platform owners, implementation
  artefacts and decision points, so scope is operationally actionable and not only
  documented.
- Every reusable AI asset — models, MCP servers, tools, data products and agents — has
  ownership, classification, lifecycle state, approved consumer scope and telemetry
  expectations.
- Model and tool consumption can be attributed to projects, teams, agents or workloads
  with enough fidelity to support FinOps, optimisation and governance discussions.
- Quality, safety and performance evaluation are integrated into the lifecycle rather
  than treated as after-the-fact documentation.
- The platform evolves as a product, with a roadmap, feedback loops, service catalogue,
  enablement material and reusable templates.
