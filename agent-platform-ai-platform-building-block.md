# AI Platform Building Block

## Purpose

The AI Platform provides the governed model and AI service access layer for the Enterprise Agent Platform. It makes approved models consistently available to agents, coding environments, embedded AI services and agent platforms without requiring each consumer to manage model hosting, regional capacity, credentials, API versions or telemetry independently.

Its core design principle is to separate model consumption from model hosting. Consumers use stable model contracts while the AI Platform team manages the underlying deployments, capacity, regions, lifecycle and controls. This allows model infrastructure to evolve without forcing every agent or application team to redesign its integration.

Within the six-building-block model, the AI Platform owns the supply and governed consumption of model capabilities. It does not own agent runtimes, business workflows, enterprise data products or the lifecycle registry for agents and tools.

## Capability alignment

The AI Platform is the primary realisation of **capability 15, Model Gateway & AI Access Platform**. It shares responsibility for **capability 18, Evaluation, Benchmarking & Quality Engineering**, by providing common evaluation tooling and model-level evidence while project teams own scenario acceptance. It also contributes enforcement and telemetry to **capability 8, AI Security, Trust & Runtime Protection**, **capability 22, Observability, Telemetry & Evaluation Platform**, and **capability 23, AI FinOps & Cost Management Platform**.

The building block depends on the Cloud Platform for resource hierarchy, identity, network connectivity, policy and resilience; on lifecycle automation for repeatable deployment; and on the Agent Control Plane for enterprise inventory, policy and governance state.

## Boundaries with the other building blocks

| Building block | Interface with the AI Platform | Boundary |
| --- | --- | --- |
| Cloud Platform | Supplies subscriptions, policy, private networking, DNS, identity foundations, resilience patterns and cost-management primitives | The AI Platform configures model-specific services within those guardrails; it does not create a separate cloud foundation |
| Application Platform | Hosts agents, applications and integration components that consume model contracts; may provide API-management runtime capabilities used by the gateway | The AI Platform owns model-access policy, routing and service levels; the Application Platform owns general workload hosting and integration |
| Data Platform | Supplies governed data products, semantic assets and retrieval interfaces used by AI solutions | The AI Platform may provide embedding models and evaluation integration, but it does not own project knowledge stores, retrieval quality or enterprise semantics |
| Agent Control Plane | Supplies registration, risk state, ownership metadata and enterprise policy used in access decisions | The AI Platform enforces model-specific access and emits evidence; it does not own the lifecycle of agents, tools or MCP servers |
| Agent Project | Selects and evaluates model contracts, then consumes them from application or agent code | The project owns prompts, business behaviour, scenario evaluation, data use and outcome accountability |

## What the AI Platform contributes

The AI Platform contributes a reusable model-access service with four outcomes:

1. **Discover approved models.** Developers and project teams can find supported models, capabilities, modalities, intended uses, regions, data-processing constraints, quotas and lifecycle status through a curated catalogue.
2. **Evaluate through self-service.** Developers can subscribe to approved models for experimentation and comparative evaluation without requesting a bespoke deployment. The platform provides bounded quotas, developer identities, usage visibility, common evaluation tooling and a clear path from testing to production.
3. **Consume models in production.** Deployed agents and AI services receive stable, private and identity-based endpoints with production quotas, resilience policies, telemetry and service commitments.
4. **Manage shared concerns once.** Capacity, authentication, rate limits, cost attribution, model telemetry, safety controls and model lifecycle are implemented as platform capabilities instead of being rebuilt by every project.

## Consumers

The model-access contract supports four primary consumer types:

| Consumer | Typical need | Platform experience |
| --- | --- | --- |
| Agents | Reasoning, planning, multimodal processing and tool selection | Stable production endpoint, workload identity, quota and trace correlation |
| Coding environments | Model exploration, prompt development, evaluation and prototyping | Searchable catalogue and self-service test subscription with bounded usage |
| Embedded AI services | AI functions integrated into applications, workflows and APIs | Versioned model contract, predictable authentication and usage attribution |
| Agent platforms | Shared model access for managed agent builders and runtimes | Approved model inventory, tenant or project policy, capacity and telemetry integration |

## Core components

The building block consists of:

- **Model catalogue** containing approved models, versions, modalities, compatibility classes, intended uses, regional availability, retirement dates and evaluation guidance.
- **AI gateway** exposing stable logical model endpoints and enforcing the common access contract.
- **Foundry accounts and projects** providing centrally owned model deployment, development and evaluation capabilities with delegated project access where appropriate.
- **Model deployments** distributed across global, data-zone or regional locations using provisioned or pay-as-you-go capacity.
- **Evaluation service and evidence store** providing reusable harnesses, baseline model benchmarks and versioned results that projects can extend with scenario-specific datasets and thresholds.
- **Identity and access integration** using Entra ID, managed identities and scoped developer access. Shared production credentials are not permitted.
- **Telemetry and cost services** attributing usage, tokens, latency, errors and approximate cost to projects, agents and environments.

Azure AI Gateway is the strategic gateway target. Azure API Management can provide the interim or complementary implementation where required. The logical service contract must remain independent of the selected gateway product.

## Model access contract

Consumers integrate with a logical model contract rather than a physical deployment. Each contract declares:

- logical model identifier and supported API or protocol;
- supported modalities, context limits and required request or response schemas;
- approved use tiers, data-processing boundaries and available regions;
- authentication audience, authorization scope and required attribution metadata;
- quota, rate-limit, availability and support tier;
- version policy, compatibility class, fallback behavior and retirement notice period;
- telemetry, content-safety and evaluation obligations.

A stable contract does not imply that model behavior is identical across versions. The platform may move a contract between equivalent deployments without consumer changes, but a materially different model or behavior requires published evaluation evidence, consumer notice and an explicit promotion decision.

## Two model-access paths

### Developer evaluation path

Developers discover approved models in the catalogue and create a self-service test subscription. The subscription provides a stable evaluation endpoint, bounded quota, usage dashboard and access to relevant model documentation. This path is intended for coding environments, prototypes, prompt testing and comparative evaluation. It must be isolated from production capacity and data permissions.

Evaluation should produce evidence for model selection, including task quality, safety, latency and expected unit cost. Promotion to production is an explicit onboarding decision, not an automatic increase of a developer quota.

### Production consumption path

Production agents, embedded AI services and agent platforms use workload identities to call private logical endpoints. The gateway authorizes the project and requested model contract, applies quotas and safety policies, selects an eligible deployment and records the resolved model version. Routing can consider health, capacity, residency and approved compatibility, but it must not silently substitute a materially different model.

The production path provides declared availability, capacity, fallback behavior, support ownership and cost attribution. Applications consume the contract and do not embed raw deployment endpoints, API versions or model keys.

## Evaluation and promotion

Evaluation is a shared responsibility with a deliberate split:

- The AI Platform team benchmarks supported models for baseline quality, safety, latency, throughput and unit cost, and publishes known limitations and compatibility evidence.
- The Agent Project team supplies representative scenario datasets, defines business acceptance thresholds and validates prompts, grounding, tool use and end-to-end behavior.
- Promotion records the approved model contract, resolved version, evaluation evidence, risk tier, quota and accountable owner.
- Model-version changes are tested against the affected compatibility suites before promotion. Projects are notified when their scenario evidence must be rerun.

Platform benchmarks help teams shortlist models; they do not replace scenario-specific acceptance testing.

## AI gateway responsibilities

The AI gateway is a cross-functional control point for model consumption. It provides:

- **Discovery and subscription:** catalogue integration, products, access requests and self-service evaluation subscriptions.
- **Authentication and authorization:** developer identity for testing and managed identity for production.
- **Capacity management:** quotas, backend pools, health checks, throttling signals and controlled failover.
- **Rate and token limits:** policy by project, agent, environment or subscription.
- **Cost management:** consumption attribution and indicative cost reporting; gateway estimates are not billing records.
- **Telemetry:** model, version, tokens, latency, errors, throttling, fallback and correlation identifiers.
- **Lifecycle abstraction:** stable logical names across deployment changes, version promotion and retirement.
- **Guardrails:** baseline content safety and model-access policies, supplemented by scenario-specific controls in each project.

The gateway does not own application behavior, business authorization, prompt quality or project evaluation. It also does not make all model types interchangeable: model compatibility must be demonstrated through evaluation.

## Ownership model

### AI Platform team

The AI Platform team owns the Foundry account structure, approved model catalogue, deployments, capacity strategy, gateway, logical model contracts, shared policies, platform telemetry and service lifecycle. It decides where models are hosted, how capacity is allocated, which compatibility classes are supported and when versions are promoted or retired.

The team also owns the common evaluation harness, baseline model benchmarks, managed-identity integration, private connectivity, onboarding automation, documentation, dashboards and support commitments. Foundry accounts remain part of the shared platform and are not transferred to individual projects.

### Agent and AI project teams

Project teams own their business scenario, agent or application code, prompts, model selection, evaluation evidence, runtime configuration and production behavior. They may receive delegated Foundry project roles and self-service evaluation access, but they do not manage shared accounts or central model deployments.

Projects must validate that the selected model meets their quality, safety, latency and cost requirements. They integrate through prepared logical endpoints and preserve the required telemetry context.

## Service contract to the Agent Platform

For each onboarded project or runtime, the AI Platform provides:

- a discoverable list of approved model contracts;
- a self-service evaluation subscription with bounded quota;
- common evaluation tooling and baseline model evidence;
- a production logical endpoint and Entra audience;
- workload identity and least-privilege authorization guidance;
- declared quota, capacity, residency, fallback and lifecycle behavior;
- token, latency, error, usage and cost-attribution telemetry;
- model version and policy information required for evaluation and audit;
- promotion, deprecation and retirement procedures with notice periods;
- an escalation path for capacity, access, reliability and model lifecycle issues.

This service contract lets the wider Agent Platform offer model access as a prepared capability. Agent teams can focus on solution behavior and measurable outcomes while the enterprise manages model supply, control and evidence once, at platform scale.

## Success measures

The building block is effective when:

- developers can obtain bounded evaluation access without a bespoke model deployment;
- production consumers use logical endpoints and workload identities rather than deployment URLs or shared keys;
- model requests can be attributed to a project, workload, environment and model version;
- production onboarding records scenario evaluation evidence and an accountable owner;
- quota, throttling, availability and cost are visible against the declared service contract;
- model changes and retirements complete within published notice periods without unplanned consumer outages.