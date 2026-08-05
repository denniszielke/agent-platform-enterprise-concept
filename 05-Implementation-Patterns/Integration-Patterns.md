---
layout: default
title: Integration Patterns
parent: Implementation Patterns
nav_order: 4
---

# Integration Patterns

Enterprise agents deliver value by acting on enterprise systems—reading from and writing to ERP, CRM, ITSM, data warehouses, communication platforms, and custom internal APIs. Integration patterns govern how agents connect to these systems safely, reliably, and in a way that preserves the enterprise's security and governance posture. This page describes the principal integration patterns available on the platform, spanning tool-based integration via MCP, event-driven integration, and workflow-level integration with enterprise systems of record.

## Supported Capabilities

| Capability | Integration Patterns Role |
|---|---|
| **19 – Tool & MCP Connectivity** | Core: the protocol layer through which agents call enterprise systems |
| **20 – Agent & MCP Registry** | Catalogue of approved integrations available to agents |
| **21 – Enterprise Capability Marketplace** | Reusable integration building blocks shared across teams |
| **12 – Workflow Orchestration** | Long-running integrations managed as durable workflow steps |
| **7 – Identity & Trust** | Integration credentials bound to agent identity |
| **4 – Network Topology** | Network path from agent runtime to backend system |
| **22 – Observability** | Integration call telemetry |

## Model Context Protocol (MCP)

MCP is the open standard for exposing enterprise tools and data sources to agents in a structured, discoverable way. An MCP server wraps an enterprise system and exposes:

- **Tools**: functions the agent can call (e.g., `create_ticket`, `query_database`, `send_email`)
- **Resources**: data the agent can read (e.g., documents, records, configuration)
- **Prompts**: reusable prompt templates for common operations

### MCP Integration Lifecycle

```
1. Author MCP server wrapping target system
2. Register server in Agent & MCP Registry (Capability 20)
3. Governance review: tool schema, access scope, data classification
4. Approve and publish to the Enterprise Capability Marketplace (Capability 21)
5. Agent developers discover and declare dependency on MCP server
6. At runtime: agent acquires scoped token → calls MCP server → server calls backend
```

### MCP Security Model

| Control | Implementation |
|---|---|
| Authentication | Agent presents scoped token to MCP server; server validates before accepting calls |
| Authorisation | MCP server maps agent identity to backend system permissions |
| Input validation | MCP server validates all tool arguments against declared JSON schema |
| Output filtering | Sensitive fields in responses are masked based on agent's data access scope |
| Audit | Every MCP tool call is logged with agent identity, tool name, arguments (sanitised), and result status |

{: .note }
> Never trust MCP tool arguments as safe inputs to backend systems. An agent's prompt may have been influenced by malicious content (indirect prompt injection). Always validate and sanitise MCP tool arguments server-side before passing them to backend APIs or databases.

## Pattern 1 – Synchronous REST/HTTP Integration

The most common pattern: the MCP server or agent runtime makes direct HTTP calls to a REST API.

**When to use**: real-time data reads, low-latency writes, CRUD operations on enterprise records.

**Considerations**:
- Route all HTTP egress through the platform's egress proxy for logging and filtering.
- Implement timeout and retry logic (idempotent retries for GET; conditional retries for POST).
- Use connection pooling; avoid per-request TCP connection establishment.
- Prefer synchronous calls only for operations that complete within 5–10 seconds; use asynchronous patterns for longer operations.

## Pattern 2 – Event-Driven Integration

The agent subscribes to an event stream and reacts to business events (record created, status changed, threshold breached).

**When to use**: change notifications, monitoring and alerting agents, data synchronisation pipelines.

**Implementation**:
- Use enterprise message brokers (Azure Service Bus, Apache Kafka, AWS SQS) as the integration layer.
- Agent runtime implements an event consumer; processes each event as a new agent invocation.
- Implement idempotency: use event IDs to detect and skip duplicate deliveries.
- Configure dead-letter queues with alerting for events that cannot be processed.

## Pattern 3 – Database Integration

Agents query enterprise databases directly via the MCP tool layer.

**When to use**: SQL analytics, report generation, operational data lookups.

**Considerations**:
- MCP database tools must enforce read-only access for agents that do not require write permissions.
- Use parameterised queries exclusively; never construct SQL from unvalidated agent output.
- Apply row-level and column-level security at the database layer; agent identity is passed as a session variable where the database supports it.
- Limit result set size in MCP tool definitions to prevent agents from extracting bulk data.

## Pattern 4 – Filesystem and Document Integration

Agents read enterprise documents from content management systems, SharePoint, OneDrive, blob storage, or document databases.

**When to use**: document analysis, policy lookup, contract review.

**Considerations**:
- Retrieve documents through the knowledge platform (Capability 17) rather than direct filesystem access where possible; this ensures classification labels and access controls are applied.
- For direct blob storage access, use SAS tokens or managed identity scoped to the specific container and operation.
- Implement a document scanning step: validate MIME type, check file size, and scan for malware before processing.

## Pattern 5 – Async Long-Running Integration

Some enterprise systems expose long-running operations (batch jobs, approval workflows, data exports). The agent initiates the operation and polls or subscribes for completion.

**When to use**: ERP batch jobs, data export requests, approval workflows.

**Implementation using Durable Workflow**:
```
1. Agent calls MCP tool: initiate_job(params) → returns job_id
2. Durable workflow suspends with external event handler
3. Backend system calls platform webhook OR platform polls backend
4. Webhook/poll receives completion event → resume workflow
5. Agent calls MCP tool: get_job_result(job_id) → processes result
```

## Pattern 6 – Enterprise Capability Marketplace

Reusable integration building blocks (MCP servers, connectors, workflow templates) are published to the Enterprise Capability Marketplace (Capability 21) for discovery and reuse across teams.

**Governance of marketplace assets**:
- Every marketplace asset must have an owner, version, documentation, and a support contact.
- Assets must pass security review before publication.
- Deprecation notices must be published at least 90 days before an asset is withdrawn.
- Usage metrics (which agents consume which assets) should be tracked for impact analysis.

## Centralized vs. Federated Integration Governance

| Dimension | Centralized | Federated |
|---|---|---|
| MCP Server Authorship | Central integration team authors all MCP servers | Domain teams author MCP servers for their systems; central team reviews |
| Registry Management | Central team manages all registry entries | Domain teams manage their entries; central team governs standards |
| Marketplace Curation | Central team curates and publishes all assets | Domain teams publish; central team quality-gates before promotion to "approved" tier |
| Cross-domain Integration | Central team designs and implements | Domain teams negotiate interfaces; central team mediates disputes |
| Security Review | Central security team reviews all integrations | Domain security leads review; central team audits a sample |

See [Centralized Operating Model](../03a-Centralized-Operating-Model/Overview.md) and [Federated Operating Model](../03b-Federated-Operating-Model/Overview.md).

## Integration Observability

Integration calls are a significant source of latency, errors, and cost. Instrument every integration:

- Emit a span for each MCP tool call, including target system, tool name, and duration.
- Record error codes and retry counts.
- Alert on sustained error rates or latency regressions for critical integrations.
- Track call volume per integration to identify dependency concentration risk.

## Summary

Integration patterns—anchored in MCP for tool connectivity, supplemented by event-driven and async patterns for complex enterprise workflows—provide a structured, governed approach to connecting agents with enterprise systems. Registering all integrations in the platform registry and publishing reusable assets to the marketplace accelerates development and ensures consistent security and observability standards across the agent estate.
