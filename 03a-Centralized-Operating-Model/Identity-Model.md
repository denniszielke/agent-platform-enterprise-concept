---
layout: default
title: Identity Model
parent: Centralized Operating Model
nav_order: 4
---

# Identity Model

Identity is the foundational security control for the enterprise agent platform. In the centralized operating model, the platform team owns the full identity architecture: from how human users authenticate to agent endpoints, to how agent workloads prove their identity to downstream services, to how multi-agent chains establish and propagate trust. A consistent, well-designed identity model eliminates the need for static secrets, enables end-to-end audit trails, and is a prerequisite for the governance and observability capabilities.

---

## Identity Principals

The platform recognises four classes of identity principal:

| Principal Type | Description | Authentication Mechanism |
|---|---|---|
| **Human user** | Enterprise employees, contractors, and partners invoking agents via UX integrations or APIs | Entra ID (OAuth 2.0 / OIDC), MFA enforced, conditional access policies applied |
| **Agent workload** | A deployed agent process that calls models, tools, memory, and other agents | Managed identity (system-assigned or user-assigned); no static credentials |
| **MCP tool server** | A service that exposes tools to agents via the Model Context Protocol | Workload identity federation; OAuth 2.0 client credentials |
| **Platform service** | Internal platform components (model gateway, evaluation harness, observability pipeline) | Managed identity; service principal with minimal RBAC scope |

{: .highlight }
> Every principal on the platform must authenticate using a managed, short-lived credential. Static API keys, connection strings, and service account passwords in configuration files are prohibited and flagged by the secrets scanning gate in the CI/CD pipeline.

---

## Identity Architecture

### Human User Authentication

Human users authenticate to the platform through Entra ID (or an equivalent enterprise identity provider). The platform enforces:

- **Multi-factor authentication (MFA)** for all users with access to agent management, model configuration, or sensitive data connectors.
- **Conditional access policies** that evaluate device compliance, network location, and risk signals before granting tokens.
- **Role-based access control (RBAC)** scoped to the minimum permissions required — e.g., a business analyst can invoke agents but cannot modify system prompts or approve deployments.
- **Privileged Identity Management (PIM)** for platform team members who require elevated access to production infrastructure — access is just-in-time and time-limited.

### Agent Workload Identity

Agent workloads use managed identities to authenticate to Azure and third-party services:

- **System-assigned managed identity** is used for single-instance agents with a 1:1 relationship to the underlying compute resource.
- **User-assigned managed identity** is used for agents that scale across multiple instances or that share identity with related workloads (e.g., a family of agents serving the same business domain).
- All managed identities are registered in the platform's identity inventory and reviewed quarterly for unused permissions.

Managed identities obtain short-lived tokens from the identity provider at runtime. These tokens are automatically rotated; no operator action is needed.

### Workload Identity Federation

For agent workloads that call external services not natively integrated with managed identities (e.g., third-party SaaS APIs, on-premises systems), **workload identity federation** is used:

1. The agent workload presents its managed identity token to the platform's token exchange service.
2. The token exchange service validates the token and issues a scoped credential for the target external service.
3. The external credential is never stored; it is fetched at runtime for each session.

### Agent-to-Agent Trust

In multi-agent scenarios where a primary agent delegates tasks to specialist sub-agents, trust must be propagated without granting excessive permissions:

- The orchestrating agent passes a **scoped delegation token** to sub-agents. This token is derived from the original user's token and carries only the permissions needed for the delegated task.
- Sub-agents cannot escalate permissions beyond the scoped token. Any attempt to call a resource not covered by the token is blocked by the API gateway.
- The full delegation chain is recorded in the distributed trace, enabling forensic reconstruction of which agent performed which action on whose behalf.

---

## Secret Management

All secrets required by the platform (third-party API keys, database connection strings, certificate private keys) are stored in Azure Key Vault:

| Secret Type | Storage | Access Pattern | Rotation |
|---|---|---|---|
| Third-party API keys | Key Vault secret | Agent reads at startup via managed identity; cached in memory for session duration | Automated rotation via Key Vault + platform-managed rotation jobs |
| TLS certificates | Key Vault certificate | Automatically renewed and deployed to services | Automated (90-day certificates, renewed at 60%) |
| Signing keys (JWT) | Key Vault key | Referenced by ID; cryptographic operations performed in Key Vault | Annual rotation, orchestrated by platform team |
| Database credentials | Key Vault secret | Read at startup; connection pool held in memory | 30-day rotation via managed rotation pipeline |

Secrets are never written to logs, environment variables visible to child processes, or configuration files committed to source control. The CI/CD pipeline includes a secrets scanning step that fails the build if a potential secret is detected in any committed file.

---

## Trust Boundaries and Zero Trust Principles

The centralized identity model implements Zero Trust principles:

- **Verify explicitly** — Every request to every platform service is authenticated and authorised, even between internal components. There is no "trusted network zone" that bypasses identity checks.
- **Least privilege** — RBAC assignments grant the minimum permissions required. Broad roles (e.g., Contributor at subscription level) are not permitted for agent workloads.
- **Assume breach** — Network controls, identity controls, and observability are layered. A compromised agent workload can access only the resources explicitly granted to its managed identity.

### Permission Scopes

| Role | Permitted Actions | Prohibited Actions |
|---|---|---|
| Agent consumer (human) | Invoke deployed agents, read agent outputs | Modify agent configuration, access model endpoints directly |
| Domain developer | Submit agent configurations via GitOps, view logs for their domain | Approve production deployments, modify platform policies |
| Platform engineer | Deploy and configure platform infrastructure | Approve regulatory exceptions (requires separate compliance role) |
| Security & compliance | Author and approve policies, review audit logs, manage exceptions | Deploy agent workloads |
| Platform SRE | Access production infrastructure for incident response (JIT) | Modify identity policies without change management approval |

---

## Identity Observability

All identity events are streamed to the centralized observability pipeline:

- Token issuance and validation events
- Permission check outcomes (allow and deny)
- PIM activations and deactivations
- Failed authentication attempts and anomalous access patterns

Alerts are configured for:
- Any agent workload accessing a resource outside its approved scope
- Multiple failed authentication attempts from the same managed identity
- Token issued to a managed identity that has been decommissioned in the registry
- Anomalous access time (e.g., agent workload accessing data stores outside business hours without a documented maintenance window)

---

## Relationship to Governance and Observability

The identity model is deeply integrated with the [Governance Model](./Governance-Model.md) (which defines access policies enforced by the identity layer) and the [Observability Model](./Observability-Model.md) (which captures identity events as part of the end-to-end trace). Changes to the identity model must be reviewed by both the security & compliance team and the platform SRE team before deployment.
