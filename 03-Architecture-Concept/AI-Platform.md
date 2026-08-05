---
layout: default
title: AI Platform
parent: Architecture Concept
nav_order: 4
---

# AI platform

The AI platform layer supplies intelligence to agents: models, prompt assets, semantic
context and the evaluation machinery that keeps quality measurable. It realises
capabilities 15-18.

![Model gateway request flow]({{ site.baseurl }}/assets/diagrams/model-gateway.svg)
*The gateway is the boundary between agents and model capacity.*

## Model management

Models are managed as a catalogue rather than as individual endpoints. Each entry records
the model family and version, hosting region and data residency, approved use tiers, cost
per unit of consumption, deprecation date and known limitations.

| Concern | Approach |
| --- | --- |
| Version churn | Route by logical model name; change the mapping centrally |
| Capacity | Mix provisioned capacity for baseline load with on-demand for peaks |
| Multi-region | Route by residency requirement first, then latency and availability |
| Deprecation | Publish dates; use evaluation suites to validate migration |
| Cost | Route by task complexity; default to the smallest sufficient model |

## Prompt and instruction assets

System prompts, tool descriptions, few-shot examples and output schemas are versioned
artefacts under source control with owners, review and release history. They change
behaviour as significantly as code does, so they follow the same release and evaluation
gates.

## Semantic foundation

Shared ontologies, glossaries, embedding standards and metadata make agent answers
consistent across domains. Practically this means a single definition of key business
entities, a supported embedding model set with a defined re-embedding process, and
metadata that carries sensitivity and ownership into retrieval results.

## Evaluation engineering

Evaluation is built into the platform, not bolted onto each project:

- A shared harness that any team can invoke from a pipeline.
- Reusable dataset structures with versioning and ownership.
- Standard metrics for groundedness, task success, safety and regression.
- Release gates with published thresholds per risk tier.
- Production monitoring that feeds failures back into the offline datasets.

{: .warning }
> An agent without an evaluation suite cannot be safely changed. Treat the suite as part
> of the definition of done for the first release, not as a later improvement.

## Interaction with the rest of the architecture

The AI platform depends on the [control plane](AI-Control-Plane.md) for policy and
identity, on the [data platform](Data-Platform-Integration.md) for grounding, and it
serves the [agent platform](Agent-Platform.md), which composes these services into
business scenarios. Keeping these boundaries clean is what allows a model, a retrieval
strategy or an agent framework to be replaced independently.
