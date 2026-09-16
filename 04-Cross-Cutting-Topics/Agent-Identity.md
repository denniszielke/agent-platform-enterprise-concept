---
layout: default
title: Agent Identity
parent: Cross-Cutting Topics
nav_order: 1
---

# Agent Identity

Every agent that acts on behalf of a user or a system must carry a verifiable, auditable identity. Without a coherent identity model, enterprise platforms cannot enforce least-privilege access, trace actions back to an originating principal, or demonstrate compliance to auditors. Agent Identity is therefore one of the most foundational cross-cutting concerns on the platform.

![Agent identity chain]({{ site.baseurl }}/assets/diagrams/agent-identity.svg)
*The agent identity chain: human principal → delegated agent token → sub-agent → tool call, each link carrying verifiable claims.*

## Supported Capabilities

| Capability | How Agent Identity Applies |
|---|---|
| **7 – Identity & Trust** | Core: defines the credential formats, issuers and trust anchors for agent principals |
| **1 – Billing & Commercial Management** | Charge-back requires attributing model spend to an agent identity |
| **8 – AI Runtime Protection** | Policy engines use identity claims to enforce per-agent rate limits and guardrails |
| **10 – Lifecycle Automation** | CI/CD pipelines issue workload identities to agents at promotion time |
| **22 – Observability** | Logs, traces and audit records are attributed to a specific agent principal |
| **23 – AI FinOps** | Cost dashboards aggregate spend by agent identity and owning team |

## Identity Primitives

Enterprise agents operate within a layered identity model. Three principal types exist in practice:

- **Human-delegated identity** – a user authenticates through the enterprise IdP (e.g., Entra ID, Okta) and grants a scoped token to an agent. The agent acts within the user's entitlements but cannot exceed them.
- **Workload identity** – the agent itself holds a managed or federated credential (e.g., a Managed Identity, a Kubernetes service account with OIDC federation, or a signed JWT issued by the platform's workload identity broker). No long-lived secrets are stored in code or configuration.
- **System-to-system identity** – background batch agents that run without a human session use a dedicated service principal with tightly scoped roles and a short token lifetime.

### Credential Formats

| Format | Typical Use | Notes |
|---|---|---|
| OAuth 2.0 Access Token (JWT) | Human-delegated flows | Short-lived; should include `agent_id` and `on_behalf_of` claims |
| OIDC Federated Credential | Workload identity (K8s, GitHub Actions) | No secret rotation required |
| X.509 mTLS Certificate | High-assurance service mesh | Pinned to agent deployment; revocable via CRL or OCSP |
| Platform-issued Agent Token | Internal agent-to-agent calls | Signed by platform CA; carries capability scopes |

## The Identity Chain

When an orchestrator spawns sub-agents, each hop must preserve the identity chain rather than flatten it. A compliant implementation embeds the full delegation path in every outbound token:

1. User authenticates → receives session token.
2. Orchestrator agent exchanges session token for a delegated agent token (carrying `sub`, `aud`, `agent_id`, `scope`, `parent_agent_id`).
3. Sub-agent receives delegated token; cannot request broader scopes than the parent held.
4. Tool calls (including MCP-connected tools) receive a further-scoped token valid only for that tool's resource identifier.

{: .note }
> The chain-of-delegation model mirrors OAuth 2.0 Token Exchange (RFC 8693). Platforms should implement or adopt an identity broker that validates the entire delegation graph before issuing downstream tokens.

## Centralized vs. Federated Operating Models

| Dimension | Centralized Model | Federated Model |
|---|---|---|
| Identity Issuance | A single platform identity service issues all agent credentials | Each domain issues its own credentials against a shared trust anchor |
| Role Assignment | Central IAM team manages agent role bindings | Domain teams manage their own agent principals within guardrails |
| Token Validation | One authoritative token validation endpoint | Federated validation; each domain trusts the shared CA |
| Audit Trail | Unified audit log aggregated centrally | Domain logs federated to a central SIEM |
| Risk | Single point of control simplifies governance | Broader blast radius requires stronger boundary controls |

See [Centralized Operating Model](../03a-Centralized-Operating-Model/Overview.md) and [Federated Operating Model](../03b-Federated-Operating-Model/Overview.md) for how identity governance is operationalized in each model.

## Agent Registration

Before an agent can acquire credentials it must be registered in the **Agent & MCP Registry** (Capability 20). Registration captures:

- Canonical agent identifier (stable across versions)
- Owning team and cost centre
- Declared capability scopes (what the agent is allowed to do)
- Maximum token lifetime and permitted delegation depth
- Pointer to the agent's policy document in the governance store

The registry acts as the authoritative source of truth: the identity broker validates that a requesting agent matches its registered profile before issuing a token.

## Non-Repudiation and Audit

Every privileged action an agent performs must be attributable:

- Agent tokens should carry a `jti` (JWT ID) that is logged at issuance.
- The platform's observability pipeline (Capability 22) must capture `agent_id`, `parent_agent_id`, `correlation_id` and `user_subject` on every API call.
- Immutable audit logs (write-once storage, tamper-evident) should record token issuance, token exchange, and revocation events.
- Audit records must be retained for the duration required by applicable compliance frameworks (see [Compliance](./Compliance.md)).

## Secret and Credential Hygiene

{: .warning }
> Long-lived shared secrets in agent code or container images are one of the highest-risk patterns in enterprise AI deployments. Prefer workload identity everywhere.

Best practices:

- Use managed identities or OIDC federation; eliminate static API keys.
- Rotate any credential that cannot be replaced with workload identity on a schedule no longer than 90 days.
- Store secrets in a platform-managed vault (e.g., Azure Key Vault, HashiCorp Vault) with access policies tied to agent identity.
- Scan agent container images and IaC manifests for embedded secrets in CI/CD pipelines.
- Revoke and rotate immediately upon any agent decommissioning or security incident.

## Summary

A robust agent identity model provides the foundation on which every other cross-cutting concern—security, observability, cost attribution and compliance—depends. Investing in identity primitives early pays compounding dividends as the agent estate grows.
