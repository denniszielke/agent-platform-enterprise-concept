---
layout: default
title: Identity Model
parent: Federated Operating Model
nav_order: 4
---

# Identity Model

The federated identity model extends the [centralized identity foundations](../03a-Centralized-Operating-Model/Identity-Model.md) to support autonomous domain teams operating their own agent workloads. The central platform team continues to own the identity infrastructure, policy, and audit capability — but domain teams receive delegated rights to provision and manage identities within their spoke boundaries. This model enables domain autonomy without sacrificing the enterprise-wide visibility and control that the governance framework requires.

---

## Identity Architecture Overview

The federated identity model introduces a layered structure:

```
Enterprise Identity Plane (central team)
  ├── Central Entra ID tenant
  ├── Enterprise RBAC roles and assignment policies
  ├── Central Key Vault hierarchy (root CA, platform secrets)
  ├── Workload identity federation service
  └── Identity audit pipeline

Domain Identity Scope (per domain team)
  ├── Domain-scoped user-assigned managed identities
  ├── Domain Key Vault (domain secrets, domain certificates)
  ├── Domain RBAC assignments (within approved role set)
  └── Domain identity inventory (maintained by domain team)
```

The central plane sets the rules; domain teams operate within them. A domain team can create and manage managed identities for their agent workloads, but only within the RBAC role set published by the central team, and only for resources within their domain spoke.

---

## Identity Principals in the Federated Model

| Principal Type | Who Manages | Scope | Constraints |
|---|---|---|---|
| **Human user (enterprise)** | Central identity team | Enterprise-wide | MFA, conditional access, PIM — central policy, non-negotiable |
| **Domain agent workload** | Domain team (within platform guardrails) | Domain spoke resources only | User-assigned managed identity; must be registered in central identity inventory |
| **Cross-domain agent caller** | Central identity service | Scoped delegation only | Delegation token issued per call; cannot be stored or reused |
| **Domain tool server** | Domain team | Domain resources + approved external endpoints | Registered in central MCP registry; workload identity federation |
| **Platform service** | Central team | Platform resources only | Managed identity; no domain resource access |

---

## Domain Identity Provisioning

### Domain Team Rights

Domain teams with platform onboarding certification have the right to:

- Create **user-assigned managed identities** for their agent workloads within their domain resource group.
- Assign approved RBAC roles to their managed identities — limited to the role set in the platform's approved role catalogue.
- Create and manage **domain Key Vault** secrets for domain-specific credentials (third-party SaaS keys, on-premises connection strings).
- Register managed identities in the **central identity inventory** — required before any managed identity can authenticate to platform services.

Domain teams may not:

- Create managed identities with subscription-level or cross-domain RBAC assignments.
- Store secrets in source control or environment variables visible to child processes.
- Use service principals with static client secrets (only certificate-based or federated credentials are permitted).
- Grant their agent workloads access to another domain's resources without a central-team-approved cross-domain access grant.

{: .warning }
> Unregistered managed identities — those not in the central identity inventory — are blocked from authenticating to platform services by the model gateway, central registry, and shared data platform. Domain teams must register all agent identities before deploying to staging.

### Identity Registration Process

When a domain team creates a new agent managed identity, they register it via the developer portal:

1. Domain team creates the managed identity in their resource group via IaC.
2. Identity details (object ID, display name, domain, agent ID, assigned RBAC roles, purpose) are submitted to the central identity inventory via the portal or API.
3. The central identity service validates the registration: checks for naming convention compliance, duplicate object IDs, and role assignments within the approved set.
4. On approval (automated for standard registrations, reviewed for elevated permissions), the identity is added to the inventory and permitted to authenticate to platform services.
5. The registration event is logged in the identity audit pipeline.

---

## Cross-Domain Trust

Cross-domain agent invocations require careful trust management. The federated model implements the following pattern:

### Calling Domain (Requestor)

1. The calling agent requests a **scoped delegation token** from the central identity service, specifying: target agent ID, required capabilities, session context hash, and calling agent's managed identity.
2. The central identity service validates that: the calling agent is registered, the target agent is registered and has public accessibility enabled, and the calling domain has consumer approval from the target domain.
3. A short-lived delegation token (15-minute TTL) is issued, scoped to the specific capabilities needed.
4. The calling agent presents this token to the target agent's endpoint.

### Called Domain (Provider)

1. The target agent's platform SDK validates the delegation token against the central identity service.
2. The SDK enforces that the call context (data access, tool permissions) is limited to what the delegation token authorises.
3. The invocation is logged with the delegation token metadata in the bilateral audit record.

### Why Not Direct Token Passing?

The delegation token model is preferred over passing the original user's token directly to sub-agents because:

- It enforces least privilege at every hop — each agent in a chain can only access what is needed for its specific task.
- It prevents privilege escalation — a compromised sub-agent cannot leverage the caller's full permissions.
- It produces an auditable delegation chain — the full provenance of any data access can be reconstructed from the audit log.

---

## Secret Management in the Federated Model

The federated model extends the centralized secret management pattern with domain-level Key Vaults:

| Level | Key Vault | Owner | Contents |
|---|---|---|---|
| Platform | Central Key Vault | Central platform team | Platform-wide signing keys, root CA, shared API keys |
| Domain | Domain Key Vault (per domain) | Domain team | Domain-specific API keys, domain certificates, domain DB credentials |

Domain Key Vaults are deployed using the platform's IaC module — they inherit the standard access policy, logging configuration, and rotation schedule. Domain teams cannot create Key Vaults outside this module, preventing shadow secret stores.

Rotation schedules:

| Secret Type | Rotation Frequency | Method |
|---|---|---|
| Third-party API keys | 90 days | Automated via Key Vault rotation job + domain team validation |
| Domain certificates | 90 days | Automated renewal (Let's Encrypt or internal CA) |
| Database credentials | 30 days | Automated via rotation job |
| Platform signing keys | Annual | Orchestrated by central platform team |

---

## Identity Observability

The central identity audit pipeline captures all identity events across the enterprise, including domain-level events:

| Event Type | Source | Alert Condition |
|---|---|---|
| Managed identity token issuance | Entra ID | Unregistered identity attempting authentication |
| RBAC assignment change | Azure Activity Log | Assignment outside approved role catalogue |
| Key Vault secret access | Key Vault audit log | Access by an identity not in the domain's identity inventory |
| Cross-domain delegation request | Central identity service | Delegation to a target without consumer approval |
| Failed authentication | Entra ID | 5+ failures from the same identity in 10 minutes |
| Privilege escalation attempt | OPA runtime | Any request for permissions beyond the identity's registered scope |

Domain teams receive scoped views of their domain's identity events via the developer portal. Cross-domain identity events are visible only to the central security team.

---

## Relationship to Governance

Identity is deeply integrated with the federated [Governance Model](./Governance-Model.md). Identity event data feeds the audit pipeline, exceptions to the identity policy must pass through the standard exception management process, and domain team identity inventory reviews are conducted quarterly as part of the governance cadence.
