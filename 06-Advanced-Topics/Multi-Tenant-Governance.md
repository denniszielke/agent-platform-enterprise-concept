---
layout: default
title: Multi-Tenant Governance
parent: Advanced Topics
nav_order: 2
---

# Multi-Tenant Governance

Multi-tenancy in the enterprise agent platform arises when the same platform infrastructure serves multiple distinct organisational units, business lines, subsidiary organisations, or external customers. Each tenant requires isolation of their data, workloads, and costs while benefiting from shared platform services. Multi-tenant governance defines how these isolation boundaries are established and maintained, how shared services are safely consumed by multiple tenants, and how platform operators manage the estate without violating tenant boundaries.

## Supported Capabilities

| Capability | Multi-Tenant Governance Role |
|---|---|
| **2 – Resource Organisation** | Management group / subscription hierarchy that creates tenant boundaries |
| **3 – Roles & Access** | RBAC structures that isolate tenant administrators from each other |
| **4 – Network Topology** | Network isolation between tenants |
| **9 – Governance & Compliance** | Per-tenant policy scopes and compliance postures |
| **7 – Identity & Trust** | Tenant-scoped identity issuance and validation |
| **1 – Billing & Commercial Management** | Per-tenant cost attribution and charge-back |
| **23 – AI FinOps** | Cross-tenant cost analysis and optimisation |

## Tenancy Models

The platform supports several tenancy models, each with different isolation characteristics and operational trade-offs:

| Model | Description | Isolation Level | Operational Overhead |
|---|---|---|---|
| **Dedicated subscription per tenant** | Each tenant has an entirely separate Azure subscription (or cloud account) | Highest: full billing, IAM, and network isolation | Highest: each subscription requires independent management |
| **Shared subscription, dedicated resource groups** | All tenants share a subscription; each has dedicated resource groups | Medium: IAM and network isolated; billing requires tagging | Medium: centrally managed subscription, per-tenant resource groups |
| **Shared cluster, namespace isolation** | Agent runtimes share a Kubernetes cluster; tenants are isolated by namespace + RBAC + network policy | Lower: relies on k8s isolation mechanisms | Lower: platform team manages cluster; tenants manage workloads |
| **Fully shared (multi-tenant SaaS)** | Platform provides a fully shared service; tenants identified by token claims | Lowest: application-level isolation only | Lowest: maximum operational efficiency |

Choosing the appropriate tenancy model depends on the regulatory requirements of each tenant, the sensitivity of their data, and the capacity of the platform team to manage additional isolation layers.

## Isolation Dimensions

Effective multi-tenant governance must address isolation across five dimensions:

### 1. Data Isolation

- Each tenant's knowledge indexes, conversation history, and agent memory are stored in tenant-specific containers or databases.
- Row-level security enforces that retrieval queries return only the requesting tenant's data.
- Encryption keys are tenant-specific (envelope encryption with per-tenant key encryption keys).
- Data deletion requests (e.g., GDPR right to erasure) affect only the requesting tenant's data.

### 2. Network Isolation

- Tenants should not be able to reach each other's workloads directly.
- In shared cluster models, Kubernetes Network Policies enforce that pods in tenant A's namespace cannot communicate with tenant B's namespace.
- Shared services (Model Gateway, observability pipeline) are accessible to all tenants but validate tenant identity on every request.

### 3. Identity Isolation

- Each tenant's agents and users authenticate against their own identity namespace within the platform identity broker.
- Cross-tenant impersonation is not possible: a token issued for Tenant A cannot be used to access Tenant B's resources.
- Platform operators have break-glass access to all tenants but this access is logged and subject to review.

### 4. Compute Isolation

| Isolation Level | Implementation | Suitable For |
|---|---|---|
| Process-level (namespace) | Kubernetes namespace + resource quotas | Internal business units with mutual trust |
| Virtual machine-level | Dedicated node pools per tenant | Sensitive workloads; compliance requirements |
| Subscription-level | Separate cloud subscriptions | Regulated industries; external customers |

### 5. Cost Isolation

- Cost attribution requires that every resource a tenant consumes is tagged with the tenant identifier.
- The Model Gateway meters token consumption per tenant and enforces per-tenant budgets.
- Charge-back reports are generated per tenant; each tenant can access only their own cost data.

## Shared Services Governance

Shared services (Model Gateway, identity broker, observability pipeline) serve multiple tenants and must ensure that one tenant cannot affect another's service quality or access another's data:

- **Rate limiting**: per-tenant rate limits prevent a noisy tenant from exhausting shared capacity.
- **Quota management**: token budgets, storage quotas, and API call limits are enforced per tenant.
- **Tenant claim validation**: every request to a shared service must include a validated tenant claim; requests without a valid tenant claim are rejected.
- **Audit log isolation**: audit logs are partitioned by tenant; tenant administrators can access only their own audit logs.

{: .note }
> Shared services should be designed with tenant isolation as a first-class concern from inception. Retrofitting tenant isolation onto a system originally designed for single-tenant use is significantly more expensive and error-prone.

## Onboarding and Offboarding

### Tenant Onboarding

Automating tenant onboarding reduces error and ensures all isolation controls are applied consistently:

1. Provision tenant-specific landing zone resources (subscription / resource group / namespace)
2. Create tenant identity namespace and configure federation with tenant's IdP
3. Assign tenant administrator role to designated tenant contacts
4. Configure per-tenant resource quotas and budgets
5. Provision tenant encryption keys in the key vault
6. Apply tenant-specific compliance policies
7. Send tenant welcome documentation with API endpoints and onboarding guide

### Tenant Offboarding

Offboarding must ensure complete data removal and revocation of access:

1. Suspend tenant identity (prevent new authentication)
2. Notify tenant of data retention period and export options
3. After retention period: delete all tenant data (knowledge indexes, conversation history, agent memory, logs)
4. Revoke all tenant-specific encryption keys
5. Remove tenant resource groups / namespaces
6. Archive the audit log for the required retention period
7. Close billing account and issue final invoice

## Centralized vs. Federated Multi-Tenant Governance

| Dimension | Centralized | Federated |
|---|---|---|
| Tenant Provisioning | Central platform team provisions all tenants | Domain teams provision tenants within their domain under central policy |
| Tenant Support | Central support team handles all tenant issues | Domain teams provide first-line support for their tenants |
| Compliance Posture | Uniform compliance posture applied to all tenants | Tenants may have different compliance requirements; platform accommodates through configurable policies |
| Customisation | Limited customisation; enforced standardisation | Tenants may customise within published extension points |

See [Centralized Operating Model](../03a-Centralized-Operating-Model/Overview.md) and [Federated Operating Model](../03b-Federated-Operating-Model/Overview.md).

## Summary

Multi-tenant governance requires deliberate isolation across data, network, identity, compute, and cost dimensions. The right tenancy model balances isolation requirements against operational overhead. Automating onboarding and offboarding, enforcing per-tenant quotas on shared services, and maintaining rigorous audit trails ensures the platform can serve diverse tenants safely while remaining operationally manageable.
