---
layout: default
title: Runtime Patterns
parent: Implementation Patterns
nav_order: 2
---

# Runtime Patterns

Agent runtimes are the execution environments that host, schedule, and manage the lifecycle of AI agents. Selecting and configuring the right runtime pattern for each workload type—synchronous request/response, long-running workflow, batch processing, or event-driven reaction—is essential for achieving the reliability, scalability, and cost efficiency the enterprise requires. This page describes the principal runtime patterns available on the platform and guidance on when to apply each.

## Supported Capabilities

| Capability | Runtime Patterns Role |
|---|---|
| **11 – AI Runtimes** | Core: defines the runtime environments and deployment topologies |
| **12 – Workflow Orchestration** | Long-running and multi-step agent workflows require workflow runtimes |
| **6 – Resilience** | Runtime resilience patterns: scaling, fault tolerance, health management |
| **5 – Platform Management** | Runtime lifecycle, patching, and configuration management |
| **10 – Lifecycle Automation** | CI/CD pipelines deploy agent images to runtime environments |
| **22 – Observability** | Runtimes instrument and emit telemetry |
| **4 – Network Topology** | Runtimes are deployed in network segments that enforce connectivity policies |

## Runtime Taxonomy

### Pattern 1 – Synchronous Request/Response

The simplest runtime pattern: the agent is deployed as a stateless HTTP service that accepts a request, performs inference (potentially with tool calls), and returns a response within a single connection lifetime.

**Characteristics:**
- Session state is held in-memory for the request duration only
- Scales horizontally behind a load balancer
- Short-lived token validity is sufficient
- Suitable for: chatbots, single-turn question answering, classification, extraction

**Deployment targets**: Azure Container Apps, Kubernetes (standard Deployment), AWS Lambda (with appropriate timeout), Google Cloud Run.

**Considerations:**
- Set gateway and load balancer timeouts generously (60–180 seconds) to accommodate multi-hop tool call chains.
- Use connection pooling to model endpoints rather than per-request connections.

### Pattern 2 – Streaming Response

A variant of Pattern 1 where the model response is streamed to the client using Server-Sent Events (SSE) or WebSocket. Improves perceived latency for users and reduces memory pressure for long completions.

**Characteristics:**
- Requires the agent runtime, gateway, and client to all support streaming
- Backpressure handling is important: slow clients can cause buffer overflows
- Suitable for: interactive chat, real-time code completion, live document generation

### Pattern 3 – Durable Workflow

For multi-step, long-running processes that may span minutes to hours, a durable workflow runtime persists state externally and resumes execution after interruptions.

**Characteristics:**
- Execution state is checkpointed to durable storage (database, blob store)
- Individual steps may be retried independently without restarting the full workflow
- Human-in-the-loop steps (approval, review) are modelled as workflow tasks with async completion
- Suitable for: multi-agent orchestration, document processing pipelines, business process automation

**Technology options**:
- **Dapr Workflow** (open-source, portable)
- **Azure Durable Functions**
- **Temporal** (open-source; strong support for complex orchestration logic)
- **AWS Step Functions** / **Google Workflows**

### Pattern 4 – Event-Driven / Reactive

Agents subscribe to event streams and react to events asynchronously. No persistent connection is held open; the runtime processes each event independently.

**Characteristics:**
- Decoupled producers and consumers; high scalability
- At-least-once delivery semantics require idempotent event handlers
- Dead-letter queues and poison message handling are essential
- Suitable for: change-data-capture pipelines, alert processing, monitoring and remediation agents

**Technology options**: Azure Service Bus / Event Hubs, Apache Kafka, AWS SQS/SNS, Google Pub/Sub.

### Pattern 5 – Batch / Scheduled

Agents process large volumes of data on a schedule or triggered by a dataset becoming available. No real-time latency requirement; optimised for throughput and cost (batch inference pricing).

**Characteristics:**
- Parallelism controlled by job scheduler; horizontal scaling at job level
- Suitable for: nightly report generation, bulk document classification, periodic evaluation runs
- Uses batch inference APIs where available (lower cost per token)

**Technology options**: Azure Batch, Kubernetes Jobs/CronJobs, Apache Spark (for data-scale workloads).

## Runtime Pattern Selection Guide

| Workload Characteristic | Recommended Pattern |
|---|---|
| User-facing, real-time response required | Synchronous / Streaming |
| Multi-step process spanning > 30 seconds | Durable Workflow |
| Triggered by external events, no real-time SLA | Event-Driven |
| Large volume, scheduled, cost-sensitive | Batch |
| Mix of real-time and long-running steps | Synchronous entry + Durable Workflow for async steps |

## Container Image Standards

All agent runtimes should be deployed as container images that meet the platform's image standards:

- Base image from the platform-approved image catalogue (hardened, scanned, patched)
- No root user; non-root UID/GID specified in Dockerfile
- Image signed with Sigstore/Cosign; signature verified at admission
- SBOM generated and stored in the artefact registry
- No embedded secrets; credentials injected via workload identity or vault integration
- Health check endpoints (`/health/live` and `/health/ready`) implemented

## Scaling Patterns

| Scaling Dimension | Pattern |
|---|---|
| **Horizontal pod autoscaling** | Scale on CPU, memory, or custom metrics (queue depth, token throughput) |
| **KEDA (event-driven autoscaling)** | Scale to zero when no events; scale up on queue depth or event rate |
| **Vertical scaling** | Increase memory/CPU for memory-intensive embedding or reasoning steps |
| **Node pool specialisation** | GPU node pools for self-hosted model inference; CPU-optimised pools for orchestration |

{: .note }
> Scale-to-zero is cost-efficient for development and low-traffic agents but introduces cold-start latency. For production agents with SLAs, maintain a minimum replica count to avoid cold starts.

## Centralized vs. Federated Runtime Deployment

| Dimension | Centralized | Federated |
|---|---|---|
| Cluster Ownership | Platform team owns and operates all runtime clusters | Domain teams own clusters; platform team provides reference configurations |
| Base Image Management | Central team publishes and maintains approved base images | Domain teams consume central images; may extend with domain-specific layers |
| Scaling Policy | Central SRE configures scaling for all agents | Domain teams configure their own scaling within platform guardrails |
| Runtime Patching | Central team patches runtime infrastructure | Shared responsibility: platform patches infrastructure, domains patch agent code |

See [Centralized Operating Model](../03a-Centralized-Operating-Model/Overview.md) and [Federated Operating Model](../03b-Federated-Operating-Model/Overview.md).

## Observability Integration

Runtimes must be instrumented from the start:

- Sidecar or init container injects the OpenTelemetry collector configuration.
- Agent framework libraries provide out-of-the-box spans for model calls, tool calls, and memory operations.
- Liveness and readiness probes ensure unhealthy replicas are removed from the load balancer before serving errors.
- Structured JSON logs written to stdout/stderr; collected by the platform log aggregator.

## Summary

Matching the runtime pattern to the workload type—synchronous for interactive use cases, durable workflow for complex multi-step orchestration, event-driven for reactive scenarios, and batch for high-volume scheduled processing—ensures that agent workloads are reliable, cost-efficient, and appropriately observable. Standardising on container images and a consistent observability approach across all patterns reduces operational complexity as the agent estate scales.
