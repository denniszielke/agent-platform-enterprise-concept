---
layout: default
title: Capability to Architecture Mapping
parent: Architecture Concept
nav_order: 8
---

# Capability to architecture mapping

This mapping is the join between *what* the enterprise committed to and *where* it is
realised. It is the page to review when a new component is proposed: if it does not
realise a capability, it needs a justification; if a capability has no component, it is a
gap.

## Mapping table

| # | Capability | Architecture location | Primary owner (centralised) | Primary owner (federated) |
| --- | --- | --- | --- | --- |
| 1 | Billing and commercial management | Hub — FinOps service | Central | Central |
| 2 | Resource organisation | Hub — landing zone service | Central | Central |
| 3 | Roles and access | Hub — identity service | Central | Central definition, domain assignment |
| 4 | Network topology | Hub — shared network services | Central | Central |
| 5 | Platform management | Hub — platform automation | Central | Central |
| 6 | Resilience | Hub and spoke — per service tier | Central | Shared |
| 7 | Identity and trust | Hub — control plane | Central | Central policy, domain provisioning |
| 8 | AI runtime protection | Hub gateway + spoke runtime | Central | Central defaults, domain runtime |
| 9 | Governance and compliance | Hub — policy service | Central | Central policy, domain enforcement |
| 10 | Lifecycle automation | Hub templates, spoke pipelines | Central | Central templates, domain pipelines |
| 11 | AI runtimes | Spoke | Central | Domain |
| 12 | Workflow orchestration | Spoke | Central | Domain |
| 13 | Agent memory | Spoke stores under hub policy | Central | Domain |
| 14 | User experience integration | Spoke | Central | Domain |
| 15 | Model gateway | Hub | Central | Central |
| 16 | Semantic foundation | Hub — AI platform | Central | Central standards, domain assets |
| 17 | Knowledge and data platform | Spoke data products, hub catalogue | Central | Domain |
| 18 | Evaluation engineering | Hub harness, spoke datasets | Central | Central harness, domain datasets |
| 19 | Tool and MCP connectivity | Hub broker, spoke tools | Central | Central broker, domain tools |
| 20 | Agent and MCP registry | Hub | Central | Central |
| 21 | Enterprise capability marketplace | Hub | Central | Central platform, domain contributions |
| 22 | Observability | Hub collection, spoke emission | Central | Central platform, domain dashboards |
| 23 | AI FinOps | Hub | Central | Central platform, domain budgets |
| 24 | Enterprise AI enablement operating model | Enterprise-wide | Central | Shared governance forum |

## How to read the mapping

- **Hub** means the component is shared and must not be duplicated per domain.
- **Spoke** means the component is instantiated per domain or per scenario.
- **Hub + spoke** means a shared definition with distributed enforcement or execution —
  the pattern that makes federation safe.

{: .highlight }
> Note how few rows actually change between the two operating models. The architecture is
> stable; the ownership column is the operating-model decision.

## Using the mapping

1. Baseline each capability's maturity in the
   [capability maturity model]({{ site.baseurl }}/02-Enterprise-Capabilities/Capability-Maturity-Model.html).
2. Confirm each capability has exactly one accountable owner in this table.
3. Flag capabilities with no realising component as roadmap items.
4. Review new components against the table before they are approved.
5. Re-check the ownership column whenever the operating model is revisited.
