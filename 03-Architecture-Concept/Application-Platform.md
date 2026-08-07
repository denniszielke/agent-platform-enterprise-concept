---
layout: default
title: Application Platform
parent: Architecture Concept
nav_order: 3
---

# Application platform

The Application Platform supplies regional execution and integration services for
containerized agents, MCP servers, APIs and supporting workflows. It gives Agent Projects
a governed deployment path with workload identity, private connectivity, image controls,
ingress, messaging and end-to-end operational telemetry already connected.

## Service contract

| Platform area | Reference implementation | Contract to Agent Projects |
| --- | --- | --- |
| Container runtime | Azure Kubernetes Service or Azure Container Apps | Namespace, ACA environment or dedicated runtime with quota, identity and deployment endpoint |
| Image supply chain | Azure Container Registry, GitHub Advanced Security or Defender for DevOps | Approved repository, image naming convention and immutable release evidence |
| Ingress | Azure Front Door, WAF, Application Gateway, Application Gateway for Containers or ACA ingress | Private service endpoint by default and an approved public hostname where required |
| API and tool gateway | Azure API Management and API Center | Versioned API or MCP publication process and a stable consumer endpoint |
| Workflow and messaging | Logic Apps, Durable Functions, Functions, Service Bus, Event Grid and Event Hubs | Standard workflow, event and queue patterns with dead-letter and replay procedures |
| Runtime operations | Azure Monitor, Application Insights, Log Analytics, managed Prometheus and Managed Grafana | SLO dashboard, logs, traces, alerts and support runbook |

## Runtime selection

Azure Container Apps is the default for stateless APIs, MCP servers, event-driven workers
and agents that do not need Kubernetes-specific controls. Azure Kubernetes Service is
selected when a project requires advanced network policy, custom operators, specialized
scheduling, service mesh, extensive sidecars, high tenancy control or an established
Kubernetes operating model.

Shared runtime tenancy is the normal starting point. Dedicated environments are justified
by regulated isolation, unusual scale, custom platform components or an independent
lifecycle requirement, not by team preference alone.

## Integration and operations

Public traffic terminates only at approved edge and ingress services. Tool, MCP and agent
endpoints are published through a logically separate APIM gateway domain that validates
tokens, restricts operations, applies quotas and records caller, agent, operation and
result class. Direct access to private runtime endpoints is blocked for consumers.

Durable workflows own retries, approvals, long-running process state and compensating
actions. Queues isolate agent latency from downstream systems. OpenTelemetry context is
preserved across gateways, queues and workflows so a project can correlate the user
request, agent, model, retrieval, tool and business outcome.

The Application Platform team owns runtime and integration service commitments. Agent
Projects own their deployed application behavior, configuration, SLO response and
runbooks within those service contracts.
