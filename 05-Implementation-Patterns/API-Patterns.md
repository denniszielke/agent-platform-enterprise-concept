---
layout: default
title: API Patterns
parent: Implementation Patterns
nav_order: 5
---

# API Patterns

APIs are the primary interface through which enterprise agents are exposed to users, applications, and other agents. Designing these APIs well—with consistent conventions, robust security, and thoughtful versioning—is essential for building an agent estate that is maintainable, interoperable, and trustworthy. This page covers the API design patterns recommended for the enterprise agent platform, including the specialised considerations that arise from streaming responses, long-running operations, and multi-agent composition.

## Supported Capabilities

| Capability | API Patterns Role |
|---|---|
| **11 – AI Runtimes** | Agent services expose APIs consumed by clients |
| **19 – Tool & MCP Connectivity** | MCP server APIs follow the patterns defined here |
| **20 – Agent & MCP Registry** | APIs are registered and versioned in the registry |
| **7 – Identity & Trust** | API security depends on the identity primitives |
| **22 – Observability** | API calls are traced and metered |
| **5 – Platform Management** | API gateway and management infrastructure |

## API Design Principles

Before selecting a specific pattern, all agent APIs should adhere to these principles:

- **Consistency**: use the same conventions across all agent APIs to reduce the cognitive overhead for consumers.
- **Least disclosure**: APIs expose only the data the consumer requires; sensitive internals are not surfaced.
- **Idempotency**: write operations should be idempotent where possible (accept an `Idempotency-Key` header for non-idempotent operations to enable safe retries).
- **Versioning**: all APIs are versioned from day one; breaking changes require a new version.
- **Discoverability**: APIs are documented in OpenAPI format and published to the platform's API catalogue.

## Pattern 1 – Synchronous HTTP/REST

The standard pattern for request/response interactions with an agent.

**Request shape**:
```http
POST /v1/agents/{agent_id}/invoke
Authorization: ******
Content-Type: application/json
Idempotency-Key: <client-generated-uuid>

{
  "session_id": "sess_abc123",
  "messages": [
    { "role": "user", "content": "Summarise the Q3 sales report." }
  ],
  "context": {
    "user_id": "u_xyz",
    "locale": "en-GB"
  }
}
```

**Response shape**:
```http
HTTP/1.1 200 OK
X-Trace-Id: trace_def456
X-Agent-Version: 1.2.3
X-Model-Used: gpt-4o-mini
X-Cost-Estimate-USD: 0.0042

{
  "session_id": "sess_abc123",
  "message": { "role": "assistant", "content": "..." },
  "finish_reason": "stop",
  "usage": { "input_tokens": 512, "output_tokens": 256 }
}
```

**Key conventions**:
- Include `X-Trace-Id` in every response to support distributed tracing.
- Report `usage` in every response to support client-side cost tracking.
- Use standard HTTP status codes; include a structured error body for non-2xx responses.

## Pattern 2 – Streaming Response (SSE)

For interactive experiences, stream the completion using Server-Sent Events.

```http
POST /v1/agents/{agent_id}/invoke
Accept: text/event-stream

data: {"type":"content_delta","delta":"The Q3 sales ","session_id":"sess_abc123"}
data: {"type":"content_delta","delta":"report shows...","session_id":"sess_abc123"}
data: {"type":"done","finish_reason":"stop","usage":{"input_tokens":512,"output_tokens":256}}
```

**Considerations**:
- Include a final `done` event with usage and finish reason.
- Emit `heartbeat` events on long pauses to prevent proxy and load balancer timeouts.
- The SSE stream must be terminated on auth failure; do not continue streaming after a token expires mid-response.

## Pattern 3 – Asynchronous Job (Long-Running Operations)

For agent invocations that may take minutes, use the async job pattern: the client receives a job ID immediately and polls or subscribes for completion.

```http
POST /v1/agents/{agent_id}/jobs
→ 202 Accepted
  Location: /v1/agents/{agent_id}/jobs/job_ghi789
  { "job_id": "job_ghi789", "status": "queued" }

GET /v1/agents/{agent_id}/jobs/job_ghi789
→ 200 OK
  { "job_id": "job_ghi789", "status": "running", "progress": 0.4 }

GET /v1/agents/{agent_id}/jobs/job_ghi789
→ 200 OK
  { "job_id": "job_ghi789", "status": "completed", "result": { ... } }
```

**Alternatively**, clients may register a webhook URL at job creation time; the platform delivers a `POST` to the webhook on completion, avoiding polling.

{: .note }
> Provide both polling and webhook delivery; enterprise environments often cannot expose inbound webhook endpoints, making polling the only viable option.

## Pattern 4 – OpenAI-Compatible API

Agents that serve as model endpoints should expose an OpenAI-compatible API (`/v1/chat/completions`). This enables agent-as-model composition: an orchestrator can treat a specialised sub-agent as a model endpoint, using standard client libraries.

**Benefits**:
- Zero-cost adoption of new client frameworks and tools.
- Sub-agents can be swapped for actual model endpoints (and vice versa) without consumer changes.
- The Model Gateway natively supports this pattern.

**Considerations**:
- Declare agent-specific extensions (e.g., `agent_id`, `session_id`) as additional fields in request/response bodies; standard fields must retain standard semantics.

## Pattern 5 – MCP Tool API

MCP servers expose tools following the MCP wire protocol. Tool APIs differ from REST in that they are discoverable at runtime by agents.

**MCP tool schema example**:
```json
{
  "name": "create_incident",
  "description": "Creates an incident ticket in the ITSM system.",
  "inputSchema": {
    "type": "object",
    "properties": {
      "title": { "type": "string", "maxLength": 200 },
      "severity": { "type": "string", "enum": ["P1","P2","P3","P4"] },
      "description": { "type": "string", "maxLength": 2000 }
    },
    "required": ["title", "severity"]
  }
}
```

**Governance requirements for MCP tool APIs**:
- Tool descriptions must accurately describe what the tool does and its side effects.
- Input schemas must declare all required fields; the server must validate all inputs.
- Tools with side effects (writes, sends, deletions) must be clearly distinguished from read-only tools.

## API Versioning Strategy

| Scenario | Approach |
|---|---|
| Non-breaking additive change (new optional field) | No version bump required; document in changelog |
| Breaking change (removed field, changed semantics) | Increment major version; maintain previous version for deprecation period |
| Deprecation | Announce with `Deprecation` and `Sunset` response headers; minimum 90-day deprecation window |
| Experimental API | Use `/experimental/` path prefix; no stability guarantees |

## API Security

All agent APIs must implement:

- **Authentication**: ****** (validated against the platform identity broker)
- **Authorisation**: the caller's identity must be authorised to invoke the specific agent
- **Rate limiting**: enforced at the API gateway layer; return `429 Too Many Requests` with `Retry-After` header
- **Input size limits**: reject requests exceeding configured limits; return `413 Content Too Large`
- **CORS**: for browser-facing APIs, restrict origins to approved domains
- **TLS**: all APIs exposed over HTTPS only; minimum TLS 1.2, prefer 1.3

## API Documentation Standards

Every agent API must be documented with:

- OpenAPI 3.1 specification published to the platform API catalogue
- Authentication requirements and token scopes
- Example requests and responses for all operations
- Error codes and their meaning
- Rate limits and usage quotas
- Deprecation and versioning policy
- Contact information for the owning team

## Summary

Consistent API patterns—synchronous REST for interactive use, streaming for real-time UX, async jobs for long-running workloads, and OpenAI-compatible interfaces for agent composition—provide a coherent foundation for the enterprise agent estate. Combining strong versioning discipline, documented security requirements, and published OpenAPI specifications ensures that agent APIs remain maintainable and trustworthy as the platform scales.
