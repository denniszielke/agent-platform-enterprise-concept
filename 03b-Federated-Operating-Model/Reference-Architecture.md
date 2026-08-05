---
layout: default
title: Reference Architecture
parent: Federated Operating Model
nav_order: 2
---

# Reference Architecture

The federated reference architecture defines how the 24 platform capabilities are partitioned between a central platform team and multiple autonomous domain teams. The central team provides a stable, governed **platform plane** — shared infrastructure, model gateway, identity, observability backbone, and guardrails. Domain teams build on top of this plane, operating their own agent workloads, domain-specific knowledge sources, and tool integrations within the boundaries the platform enforces.

![Federated operating model]({{ site.baseurl }}/assets/diagrams/federated-operating-model.svg)

*Figure 1 — Federated operating model: shared platform plane at the centre with domain workloads around the hub.*

![Hub and spoke architecture]({{ site.baseurl }}/assets/diagrams/hub-and-spoke.svg)

*Figure 2 — Hub-and-spoke topology: the central platform is the hub; each domain team is a spoke with its own agent workloads, indexes, and tooling connecting through the central plane.*

---

## Architecture Layers

### Central Platform Plane (Hub)

The hub provides shared services that every domain team consumes. These services are provisioned and operated by the central platform team.

#### Shared Infrastructure

| Component | Description |
|---|---|
| Hub virtual network | Central egress, firewall, DNS, and Private DNS Zones shared by all domain spokes |
| Identity plane | Entra ID tenant, managed identity infrastructure, workload identity federation, Key Vault hierarchy |
| Central monitoring | OpenTelemetry collector cluster, Log Analytics workspace, Prometheus metrics aggregation |
| Model gateway | Centralised API management layer fronting all approved model endpoints — routing, rate limiting, content filtering, cost metering |
| Governance engine | Azure Policy, OPA, CI/CD policy gates — enforced for all domain deployments |
| Agent & MCP registry | Central registry of all approved agents, tool servers, and capabilities |
| Platform developer portal | Internal portal with golden paths, SDK documentation, self-service intake |

#### Platform Team Responsibilities

- Provision and update the shared infrastructure on defined release cadences.
- Author and publish governance policies; enforce them automatically.
- Operate the model gateway, identity plane, and central registry.
- Maintain the golden path library and SDK.
- Aggregate observability and cost data from all domain spokes.
- Run the enterprise AI enablement programme.

### Domain Team Spokes

Each participating domain team operates an independent **spoke** — a set of Azure subscriptions, resource groups, or namespaces (depending on isolation model chosen) that are peered to the hub network and subject to the platform's policy scope.

#### Domain Spoke Components

| Component | Owner | Notes |
|---|---|---|
| Domain agent runtime | Domain team | Containerised or serverless agent workloads using approved runtimes |
| Domain orchestration | Domain team | Orchestration logic (LangGraph, Semantic Kernel, custom) using platform templates |
| Domain knowledge index | Domain team | Domain-specific vector index, RAG pipeline, document corpus — governed by platform data policy |
| Domain memory store | Domain team | Short-term context cache and long-term memory — using approved storage services |
| Domain observability view | Domain team | Scoped dashboards over the central telemetry pipeline — domain team cannot see other domains |
| Domain tool servers | Domain team | MCP-compatible tool servers registered in the central registry before use |
| Domain UX integration | Domain team | Teams app, web app, or API integration built and operated by the domain team |

---

## Network Topology

The hub-and-spoke network topology ensures that all inter-service traffic flows through the central hub:

- **Domain spokes** are connected to the hub via VNet peering (Azure) or equivalent VPC/account sharing (AWS, GCP).
- **Private Endpoints** for all shared platform services (model gateway, Key Vault, Log Analytics) are deployed in the hub and accessible from all spoke networks.
- **Domain agent workloads** cannot communicate with each other's spokes directly — cross-domain agent invocations must pass through the central agent registry and API gateway.
- **Internet egress** is routed through the central firewall, enabling enterprise-wide egress policy enforcement.

{: .highlight }
> Cross-domain agent-to-agent calls must pass through the central registry and API gateway. This ensures that every cross-domain invocation is authenticated, authorised, logged, and subject to content policies — preventing shadow integrations between domain agents.

---

## Identity and Trust Across Domains

Identity in the federated model extends the [centralized identity model](../03a-Centralized-Operating-Model/Identity-Model.md) with domain-specific considerations:

- **Domain managed identities** — Each domain agent workload has its own user-assigned managed identity, scoped to the resources in its domain spoke. It cannot access resources in other domain spokes without explicit RBAC grant.
- **Cross-domain delegation** — When a domain agent needs to call a capability owned by another domain, it requests a scoped delegation token from the central identity service. The token grants only the specific permission needed and is recorded in the audit log.
- **Platform service mesh** — Domain agents authenticate to central platform services (model gateway, registry, central knowledge sources) using their managed identity. The platform enforces domain-level rate limits and quotas per identity.
- **Domain-level Key Vault** — Each domain spoke has its own Key Vault for domain-specific secrets, connected to the central certificate authority and rotation infrastructure.

---

## Deployment Model

Domain teams deploy agents via the **platform's GitOps pipeline**, which enforces policy gates regardless of who triggers the deployment:

```
Domain team PR (agent config, prompt, tool bindings)
  → Policy scan (secrets, approved base images, required tags)
  → Automated evaluation gate (quality, safety, latency benchmarks)
  → Security scan (dependency vulnerabilities, SAST)
  → Platform team auto-approval (if all gates pass)
  → Staging environment (domain spoke staging namespace)
  → Domain team acceptance test
  → Production deployment (domain spoke production namespace)
  → Registry update (agent registered / version bumped)
  → Observability validation (trace coverage check)
```

The pipeline is owned and operated by the central platform team. Domain teams do not have direct access to production infrastructure — they submit agent configurations and the pipeline enforces every gate automatically. This means domain teams can deploy autonomously without a platform team ticket, while the platform team retains governance through the automated gates.

---

## Data Architecture

The federated model supports domain-owned data while enforcing enterprise data governance:

| Layer | Central Platform | Domain Team |
|---|---|---|
| **Data catalogue** | Purview-based enterprise catalogue with lineage and classification | Domain data stewards register domain datasets; sensitivity labels applied centrally |
| **Knowledge indexes** | Shared embedding model endpoint and vector database infrastructure | Domain team builds and manages domain-specific vector indexes using platform tooling |
| **Retrieval pipeline** | Standard chunking, embedding, and reranking templates published as golden paths | Domain team configures pipeline for their documents; platform policy enforces data classification rules |
| **Cross-domain retrieval** | Central retrieval federation layer for approved cross-domain queries | Domain team registers which indexes are accessible to other domains |
| **Data lineage** | All ingestion and retrieval events logged to central Purview | Domain team tags ingestion jobs with required lineage metadata |

---

## Capability Marketplace

The federated model enables a rich internal capability marketplace that grows as more domains contribute:

- Domain teams register tools, MCP servers, and agent capabilities in the central registry.
- Other domain teams can discover and request access to these capabilities via the developer portal.
- Access grants are governed by the platform's RBAC model — the providing domain team approves consumers.
- The central team reviews all marketplace entries for security and quality before they are discoverable enterprise-wide.

This creates a network effect: as more domains contribute capabilities, the total productive value available to every domain increases, reducing duplication and accelerating delivery across the enterprise.

---

## Technology-Neutral Applicability

The hub-and-spoke reference architecture is applicable across cloud environments:

| Layer | Azure Example | AWS Equivalent | GCP Equivalent |
|---|---|---|---|
| Hub network | Hub VNet + Azure Firewall | Transit Gateway + Network Firewall | Shared VPC + Cloud Armor |
| Identity | Entra ID + Managed Identity | IAM + EKS Pod Identity | Workload Identity Federation |
| Model gateway | APIM + Azure OpenAI | API Gateway + Bedrock | Apigee + Vertex AI |
| Observability | Azure Monitor + Log Analytics | CloudWatch + X-Ray | Cloud Monitoring + Cloud Trace |
| Policy enforcement | Azure Policy + OPA | SCP + OPA | Organization Policy + OPA |

The central principle — a governed hub providing shared services to autonomous domain spokes — is vendor-agnostic and applies equally to hybrid or multi-cloud environments.
