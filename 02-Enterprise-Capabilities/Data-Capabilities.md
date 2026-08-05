---
layout: default
title: Data Capabilities
parent: Enterprise Capabilities
nav_order: 6
---

# Data capabilities

Data capabilities cover the Intelligence area: model gateway (15), semantic foundation
(16), knowledge and data platform (17) and evaluation engineering (18). Agent quality is
bounded by the quality, governance and reachability of enterprise knowledge — most
disappointing agent pilots are data problems wearing a model costume.

## Model gateway (15)

A single governed entry point for model access provides routing across model families and
regions, quota and rate limiting per consumer, safety enforcement, caching, failover and
— critically — metering that makes consumption attributable.

![Model gateway request flow]({{ site.baseurl }}/assets/diagrams/model-gateway.svg)
*Every model call is authenticated, policy checked, safety filtered, routed and metered.*

The gateway is also the abstraction that makes model choice reversible. When a new model
version is released, teams change a routing rule rather than redeploying every agent.

## Semantic foundation (16)

Shared meaning is what allows agents built by different teams to talk about the same
business concepts. The semantic foundation includes a business glossary, ontologies or
knowledge graphs for key entities, embedding standards, and metadata that describes
sensitivity and ownership. Without it, each agent invents its own definition of
"customer", "order" or "incident", and answers stop reconciling across scenarios.

## Knowledge and data platform (17)

Grounding content should be delivered as governed data products rather than as ad-hoc
copies:

| Property | Why it matters for agents |
| --- | --- |
| Named owner | Someone is accountable for correctness and change |
| Permission trimming | Retrieval respects source system access rights |
| Freshness contract | Users can trust the recency of an answer |
| Lineage | An answer can be traced back to its source |
| Retention and deletion | Deleted source content disappears from agent answers |
| Quality signals | Poor content is detected before it degrades answers |

Retrieval patterns — keyword, vector, hybrid, graph-augmented and structured query —
are selected per scenario. Chunking, metadata enrichment and citation generation are
platform concerns because getting them wrong produces confident, unsourceable answers.

{: .warning }
> Copying data into an agent-specific index is the fastest way to lose permission
> trimming, retention and lineage all at once. Prefer governed data products with
> security trimming applied at query time.

## Evaluation engineering (18)

Evaluation is the capability that keeps quality from drifting as models, prompts, tools
and data change.

- **Offline evaluation** against curated datasets with expected behaviours, run as a
  release gate.
- **Safety evaluation** covering harmful content, injection resistance and data leakage.
- **Regression evaluation** on every prompt, tool or model change.
- **Online monitoring** of production signals: user feedback, escalation and abandonment
  rates, citation coverage and answer refusal patterns.
- **Human review** sampling for high-risk scenarios.

Evaluation datasets are enterprise assets. They should be versioned, owned, and reused
across scenarios that share a domain.

## Operating model differences

| Capability | Centralised | Federated |
| --- | --- | --- |
| Model gateway (15) | Central, mandatory for all traffic | Central, mandatory for all traffic |
| Semantic foundation (16) | Central team curates | Central standards, domain-contributed assets |
| Knowledge platform (17) | Central ingestion and indexing | Domains publish data products centrally registered |
| Evaluation (18) | Central evaluation service | Central harness, domain-owned datasets |

The model gateway stays central in both models: it is the one place where consistent
metering, safety and routing can be guaranteed.
