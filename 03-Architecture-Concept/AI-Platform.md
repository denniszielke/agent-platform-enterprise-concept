---
layout: default
title: AI Platform
parent: Architecture Concept
nav_order: 4
---

# AI platform

The AI Platform gives projects governed, stable and observable access to approved models
and AI services while retaining central control of deployment, capacity, safety and
technical lifecycle. It primarily realises the model gateway capability and supplies
shared evaluation and usage-evidence services consumed by Agent Projects.

In the Microsoft Cloud realisation described by this concept, Microsoft Foundry accounts
and projects provide the centre of gravity for model deployments, capacity and evaluation
artefacts. The model gateway and platform contracts keep those services governed and
replaceable without exposing raw model endpoints to every project.

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

A logical model can route only to an approved compatibility class. Deployments in that
class have an equivalent API contract, modality, minimum context, residency class,
safety baseline and tested task behavior. A different model or version is compatible
only after project regression tests meet declared quality, latency and cost tolerances;
gateway policy never infers compatibility from a similar product name.

## Model gateway contract

Projects receive a private endpoint, Microsoft Entra audience, approved logical model
names, quotas and an SLO rather than direct access to regional deployments. Workload
identities authenticate to the gateway. Gateway policy authorizes the project and model,
records token usage, and selects an eligible deployment using health, capacity and
residency constraints.

Fallback is bounded and declared before production. Data-zone or regional restrictions
override availability, and a caller can determine the correlation identifier while
telemetry records the resolved model version, region and fallback reason.

## Prompt and instruction assets

System prompts, tool descriptions, few-shot examples and output schemas are versioned
artefacts under source control with owners, review and release history. They change
behaviour as significantly as code does, so the Agent Project versions them with its
code and applies the same release and evaluation gates. The AI Platform can publish
templates and common test packs but does not own scenario behavior.

## Safety and usage evidence

The AI Platform applies baseline input and output controls through gateway policy and
Azure AI Content Safety where appropriate. Agent Projects add scenario-specific
validation. Every trace records the applied policy version so a safety decision can be
reconstructed without logging sensitive prompt or response content by default.

Gateway and Foundry telemetry attributes model usage, latency, errors, throttling,
safety events and cost to project, environment and registered agent. User or channel
attribution is included only where permitted by privacy policy.

## Evaluation engineering

Evaluation is shared between the platform and each project:

- A shared harness that any team can invoke from a pipeline.
- Common safety, authorization, tool-abuse and reliability suites.
- Project-owned business scenarios and datasets with versioning and ownership.
- Standard metrics for groundedness, task success, safety and regression.
- Release gates with published thresholds per risk tier.
- Production monitoring that feeds failures back into the offline datasets.

{: .warning }
> An agent without an evaluation suite cannot be safely changed. Treat the suite as part
> of the definition of done for the first release, not as a later improvement.

## Interaction with the rest of the architecture

The AI Platform depends on the [Agent Control Plane](Agent-Control-Plane.md) for policy and
identity, on the [Data Platform](Data-Platform.md) for grounding, and it serves the
[Agent Project](Agent-Project.md), which composes these services into
business scenarios. Keeping these boundaries clean is what allows a model, a retrieval
strategy or an agent framework to be replaced independently.
