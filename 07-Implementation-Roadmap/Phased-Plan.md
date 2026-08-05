---
layout: default
title: Phased Plan
parent: Implementation Roadmap
nav_order: 1
---

# Phased plan

Each phase removes the largest remaining constraint and ends with an explicit gate. The
horizons below are typical for a large enterprise starting from an established cloud
foundation; adjust them, but keep the ordering.

## Phase 0 — Align (0-1 month)

**Purpose.** Remove ambiguity about scope, value and success.

*Activities:* agree the vision and strategic objectives; select two to four candidate
business scenarios with named sponsors; baseline all 24 capabilities against the
[maturity model]({{ site.baseurl }}/02-Enterprise-Capabilities/Capability-Maturity-Model.html);
capture current-state metrics; agree design and architecture principles; identify
regulatory constraints and data sensitivity tiers.

*Exit criteria:* signed-off objectives with measurable indicators; capability baseline
with owners; candidate scenarios with sponsors and success measures.

## Phase 1 — Foundations (1-3 months)

**Purpose.** Remove the foundation work that would otherwise be repeated per project.

*Activities:* deploy the landing zone (capabilities 2-5); stand up the model gateway (15)
with quota, safety and metering; implement agent identity and delegation patterns (7);
establish policy as code and pipeline gates (9, 10); wire correlated telemetry (22);
publish the first golden path with a reference implementation; define the initial
allocation model (23).

*Exit criteria:* a domain team can onboard self-service within the published time; all
model traffic flows through the gateway; one correlated trace per interaction is visible;
policy violations fail a pipeline.

{: .warning }
> Do not attempt all 24 capabilities here. Foundations plus governance and the gateway
> are enough; interoperability capabilities deliberately wait.

## Phase 2 — First agents (3-6 months)

**Purpose.** Remove doubt about whether the paved road works under real conditions.

*Activities:* deliver two production scenarios end to end on golden paths; build
evaluation datasets and release gates (18); integrate governed grounding data products
(16, 17); implement channel integration patterns (14); establish memory retention rules
(13); run the first operational readiness and security reviews; measure unit cost per
scenario.

*Exit criteria:* two agents serving real users with evaluation gates in the pipeline,
correlated telemetry, attributed cost, and a documented incident and rollback procedure.

## Phase 3 — Scale (6-12 months)

**Purpose.** Remove duplication now that there is enough built to reuse.

*Activities:* operationalise the registry (20) as the source of truth; establish the tool
broker and MCP connectivity standards (19); launch the marketplace (21); mature FinOps to
chargeback or hybrid allocation (23); onboard additional domains; take the operating model
decision or transition; harden resilience targets (6).

*Exit criteria:* components consumed outside their owning domain; onboarding time stable
as domain count grows; unit cost trending down; operating model decision recorded with
rationale.

## Phase 4 — Operate (12+ months)

**Purpose.** Remove decay by making improvement routine.

*Activities:* continuous evaluation and drift monitoring; model deprecation and migration
cycles; periodic capability re-assessment; access and identity reviews; decommissioning of
unused agents; ongoing enablement, documentation and community practice (24).

*Exit criteria:* this phase does not end. It is measured by cadence adherence rather than
completion.

## Phase summary

| Phase | Capability focus | Gate question |
| --- | --- | --- |
| 0 Align | Baseline all 24 | Do we agree what success looks like? |
| 1 Foundations | 2-5, 7, 9, 10, 15, 22 | Can a team build safely without central help? |
| 2 First agents | 13, 14, 16, 17, 18, 23 | Does the paved road survive real users? |
| 3 Scale | 6, 19, 20, 21, 23 | Are we reusing rather than repeating? |
| 4 Operate | 8, 18, 22, 24 | Is the platform improving without a programme? |
