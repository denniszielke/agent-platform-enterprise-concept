---
layout: default
title: Cost and Usage Capabilities
parent: Enterprise Capabilities
nav_order: 8
---

# Cost and usage capabilities

Cost and usage capabilities span billing and commercial management (1), resource
organisation (2), the metering function of the model gateway (15) and AI FinOps (23).
Together they answer a question executives ask early and repeatedly: *what does this cost,
per what, and is it worth it?*

## The measurement chain

Cost transparency is only possible if every link in the chain holds:

1. **Tagging** — resources carry scenario, owner and cost-centre tags, enforced by policy
   at provisioning time.
2. **Metering** — the gateway emits per-call records with model, tokens, latency,
   scenario and calling identity.
3. **Correlation** — telemetry links model calls, tool calls and infrastructure usage to
   a single interaction.
4. **Aggregation** — records roll up to scenario, domain and enterprise views.
5. **Allocation** — costs are shown back or charged back to the accountable budget.

A break in any link turns cost reporting into an estimation exercise, and estimates lose
arguments with finance.

## Cost drivers

| Driver | Effect | Typical lever |
| --- | --- | --- |
| Model selection | Order-of-magnitude differences per call | Route simple tasks to smaller models |
| Context size | Retrieval and history dominate token cost | Trim context, tune chunk size and top-k |
| Agent loop depth | Multi-step reasoning multiplies calls | Step limits, better tool descriptions |
| Tool call volume | Downstream API and data cost | Caching, batching |
| Retrieval infrastructure | Index and query cost scales with corpus | Lifecycle rules on stale content |
| Idle capacity | Reserved capacity unused off-peak | Mix provisioned and on-demand capacity |

## Unit economics

Absolute spend is a poor decision input. Useful units are cost per completed task, cost
per resolved conversation, or cost per document processed — always paired with a value
measure such as handling time avoided or revenue influenced.

{: .highlight }
> Track unit cost as a trend from the first production scenario. A scenario whose unit
> cost falls as volume grows is scaling; one whose unit cost is flat is just spending
> more.

## Controls

- **Quotas** per scenario and per environment, enforced at the gateway.
- **Budgets** with alerts at defined thresholds and an agreed action when exceeded.
- **Guardrails on experimentation** — non-production environments get smaller quotas and
  cheaper default models.
- **Anomaly detection** on consumption patterns, since a looping agent can generate a
  significant bill in hours.

## Allocation models

| Model | Description | Best suited to |
| --- | --- | --- |
| Central absorption | Platform absorbs all cost | Early phases, to encourage adoption |
| Showback | Costs reported to domains, not billed | Transition phase, building awareness |
| Chargeback | Costs billed to domain budgets | Mature federated operation |
| Hybrid | Platform absorbs shared services, domains pay consumption | Most common steady state |

The hybrid model is usually the right steady state: it funds the paved road centrally so
using it is attractive, while making consumption a domain decision with a domain
consequence.

See [Cost Transparency]({{ site.baseurl }}/04-Cross-Cutting-Topics/Cost-Transparency.html)
for implementation detail.
