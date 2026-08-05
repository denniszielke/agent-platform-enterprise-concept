---
layout: default
title: Security
parent: Cross-Cutting Topics
nav_order: 2
---

# Security

Security for an enterprise agent platform extends well beyond traditional application security. Agents consume sensitive enterprise data, call internal and external APIs, execute code, and may act autonomously over extended periods. A comprehensive security posture must address threats at every layer: the model itself, the runtime environment, the data pipelines, the tool integrations, and the human-agent interaction surface.

## Supported Capabilities

| Capability | Security Relevance |
|---|---|
| **7 – Identity & Trust** | Foundation for authentication and authorisation of agent principals |
| **8 – AI Runtime Protection** | Real-time guardrails, prompt injection defence, output filtering |
| **9 – Governance & Compliance** | Policy enforcement, security baselines, risk registers |
| **4 – Network Topology** | Network segmentation, private endpoints, egress control |
| **5 – Platform Management** | Patch management, vulnerability scanning, configuration baselines |
| **10 – Lifecycle Automation** | Secure CI/CD, SBOM generation, image signing |
| **22 – Observability** | Threat detection, anomaly alerting, SIEM integration |

## Threat Landscape

The agent platform introduces threat vectors that are unique or amplified compared with conventional software:

- **Prompt injection** – malicious content in retrieved documents, tool outputs, or user inputs attempts to hijack agent instructions.
- **Indirect prompt injection** – adversarial content embedded in external data sources (web pages, emails, database records) is consumed by the agent during retrieval.
- **Excessive agency** – an agent with over-provisioned permissions takes destructive or unauthorised actions.
- **Data exfiltration via model output** – a model is coerced into including sensitive data in responses that are forwarded to an attacker-controlled endpoint.
- **Supply chain attacks** – compromised model weights, tool packages, or MCP server implementations introduce backdoors.
- **Credential theft** – improperly stored API keys or tokens allow lateral movement within the enterprise.
- **Model denial-of-service** – adversarial prompts designed to maximise token consumption exhaust budget limits.

## Defence-in-Depth Architecture

Security controls should be layered so that the failure of any single control does not result in a significant breach.

### Layer 1 – Network Controls

- Deploy agent runtimes in isolated virtual networks with no default internet egress.
- Use private endpoints for all AI APIs (model gateways, vector stores, data platforms).
- Implement egress filtering to allow only approved external destinations.
- Apply network security groups or equivalent firewall rules at every subnet boundary.

### Layer 2 – Identity and Access Controls

- Enforce workload identity for all agent principals (see [Agent Identity](./Agent-Identity.md)).
- Apply least-privilege RBAC: agents receive only the permissions their declared capabilities require.
- Require MFA for human administrators who manage agent configurations or policies.
- Implement just-in-time privileged access for break-glass operations.

### Layer 3 – AI Runtime Protection (Capability 8)

The AI runtime protection layer sits between the agent and the model endpoint:

| Control | Purpose |
|---|---|
| Input guardrails | Detect and block prompt injection, jailbreak attempts, PII submission |
| Output guardrails | Filter hallucinated credentials, offensive content, policy violations |
| Topic restrictions | Prevent agents from reasoning outside their declared domain |
| Rate limiting | Per-agent and per-user token budgets enforced at the gateway |
| Audit logging | Immutable record of all prompts and completions |

Technologies such as Azure AI Content Safety, Guardrails AI, and LLM-Guard implement parts of this layer. The Model Gateway (Capability 15) is the natural integration point.

### Layer 4 – Application and Code Controls

- Validate and sanitise all inputs before injection into prompts.
- Use structured output schemas (JSON Schema, Pydantic models) to constrain model responses and prevent injection through output parsing.
- Sign and verify agent container images (e.g., Sigstore/Cosign) in CI/CD pipelines.
- Generate and store Software Bill of Materials (SBOM) for every agent release.
- Scan dependencies for known vulnerabilities using automated tools integrated into pipelines.

### Layer 5 – Data Controls

- Classify data at ingestion; propagate sensitivity labels through retrieval pipelines.
- Enforce row-level and column-level security in knowledge stores so agents only retrieve data they are authorised to see.
- Encrypt data at rest (AES-256 or equivalent) and in transit (TLS 1.2 minimum, 1.3 preferred).
- Implement data loss prevention (DLP) policies on model outputs before they reach end users.

## Centralized vs. Federated Security Models

| Dimension | Centralized | Federated |
|---|---|---|
| Security Baseline | Single baseline enforced by platform team | Baseline defined centrally; domains extend within guardrails |
| Incident Response | Centralised SOC handles all agent incidents | Domain teams handle first response; escalate to central SOC |
| Vulnerability Management | Central team patches platform components | Shared responsibility: platform patches infrastructure, domains patch applications |
| Policy Enforcement | Central policy engine; domains consume policies | Federated policy engines synchronised from central store |
| Penetration Testing | Single programme covering entire estate | Coordinated programme with domain-specific testing |

See [Centralized Operating Model](../03a-Centralized-Operating-Model/Overview.md) and [Federated Operating Model](../03b-Federated-Operating-Model/Overview.md).

## MCP Tool Security

Model Context Protocol (MCP) servers expose tools to agents. Each MCP server is an attack surface:

- Register all MCP servers in the Agent & MCP Registry (Capability 20) before use.
- Verify MCP server provenance: use only signed, approved server images.
- Scope MCP tool tokens to the minimum required resource access.
- Implement server-side validation in every MCP tool handler; never trust agent-provided inputs without validation.
- Monitor MCP call patterns for anomalies (unusually high call rates, access to unexpected resources).

{: .note }
> MCP servers that call external APIs should route through the platform's egress proxy so traffic can be inspected, logged, and throttled.

## Security Incident Response

A documented runbook should address:

1. **Detection** – alerting from the observability pipeline on anomalous agent behaviour, policy violations, or authentication failures.
2. **Containment** – automated or on-call ability to suspend an agent's workload identity token, isolating it without full service disruption.
3. **Investigation** – full prompt/completion audit logs, token issuance records, and network flow logs available to incident responders.
4. **Eradication** – patch or replace the compromised component; rotate all credentials the agent held.
5. **Recovery** – restore agent from a known-good signed image; validate guardrails before returning to production.
6. **Post-incident review** – update threat models, guardrail configurations, and runbooks.

## Compliance Alignment

Security controls directly support several compliance frameworks. See [Compliance](./Compliance.md) for framework-specific mapping. Key controls that satisfy the broadest set of frameworks include:

- Immutable audit logging with tamper-evident storage
- Encryption at rest and in transit
- Least-privilege identity and access management
- Vulnerability management and patch cadence
- Incident response documentation and testing

## Summary

Security for enterprise agent platforms requires a defence-in-depth strategy that spans network topology, identity, runtime protection, application logic, and data handling. Centralising shared controls (network egress, identity issuance, policy baselines) while enabling domain teams to extend them creates a scalable security posture that grows with the agent estate.
