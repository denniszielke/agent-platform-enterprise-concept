---
layout: default
title: Landing Zone Patterns
parent: Implementation Patterns
nav_order: 6
---

# Landing Zone Patterns

A landing zone is a pre-configured, governed environment into which workloads—including AI agents—are deployed. It establishes the network topology, identity foundations, policy baselines, logging infrastructure, and cost management guardrails before the first workload arrives, ensuring that every agent deployed to the platform inherits a consistent security and operational baseline. Landing zone design is among the most consequential architectural decisions in the enterprise agent platform because it shapes the blast radius of security incidents, the economics of multi-tenancy, and the operational burden of governance at scale.

![Hub and spoke topology]({{ site.baseurl }}/assets/diagrams/hub-and-spoke.svg)
*Hub and spoke topology: a central hub landing zone hosts shared services (identity, gateway, observability, policy); spoke landing zones host domain-specific agent workloads.*

## Supported Capabilities

| Capability | Landing Zone Role |
|---|---|
| **4 – Network Topology** | Core: VNet/subnet architecture, private endpoints, peering |
| **2 – Resource Organisation** | Management group and subscription hierarchy |
| **3 – Roles & Access** | RBAC structure across landing zones |
| **5 – Platform Management** | Policy baselines enforced at landing zone level |
| **6 – Resilience** | Zone-redundant and region-redundant deployment patterns |
| **1 – Billing & Commercial Management** | Billing scopes aligned to landing zone boundaries |
| **10 – Lifecycle Automation** | IaC templates and CI/CD pipelines for landing zone provisioning |

## Core Landing Zone Components

Every AI platform landing zone should include these foundational components, provisioned before any agent workload:

| Component | Purpose |
|---|---|
| Virtual network with defined subnets | Network isolation; controlled connectivity to hub and internet |
| Private endpoints for platform services | Model gateway, key vault, storage, registries accessible without public internet |
| Network security groups / firewall rules | Deny-by-default; permit only required flows |
| Managed identity configuration | Default managed identities for agent runtimes |
| Key vault instance | Secrets, certificates, and keys for the landing zone's workloads |
| Log Analytics workspace | Centralised log destination for the landing zone (optionally federated to hub) |
| Policy assignments | Compliance and security policies applied at landing zone scope |
| Budget alerts | Cost guardrails configured before any spend begins |
| Tag inheritance policy | Mandatory tags enforced at resource creation |

## Hub and Spoke Topology

The hub-and-spoke pattern is the recommended topology for most enterprise agent platform deployments. It separates shared platform services (hub) from domain-specific agent workloads (spokes).

### Hub Landing Zone

The hub hosts shared, platform-wide services:

- **Model Gateway**: the single ingress for all model inference requests
- **Agent & MCP Registry**: catalogue of approved agents and tools
- **Identity broker**: workload identity issuance and validation
- **Observability pipeline**: central telemetry collection and routing
- **Policy engine**: central policy store and evaluation
- **Shared vector infrastructure**: high-capacity embedding and retrieval services

The hub is managed by the platform team. Changes to hub services go through a platform change management process.

### Spoke Landing Zones

Each spoke hosts a domain team's agent workloads:

- Isolated virtual network peered to the hub (one-way peering: spokes can reach hub services; spokes cannot reach each other by default)
- Domain-specific model gateway configuration (routes through hub gateway)
- Domain-specific knowledge indexes and data stores
- Agent runtime clusters (Container Apps, AKS namespace, or equivalent)
- Domain-specific secrets and key vault
- Domain-level RBAC assignments

```
Hub (Platform Team)
├─ Model Gateway
├─ Identity Broker
├─ Observability Pipeline
└─ Policy Engine
     │ VNet peering
     ├── Spoke: HR Domain
     │    ├─ HR Agent Runtime
     │    ├─ HR Knowledge Index
     │    └─ HR MCP Servers
     ├── Spoke: Finance Domain
     │    ├─ Finance Agent Runtime
     │    └─ Finance Data Platform
     └── Spoke: Customer Service Domain
          ├─ CS Agent Runtime
          └─ CS Knowledge Index
```

## Centralized vs. Federated Landing Zone Models

| Dimension | Centralized | Federated |
|---|---|---|
| Spoke Provisioning | Platform team provisions spokes for domain teams | Domain teams self-service provision spokes using approved IaC templates |
| Network Policy | Central team controls all firewall rules and peering | Domain teams manage spoke-internal rules; hub-spoke peering controlled by centre |
| Compliance Baseline | Single baseline applied to all landing zones | Central baseline + domain-specific extensions |
| Cost Allocation | Single billing account with internal charge-back | Domain subscriptions under enterprise management group; each domain owns its billing |
| Change Management | All changes go through central CAB | Hub changes go through central CAB; spoke changes through domain teams |

See [Centralized Operating Model](../03a-Centralized-Operating-Model/Overview.md) and [Federated Operating Model](../03b-Federated-Operating-Model/Overview.md).

## Network Connectivity Patterns

### Private Endpoints

All platform services (model gateway, vector stores, key vaults, container registries) should be accessible only via private endpoints. This ensures:

- Traffic does not traverse the public internet
- DNS resolution returns private IP addresses within the VNet
- Network security groups can enforce fine-grained access control

### Egress Control

Agent runtimes should not have unrestricted internet egress:

- Route all egress through a centralised firewall or egress proxy
- Maintain an allowlist of approved external destinations (model API endpoints, approved SaaS connectors)
- Log all egress flows for security monitoring
- Block egress to data exfiltration-risk categories (file sharing, personal email)

### Cross-Spoke Communication

By default, spokes cannot communicate directly. Cross-domain agent collaboration should route through:

- **Hub-mediated API calls**: sub-agents in different spokes are called via registered API endpoints, routed through the hub
- **Shared message bus**: event-driven integration via a hub-hosted message broker
- **Explicit peering** (exception basis): direct spoke-to-spoke peering with documented justification and security review

## Multi-Region Patterns

For resilience and data residency requirements, landing zones may be deployed across multiple regions:

| Pattern | Description | Use Case |
|---|---|---|
| **Active-active** | Workloads run simultaneously in two or more regions; traffic distributed by load balancer | Highest availability; near-zero RTO |
| **Active-passive** | Primary region handles all traffic; secondary is warm standby | Balanced cost and resilience |
| **Data residency isolation** | Specific data types processed only in approved regions | Regulatory requirements (GDPR, data localisation) |
| **Disaster recovery region** | Cold standby; activated only on DR declaration | Cost-optimised; higher RTO/RPO acceptable |

## Infrastructure as Code

Landing zones must be provisioned exclusively through Infrastructure as Code (IaC):

- Use Bicep, Terraform, or Pulumi templates maintained in version control
- Store landing zone templates in a central platform repository; domain teams fork or inherit
- Apply peer review and automated policy checks to all IaC changes
- Drift detection: continuously compare deployed state against IaC and alert on unauthorised changes
- Test IaC changes in a non-production environment before applying to production landing zones

{: .note }
> Manual changes to landing zone resources should be prohibited by policy except in break-glass emergency scenarios, which must be documented and reviewed post-incident.

## Landing Zone Maturity Progression

| Maturity Stage | Characteristics |
|---|---|
| **Foundation** | Manual provisioning; basic network isolation; identity and key vault in place |
| **Managed** | IaC-provisioned; policy baselines applied; observability integrated; budget alerts active |
| **Automated** | Self-service spoke provisioning via portal/API; drift detection; automated compliance reporting |
| **Optimised** | Dynamic scaling; cost optimisation automation; cross-region resilience; continuous compliance validation |

## Summary

A well-designed landing zone is the silent enabler of everything else on the agent platform. By establishing network boundaries, identity foundations, policy baselines, and cost controls before workloads arrive, the landing zone ensures that every agent inherits a consistent, governed operating environment—reducing the per-agent governance overhead and enabling domain teams to move fast within safe boundaries.
