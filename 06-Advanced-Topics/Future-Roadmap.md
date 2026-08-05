---
layout: default
title: Future Roadmap
parent: Advanced Topics
nav_order: 4
---

# Future Roadmap

The enterprise agent platform is a living system operating in one of the fastest-moving areas of technology. This page describes the strategic directions that platform teams and enterprise architects should anticipate and plan for, grounded in the 24 capabilities established in this documentation. The roadmap is not a commitment or a release schedule—it is a structured view of how the platform is likely to evolve and what investments are worth making today to preserve optionality as the landscape changes.

## Supported Capabilities

| Capability | Future Roadmap Relevance |
|---|---|
| **24 – Enterprise AI Enablement Operating Model** | The operating model must evolve as the platform matures |
| **18 – Evaluation Engineering** | Evaluation practices must scale to cover more complex agent behaviours |
| **15 – Model Gateway** | Gateway must accommodate new model types, modalities, and protocols |
| **9 – Governance & Compliance** | Regulatory landscape is rapidly evolving |
| **12 – Workflow Orchestration** | Long-horizon and agentic workflows will become more prevalent |
| **21 – Enterprise Capability Marketplace** | Marketplace grows in value as the ecosystem expands |

## Horizon 1 – Consolidation and Scale (Near Term)

The immediate priority for most enterprise platforms is consolidating the foundational capabilities and proving them at scale. Key near-term investments:

### Platform Hardening

- Complete the implementation of all 24 capabilities described in this documentation
- Establish production-grade observability across the full agent estate
- Implement automated compliance evidence collection (see [Compliance](../04-Cross-Cutting-Topics/Compliance.md))
- Achieve consistent landing zone provisioning for all domain teams

### Evaluation Engineering Maturity

Early AI programmes often have ad-hoc or absent evaluation practices. The near-term roadmap should establish:

- Domain-specific evaluation datasets and rubrics for every production agent
- Automated evaluation gates in CI/CD pipelines
- Regression alerting for quality metric degradation in production
- Benchmarking infrastructure for model upgrade decisions

### FinOps Maturity

Progress from the "inform" phase (cost visibility) to the "optimise" phase (see [FinOps](../04-Cross-Cutting-Topics/FinOps.md)):

- Prompt prefix caching deployed across all production agents
- Model tier routing implemented in the gateway
- Per-agent budget enforcement operational
- Quarterly FinOps reviews embedded in the operating cadence

## Horizon 2 – Expanded Capability (Medium Term)

As foundational capabilities mature, the platform expands into more sophisticated territory.

### Multi-Modal Agents

Current agent platforms are predominantly text-based. The near-future includes:

- **Vision-enabled agents**: process images, charts, diagrams, and documents as part of workflows.
- **Audio integration**: voice-to-agent interfaces; meeting transcription and summarisation agents.
- **Code and data agents**: agents that generate, execute, and validate code in sandboxed environments as part of business workflows.

The Model Gateway must evolve to route multi-modal requests, and the observability pipeline must handle richer telemetry payloads.

### Long-Horizon Autonomous Agents

Current production agents are typically bounded in scope—a single task, a defined set of tools, a time-limited execution. The platform will need to support agents that:

- Operate over days or weeks, persisting state across sessions
- Self-direct their own tool selection and sub-task decomposition
- Pause and resume around human approval checkpoints
- Manage their own resource consumption within platform-defined budgets

This requires advances in durable workflow infrastructure, memory management, and human oversight tooling.

### Agent-to-Agent Communication Standards

As agent estates grow, inter-agent communication becomes a significant architectural concern. The platform should invest in:

- Standardised agent-to-agent message formats (building on MCP or emerging standards)
- Agent capability discovery: an agent can query the registry for other agents that can help with a given task
- Delegated trust: when one agent invokes another, identity chain integrity must be maintained (see [Agent Identity](../04-Cross-Cutting-Topics/Agent-Identity.md))
- Cross-organisation agent federation: calling agents in partner or supplier organisations under controlled trust relationships

### Model Fine-Tuning and Distillation Pipelines

As organisations accumulate proprietary data and domain expertise, the case for fine-tuning grows:

- Managed fine-tuning pipelines with data governance gates (see [Data Governance](../04-Cross-Cutting-Topics/Data-Governance.md))
- Distillation workflows: use frontier model outputs to train smaller, cost-efficient domain models
- Continuous retraining triggered by evaluation score degradation
- Fine-tuned model governance: automated safety evaluation before production promotion

## Horizon 3 – Strategic Evolution (Longer Term)

These directions carry higher uncertainty but warrant early consideration to avoid architectural lock-in.

### Reasoning Models and Planning

Emerging model architectures with explicit reasoning capabilities (chain-of-thought, test-time compute scaling) change the cost-quality frontier. The platform should:

- Support configurable reasoning effort at the Model Gateway level
- Track reasoning token consumption separately for accurate cost attribution
- Update evaluation frameworks to assess reasoning quality, not just final answers

### Regulatory Evolution

The regulatory landscape for AI is maturing rapidly:

| Framework | Anticipated Evolution |
|---|---|
| **EU AI Act** | Delegated acts filling in technical standards; enforcement guidance from national authorities |
| **GDPR and AI** | Increasing regulatory attention on automated decision-making and AI-generated content |
| **Sector-specific AI rules** | Financial services, healthcare, and critical infrastructure regulators publishing AI-specific guidance |
| **International AI governance** | Bilateral and multilateral AI governance frameworks affecting cross-border deployments |

The compliance programme (see [Compliance](../04-Cross-Cutting-Topics/Compliance.md)) must include horizon scanning for regulatory change and translate emerging requirements into platform controls before they become mandatory.

### Open Standards Convergence

Several open standards are emerging that will shape platform interoperability:

- **MCP maturation**: Model Context Protocol is already widely adopted; expect further standardisation of authentication, streaming, and resource types.
- **Agent Network Protocol**: emerging work on standardised agent-to-agent communication and capability advertisement.
- **AI safety standards**: technical standards bodies (ISO, NIST, BSI) are developing AI safety and evaluation standards that will influence governance requirements.

The platform should track these standards and participate in relevant working groups where the organisation has material interest.

### Centralised vs. Federated Evolution

As both operating models mature, the distinctions may blur:

- Centralised platforms will delegate more to domain teams as platform capabilities mature.
- Federated models will converge on stronger central standards as regulatory requirements increase.
- Hybrid models—strong central governance with federated execution—are likely to become the dominant pattern for large enterprises.

## Operating Model Evolution

The Enterprise AI Enablement Operating Model (Capability 24) must itself evolve:

| Maturity Stage | Characteristics |
|---|---|
| **Emerging** | Platform team small; centralised decision-making; ad-hoc demand management |
| **Scaling** | Dedicated platform team; self-service for common patterns; community of practice active |
| **Mature** | Platform product management; domain teams largely self-sufficient; central team focused on standards and complex problems |
| **Leading** | Platform as a competitive differentiator; active contribution to open standards; AI capabilities embedded in all major business processes |

## Investment Priorities

For teams planning their roadmap, the following sequence of investments tends to deliver the best return on governance and capability:

1. **Foundation first**: complete all 24 capabilities before pursuing advanced patterns.
2. **Evaluation before scale**: establish evaluation practices before deploying agents to large user bases.
3. **FinOps in the first quarter**: cost visibility and optimisation pay back quickly.
4. **Compliance in parallel with capability**: retroactive compliance remediation is more expensive than building it in.
5. **Ecosystem before custom**: leverage the growing ecosystem before building bespoke integrations.

{: .note }
> The most durable platform investments are those that increase optionality: the Model Gateway abstracts model providers, MCP abstracts tool surfaces, and the landing zone abstracts infrastructure. Investments that lock the platform to a specific vendor or model should require explicit architectural review.

## Summary

The enterprise agent platform roadmap spans three horizons: consolidation and scale in the near term, expanded capability through multi-modality and long-horizon agents in the medium term, and strategic evolution around reasoning models, open standards, and regulatory maturation in the longer term. Planning for each horizon now—while focusing execution on the near term—ensures the platform retains architectural flexibility as the AI landscape continues to evolve rapidly.
