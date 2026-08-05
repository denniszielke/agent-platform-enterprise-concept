---
layout: default
title: Reference Architecture
parent: Centralized Operating Model
nav_order: 2
---

# Reference Architecture

The centralized reference architecture defines how the 24 platform capabilities are assembled into a coherent, operable system under the control of a single platform team. Every layer of the stack is provisioned, configured, and maintained centrally; business units interact with the platform through well-defined consumer interfaces rather than deploying their own infrastructure.

![Centralised operating model]({{ site.baseurl }}/assets/diagrams/centralized-operating-model.svg)

*Figure 1 — Centralized operating model overview: platform team owns the full stack.*

![Layered architecture]({{ site.baseurl }}/assets/diagrams/layered-architecture.svg)

*Figure 2 — Layered architecture: capabilities are organised from infrastructure foundations at the base through to user-facing agent experiences at the top.*

---

## Architecture Layers

The reference architecture is structured in six layers, each corresponding to a capability domain. Layers depend on those below them; the platform team deploys and stabilises lower layers before exposing higher-layer capabilities to consumers.

### Layer 1 — Enterprise Platform Foundations

This layer provides the non-negotiable infrastructure backbone:

| Capability | Component Examples | Notes |
|---|---|---|
| Billing & commercial management | Azure cost management, commitment-based reservations | Chargeback tags applied at resource creation |
| Resource organisation | Management groups, subscriptions, resource groups, tags | Hierarchy mirrors business unit structure |
| Roles & access | Azure AD / Entra ID, RBAC, PIM, conditional access | Least-privilege, JIT access for sensitive resources |
| Network topology | Hub-spoke VNet, Private Endpoints, Azure Firewall, DDoS | All AI endpoints accessible only via private network |
| Platform management | Azure Policy, Blueprints, Terraform/Bicep IaC, GitOps | Policy-as-code enforced at subscription level |
| Resilience | Availability zones, geo-redundancy, chaos engineering | RTO/RPO targets defined per workload tier |

The network topology is a critical early decision. In the centralized model, all AI runtime traffic, model API calls, and agent-to-tool communications are routed through centrally managed egress points, enabling comprehensive traffic inspection and logging.

### Layer 2 — Governance & Security

| Capability | Component Examples | Notes |
|---|---|---|
| Identity & trust | Managed identities, workload identity federation, certificate authority | Agents never hold static secrets |
| AI runtime protection | Prompt injection detection, content filtering, output validation | Applied at the model gateway layer |
| Governance & compliance | Policy engine, audit log pipeline, regulatory mapping | Maps controls to ISO 27001, SOC 2, GDPR, sector standards |
| Lifecycle automation | Agent registry, approval workflows, deprecation pipelines | No agent reaches production without automated gate checks |

### Layer 3 — Runtime & Experience

| Capability | Component Examples | Notes |
|---|---|---|
| AI runtimes | Azure OpenAI Service, Azure AI Foundry, containerised OSS models | Onboarded and versioned by platform team |
| Workflow orchestration | Azure Logic Apps, Semantic Kernel, LangGraph, Durable Functions | Patterns published as reusable templates |
| Agent memory | Azure Cosmos DB, Redis Cache, Azure AI Search (vector) | Short-term context and long-term episodic memory tiers |
| User experience integration | Copilot Studio, Teams extensions, REST/WebSocket APIs | UX adapters maintained by platform team |

### Layer 4 — Intelligence

| Capability | Component Examples | Notes |
|---|---|---|
| Model gateway | API management layer with routing, rate limiting, fallback | Single entry point for all model calls |
| Semantic foundation | Embedding models, vector databases, chunking pipelines | Shared embedding infrastructure reduces cost |
| Knowledge & data platform | Azure Data Lake, Azure AI Search, Purview lineage | Governed data access with row-level security |
| Evaluation engineering | Evaluation harness, red-teaming tools, regression pipelines | Gate on quality, safety, and latency before promotion |

### Layer 5 — Interoperability

| Capability | Component Examples | Notes |
|---|---|---|
| Tool & MCP connectivity | Model Context Protocol servers, REST connectors, SAP/Salesforce adapters | Centrally vetted and published |
| Agent & MCP registry | Service catalogue, OpenAPI specs, capability metadata | Single source of truth for discoverable agents and tools |
| Enterprise capability marketplace | Internal developer portal, request workflows | Business units discover and request capabilities |

### Layer 6 — Operations

| Capability | Component Examples | Notes |
|---|---|---|
| Observability | Azure Monitor, Log Analytics, distributed tracing (OpenTelemetry), AI-specific metrics | Full trace from user prompt to model response |
| AI FinOps | Cost dashboards, token consumption reports, quota management | Per-agent and per-business-unit cost visibility |
| Enterprise AI enablement | Centre of excellence, training programmes, governance reviews | Demand management and capability roadmap |

---

## Consumer Interface

In the centralized model, business units do not interact directly with the infrastructure. They consume the platform through a defined set of interfaces:

- **Internal Developer Portal** — A self-service catalogue where teams browse available agents, MCP tools, and data connectors. Requests are submitted via a structured intake form.
- **Agent REST API** — Standardised REST and streaming WebSocket endpoints for invoking deployed agents. Authentication uses OAuth 2.0 client credentials backed by Entra ID.
- **SDK & Templates** — Opinionated SDKs (Python, TypeScript, .NET) and agent project templates that embed observability, identity, and policy hooks automatically.
- **GitOps Pipeline** — Domain teams submit agent configuration (prompts, tool bindings, memory settings) as code via pull request. Platform team reviews and merges, triggering automated deployment.

{: .highlight }
> Providing a high-quality self-service developer portal is the single most effective investment a centralized platform team can make to reduce the intake bottleneck. Every capability that a domain team can access without raising a ticket reduces lead time and team frustration.

---

## Data Flow

A typical agentic request follows this path through the centralized architecture:

1. **User** submits a natural language request via a UX integration (Teams, web app, API client).
2. **UX adapter** authenticates the user and passes the request with a bearer token to the agent API gateway.
3. **Agent API gateway** validates the token, applies rate limits and quota checks, and routes to the target agent runtime.
4. **Agent runtime** (e.g., Semantic Kernel, LangGraph) decomposes the request into steps, invoking tools and memory as needed.
5. **Model gateway** receives model completion requests, applies content filters, selects the appropriate model deployment, and enforces token budgets.
6. **Knowledge & data platform** responds to retrieval requests with governed, row-security-filtered results.
7. **MCP tool servers** execute validated tool calls and return structured results to the agent.
8. **Observability pipeline** captures structured telemetry at every step: latency, token counts, tool invocation results, content filter decisions.
9. **Response** is streamed back to the user. The full trace is written to the centralised log store and linked to the cost allocation system.

---

## Security Boundaries

The centralized model enforces strict network and identity segmentation:

- All AI runtime endpoints are exposed only via Private Endpoints — no public internet access.
- Agent workloads run in dedicated compute with no egress to the public internet except through the managed firewall.
- All managed identities are scoped to minimum required permissions and reviewed quarterly.
- Secrets (connection strings, API keys for third-party tools) are stored in Azure Key Vault with access logged and auditable.

---

## Technology-Neutral Applicability

While examples above reference Azure services, the layered architecture is applicable to any major cloud or hybrid environment. Equivalent components exist in AWS (Bedrock, IAM, OpenTelemetry) and GCP (Vertex AI, IAP, Cloud Armor). The key architectural principle — layered capabilities with a central governance plane — is independent of vendor.
