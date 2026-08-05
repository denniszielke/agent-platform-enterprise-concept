---
layout: default
title: Third-Party Platforms
parent: Advanced Topics
nav_order: 1
---

# Third-Party Platforms

Enterprise AI programmes rarely build everything in-house. Third-party platforms—specialised AI development environments, model hosting services, agent frameworks, and vertical AI applications—play a significant role in accelerating capability delivery. However, integrating third-party platforms into the enterprise agent estate introduces governance, security, and interoperability challenges that must be managed systematically. This page describes how the enterprise platform accommodates third-party AI platforms while maintaining its governance posture.

## Supported Capabilities

| Capability | Third-Party Platforms Role |
|---|---|
| **21 – Enterprise Capability Marketplace** | Distribution point for approved third-party integrations |
| **19 – Tool & MCP Connectivity** | MCP as the integration protocol for third-party tool surfaces |
| **20 – Agent & MCP Registry** | Third-party agents and tools registered alongside first-party |
| **9 – Governance & Compliance** | Governance requirements applied to third-party platforms |
| **7 – Identity & Trust** | Identity federation between enterprise IdP and third-party platforms |
| **15 – Model Gateway** | Routes requests through approved third-party model endpoints |
| **23 – AI FinOps** | Cost visibility across first-party and third-party spend |

## Categories of Third-Party Platforms

| Category | Examples | Integration Considerations |
|---|---|---|
| **Model hosting services** | Azure OpenAI, Anthropic API, Cohere, Mistral AI | Route through Model Gateway; negotiate enterprise agreements; validate data residency |
| **Agent development frameworks** | LangChain, LlamaIndex, Semantic Kernel, CrewAI | Runtime instrumentation; OpenTelemetry compatibility; identity integration |
| **Low-code / no-code agent builders** | Microsoft Copilot Studio, Salesforce Einstein, ServiceNow AI | SSO federation; data governance for connected systems; output review requirements |
| **Vertical AI applications** | Legal AI, HR AI, finance AI SaaS | Approved data sharing agreements; SSO; audit log export |
| **Vector database services** | Pinecone, Weaviate Cloud, Qdrant Cloud | Data residency; encryption; access control federation |
| **Evaluation and observability** | Weights & Biases, Langfuse, Arize | Telemetry data sharing policy; PII handling in traces |
| **MCP server ecosystems** | Composio, Zapier MCP, vendor-published MCP servers | Registry vetting; supply chain security |

## Governance Framework for Third-Party Platforms

Before a third-party platform may be used on the enterprise agent platform, it must pass a structured review:

### 1. Security Assessment

- Vendor security posture review (SOC 2 Type II, ISO 27001, or equivalent)
- Data processing agreement (DPA) for any platform that processes enterprise data
- Review of the vendor's subprocessor list
- Penetration testing disclosure and vulnerability disclosure programme
- Data residency and sovereign cloud options

### 2. Compliance Review

- Verify the vendor's compliance certifications for applicable frameworks (see [Compliance](../04-Cross-Cutting-Topics/Compliance.md))
- Assess implications for EU AI Act risk classification if the vendor's model is used in a high-risk use case
- Review the vendor's model training data practices for IP and privacy implications

### 3. Commercial and Contractual Review

- Enterprise agreement terms: data ownership, intellectual property, audit rights
- SLA commitments and credits
- Pricing model and commitment options (to inform FinOps planning)
- Exit clauses and data portability obligations

### 4. Technical Integration Review

- API compatibility with platform standards (OpenAI-compatible preferred)
- Identity federation: OIDC/SAML support for SSO; service principal support for machine identity
- Observability: does the platform export telemetry in standard formats (OpenTelemetry, Prometheus)?
- MCP support: can the platform expose its capabilities as an MCP server?

## Identity Federation

Integrating third-party platforms into the enterprise identity model:

- **SSO for human users**: configure OIDC or SAML federation between the enterprise IdP (Entra ID, Okta) and the third-party platform. Users authenticate once; no separate credentials required.
- **Machine identity for agents**: use service principals or managed identities with scoped permissions. Avoid embedding API keys; use the platform's secret management infrastructure.
- **Attribute mapping**: map enterprise identity attributes (department, role, cost centre) to the third-party platform to enable attribute-based access control.

{: .note }
> If a third-party platform does not support federated identity, it should be treated as a higher-risk integration. Document the exception, implement compensating controls (short-lived API keys, IP allowlisting), and establish a roadmap for migration to a compliant alternative.

## Data Flow Controls

Data shared with third-party platforms requires explicit controls:

| Data Type | Control Required |
|---|---|
| Prompt content containing enterprise data | DPA in place; data residency confirmed; PII redaction applied before transmission where required |
| Model training data | Explicit prohibition or explicit consent in agreement; no customer or employee PII |
| Agent telemetry and traces | PII redaction before export; data retention limits agreed |
| Fine-tuning datasets | Provenance documented; licence confirmed; processed in enterprise-controlled environment |

## MCP Ecosystem Integration

The growing ecosystem of vendor-published MCP servers (Capability 19) enables rapid integration with third-party SaaS platforms. Governance requirements for third-party MCP servers:

- Register in the Agent & MCP Registry before use
- Verify image provenance: use only signed, versioned images from the vendor's official registry
- Review the tool schema: ensure tool descriptions are accurate and side effects are disclosed
- Apply network egress controls: MCP calls to third-party SaaS route through the platform egress proxy
- Monitor usage: alert on anomalous call patterns that may indicate misuse or a compromised agent

## Vendor Concentration Risk

Dependence on a small number of third-party platforms creates concentration risk. Mitigate by:

- Abstracting third-party integrations behind platform interfaces (the Model Gateway abstracts model providers; MCP abstracts tool surfaces)
- Maintaining a multi-vendor model strategy: capability at more than one provider for critical model types
- Negotiating data portability and export rights in all enterprise agreements
- Conducting annual vendor risk reviews; update the risk register with current concentration metrics

## Centralized vs. Federated Third-Party Governance

| Dimension | Centralized | Federated |
|---|---|---|
| Approval Authority | Central platform team approves all third-party platforms | Domain teams may approve domain-specific platforms within central guardrails |
| Commercial Negotiations | Central procurement negotiates all enterprise agreements | Central team negotiates strategic agreements; domain teams manage tactical spend |
| Security Assessment | Central security team conducts all assessments | Domain security leads conduct domain assessments; central team reviews |
| Registry Maintenance | Central team manages all third-party registry entries | Domain teams manage their entries; central team audits compliance |

See [Centralized Operating Model](../03a-Centralized-Operating-Model/Overview.md) and [Federated Operating Model](../03b-Federated-Operating-Model/Overview.md).

## Summary

Third-party platforms are an essential part of the enterprise AI ecosystem. By establishing a structured approval process, enforcing identity federation and data flow controls, leveraging MCP for interoperability, and managing vendor concentration risk, the enterprise can benefit from the breadth of the AI vendor ecosystem without compromising its governance posture.
