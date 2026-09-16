---
layout: default
title: Model Gateway
parent: Implementation Patterns
nav_order: 1
---

# Model Gateway

The Model Gateway is the centralised ingress point through which all agent model inference requests are routed. It provides a single, observable, policy-enforceable boundary between the agent estate and the diverse set of model endpoints—hosted commercial APIs, self-hosted open-weight models, and fine-tuned variants—that the platform supports. Implementing the Model Gateway as a platform primitive rather than an application-level concern ensures that cost metering, security controls, and routing logic are consistently applied regardless of the agent framework or programming language in use.

![Model gateway request flow]({{ site.baseurl }}/assets/diagrams/model-gateway.svg)
*Request flow through the Model Gateway: agents submit requests; the gateway authenticates, applies policy, routes, meters, and returns responses while emitting telemetry.*

## Supported Capabilities

| Capability | Model Gateway Role |
|---|---|
| **15 – Model Gateway** | Core: implements routing, metering, caching, and policy enforcement |
| **7 – Identity & Trust** | Validates agent tokens before forwarding requests |
| **8 – AI Runtime Protection** | Input/output guardrails applied inline |
| **23 – AI FinOps** | Token metering and budget enforcement |
| **22 – Observability** | Telemetry emission for all requests |
| **9 – Governance & Compliance** | Approved model catalogue enforcement; audit logging |
| **6 – Resilience** | Retry logic, fallback routing, circuit breakers |

## Architecture

The gateway exposes an OpenAI-compatible API surface, allowing agents built on any framework (LangChain, Semantic Kernel, LlamaIndex, custom) to route through it with minimal configuration change. Internally, it translates requests to each backend's native protocol.

```
Agent (any framework)
  │  OpenAI-compatible API
  ▼
┌─────────────────────────────────────────────┐
│              Model Gateway                  │
│  ┌──────────┐  ┌──────────┐  ┌──────────┐  │
│  │ Auth &   │  │ Policy & │  │ Routing  │  │
│  │ Identity │→ │ Guardrail│→ │ Engine   │  │
│  └──────────┘  └──────────┘  └────┬─────┘  │
│  ┌──────────┐  ┌──────────┐       │        │
│  │ Metering │  │ Cache    │       │        │
│  │ & Budget │  │ Layer    │←──────┘        │
│  └──────────┘  └──────────┘                │
└─────────────────────────────────────────────┘
        │           │           │
        ▼           ▼           ▼
   Azure OpenAI  Self-hosted  Third-party
   / OpenAI      Ollama/vLLM  Anthropic, etc.
```

### Authentication and Identity

Every request must carry a valid agent token. The gateway validates:

- Token signature against the platform's identity broker
- Token claims: `agent_id`, `scope`, `aud` (must include the gateway's resource identifier)
- Token freshness (not expired, not revoked)

Requests without a valid token are rejected with `401 Unauthorized` and the rejection is logged.

### Policy and Guardrails

After authentication, the gateway evaluates the request against the approved model catalogue and runtime protection policies:

- **Approved model check**: the requested model must be in the approved catalogue for the requesting agent's use case.
- **Input guardrails**: content safety, prompt injection detection, PII detection (configurable per agent and environment).
- **Topic restrictions**: agents may be restricted to specific topic domains; off-topic requests are rejected with a structured error.
- **Rate limiting**: per-agent token-per-minute and requests-per-minute limits enforced with token bucket algorithms.

### Routing Engine

The routing engine selects the target model endpoint based on:

| Routing Criterion | Description |
|---|---|
| **Model specification** | Agent explicitly requests a model ID |
| **Capability tier** | Agent requests a tier (lightweight/mid/frontier); gateway selects cheapest approved model for that tier |
| **Budget fallback** | Agent approaching budget threshold; gateway routes to lower-cost alternative |
| **Load balancing** | Multiple instances of the same model; round-robin or latency-weighted routing |
| **Geographic routing** | Route to model endpoint in required data residency region |
| **Canary routing** | A fraction of traffic routed to a new model version for A/B evaluation |

### Caching Layer

The caching layer reduces cost and latency for repeated or predictable requests:

- **Prompt prefix caching**: leverages provider-side KV cache for requests with identical system prompt prefixes.
- **Semantic cache**: for read-only, non-sensitive queries, cache by semantic similarity using a lightweight embedding lookup.
- **Exact match cache**: for fully deterministic queries, cache the exact request hash.

Cache hits bypass model inference entirely and are recorded as cost-zero events in the metering pipeline.

### Metering and Budget Enforcement

Every request—whether a cache hit, a routed inference, or a rejected request—produces a cost event:

- `agent_id`, `team`, `cost_centre`
- `model_name`, `input_tokens`, `output_tokens`, `cached_tokens`
- `estimated_cost_usd`
- Budget utilisation percentage (enables budget enforcement logic)

Budget enforcement actions (warn → fallback → reject) are configured per agent in the Agent & MCP Registry.

## Resilience Patterns

The gateway implements resilience patterns to protect agent workloads from model endpoint failures:

- **Retry with jitter**: transient errors are retried with exponential backoff and random jitter.
- **Circuit breaker**: after a configurable failure threshold, the gateway stops sending to a failing endpoint and routes to fallbacks.
- **Fallback routing**: if the primary model endpoint is unavailable, the gateway routes to a secondary endpoint (same or lower tier) with a response header indicating the fallback was used.
- **Timeout enforcement**: gateway-enforced request timeouts prevent long-tail latency from blocking agent pipelines.

{: .note }
> Fallback routing should be explicit in agent design: agents should handle `X-Model-Fallback: true` response headers gracefully, as fallback models may have different capability profiles.

## Deployment Topology

| Topology | Description | When to Use |
|---|---|---|
| **Single shared gateway** | One gateway instance serves all agents | Early maturity; simple governance; risk of single point of failure |
| **Per-environment gateways** | Separate gateways for production, staging, development | Environment isolation; enables different policy configurations |
| **Per-domain gateways** | Federated gateways for each business domain, synchronised to central policy | Federated operating model; domain autonomy with central governance |
| **Edge gateways** | Gateway deployed close to agent runtimes in each region | Latency-sensitive workloads; data residency requirements |

## Observability Integration

The gateway emits structured telemetry to the observability pipeline:

- **Metrics**: request count, latency percentiles, token consumption, cache hit rate, budget utilisation, error rate.
- **Logs**: structured log entry per request with full context (excluding prompt content unless explicitly configured).
- **Traces**: spans for auth, policy evaluation, routing, and backend call, linked to the upstream agent trace.

## Technology Examples

Several open-source and commercial implementations can serve as the foundation for the Model Gateway:

- **Azure API Management** with OpenAI extension: managed gateway with policy engine, caching, and Azure Monitor integration.
- **LiteLLM**: open-source proxy with multi-provider support, OpenAI-compatible API, and built-in cost tracking.
- **OpenRouter**: multi-provider routing with cost optimisation.
- **Portkey / Helicone**: observability-focused gateways with prompt management.

The choice of implementation should be evaluated against the organisation's existing platform investments, operational capabilities, and the depth of policy enforcement required.

## Summary

The Model Gateway transforms what would otherwise be a sprawl of direct model API integrations into a governed, observable, cost-controlled service layer. Deploying it early—before the agent estate grows—establishes the foundation on which FinOps, security, and compliance programmes depend.
