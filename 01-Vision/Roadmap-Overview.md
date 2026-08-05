---
layout: default
title: Roadmap Overview
parent: Vision
nav_order: 5
---

# Roadmap overview

The roadmap turns the vision into a sequence of phases that each deliver usable value.
It is summarised here for executive readers; the detailed activities, entry and exit
criteria are in the [implementation roadmap]({{ site.baseurl }}/07-Implementation-Roadmap/).

![Implementation roadmap]({{ site.baseurl }}/assets/diagrams/roadmap.svg)
*Five phases from alignment to continuous operation.*

| Phase | Focus | Typical horizon | Primary outcome |
| --- | --- | --- | --- |
| 0 — Align | Vision, scenarios, capability baseline | 0-1 month | Agreed scope and measurable objectives |
| 1 — Foundations | Landing zone, identity, gateway, observability | 1-3 months | A governed environment agents can be built in |
| 2 — First agents | Two production scenarios on golden paths | 3-6 months | Proven value and a validated paved road |
| 3 — Scale | Registry, marketplace, FinOps, federation | 6-12 months | Reuse across domains and predictable economics |
| 4 — Operate | Continuous evaluation and maturity growth | 12+ months | A durable, improving platform |

## Sequencing logic

Phases are ordered so that each one removes the largest remaining constraint:

- Phase 0 removes ambiguity about scope and success.
- Phase 1 removes the foundation work that would otherwise be repeated per project.
- Phase 2 removes doubt about whether the paved road actually works, by driving real
  scenarios through it end to end.
- Phase 3 removes duplication, once there is enough built to be worth reusing.
- Phase 4 removes decay, by making evaluation and improvement routine.

{: .highlight }
> Do not build every capability before the first agent. Phase 1 should deliver the
> minimum viable set of foundation capabilities, and Phase 2 should prove them under
> real load before further investment is committed.

## Operating model decision point

The choice between the [centralised]({{ site.baseurl }}/03a-Centralized-Operating-Model/)
and [federated]({{ site.baseurl }}/03b-Federated-Operating-Model/) model is normally made
at the end of Phase 1 and revisited during Phase 3. Many enterprises deliberately start
centralised to reach a compliant production scenario quickly, then federate as capability
maturity and guardrail automation improve.

Signals that it is time to federate include a growing intake backlog, domains with the
skills to operate their own agents, and guardrails that are automated rather than
review-based.
