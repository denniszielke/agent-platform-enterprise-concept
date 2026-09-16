---
layout: default
title: Compliance
parent: Cross-Cutting Topics
nav_order: 8
---

# Compliance

Compliance for the enterprise agent platform encompasses the controls, evidence artefacts, and processes needed to demonstrate adherence to applicable laws, regulations, industry standards, and internal policies. AI agent deployments face a converging set of obligations: data protection regulation, sector-specific AI rules, security standards, and emerging AI-specific regulation. A structured compliance programme prevents duplication of effort by mapping platform controls to multiple frameworks simultaneously.

## Supported Capabilities

| Capability | Compliance Role |
|---|---|
| **9 – Governance & Compliance** | Core: policy management, control registry, audit automation |
| **7 – Identity & Trust** | Authentication and authorisation controls evidence |
| **8 – AI Runtime Protection** | Technical controls for AI-specific regulatory requirements |
| **22 – Observability** | Audit log generation and retention |
| **10 – Lifecycle Automation** | Compliance checks embedded in deployment pipelines |
| **17 – Knowledge & Data Platform** | Data retention, deletion, and residency enforcement |
| **5 – Platform Management** | Configuration baselines and vulnerability management |

## Applicable Regulatory Landscape

The following frameworks are commonly applicable to enterprise agent platforms. Teams should perform a formal scoping exercise for their jurisdiction and sector:

| Framework | Applicability | Key AI-Relevant Obligations |
|---|---|---|
| **EU AI Act** | Organisations deploying AI in the EU | Risk categorisation, conformity assessment for high-risk systems, transparency, human oversight |
| **GDPR / UK GDPR** | Processing personal data of EU/UK residents | Lawful basis, purpose limitation, data subject rights, automated decision-making transparency |
| **HIPAA** | US healthcare data | PHI safeguards, audit controls, breach notification |
| **PCI DSS** | Payment card data | Access control, audit logging, encryption, vulnerability management |
| **SOC 2 Type II** | Service organisations, B2B SaaS | Trust service criteria: security, availability, confidentiality, privacy |
| **ISO 27001** | Information security management | ISMS controls, risk management, incident management |
| **NIST AI RMF** | US federal and adopting organisations | AI risk identification, measurement, management, governance |
| **MiFID II / SR 11-7** | Financial services | Model risk management, audit trail, explainability |
| **NIS2** | Critical infrastructure in the EU | Incident reporting, supply chain security, technical hygiene |

## Control Mapping

A control mapping approach allows the organisation to implement a control once and satisfy multiple frameworks. The table below illustrates a partial mapping:

| Control | EU AI Act | GDPR | SOC 2 | ISO 27001 |
|---|---|---|---|---|
| Immutable audit logging | Art. 12 (record-keeping) | Art. 5(2) (accountability) | CC7.2 | A.8.15 |
| Data classification and labelling | Art. 10 (data governance) | Art. 5(1)(b) (purpose limitation) | C1.1 | A.5.12 |
| Agent identity and access control | Art. 9 (accuracy, security) | Art. 32 (security of processing) | CC6.1 | A.5.15 |
| Incident detection and response | Art. 62 (serious incidents) | Art. 33–34 (breach notification) | CC7.3–7.5 | A.5.24 |
| Human oversight mechanism | Art. 14 (human oversight) | Art. 22 (automated decision-making) | — | — |
| Model evaluation and documentation | Art. 9, 11, 13 | Art. 35 (DPIA) | CC3.2 | A.8.25 |
| Vulnerability management | Art. 9 (security) | Art. 32 | CC7.1 | A.8.8 |

## AI Act Risk Classification

The EU AI Act introduces a tiered risk framework that affects platform design. Agent developers must classify their use cases:

| Risk Tier | Examples | Platform Requirements |
|---|---|---|
| **Unacceptable risk** (prohibited) | Social scoring, real-time biometric surveillance | Not permitted on the platform |
| **High risk** | HR recruitment, credit scoring, medical diagnosis support, law enforcement | Conformity assessment, registration in EU database, mandatory human oversight, full audit trail |
| **Limited risk** | Customer-facing chatbots, content generation | Transparency obligations (disclosure that user is interacting with AI) |
| **Minimal risk** | Internal document summarisation, code assistance | No mandatory obligations; voluntary code of practice recommended |

{: .note }
> The classification of a use case—not the underlying model—determines the risk tier. The same model used for internal summarisation (minimal risk) and credit decision support (high risk) requires fundamentally different governance treatment.

## Compliance Automation

Manual compliance evidence collection is not scalable across a large agent estate. The platform should automate:

- **Continuous control monitoring**: automated checks verify that controls are in place and effective (e.g., audit logging enabled, encryption at rest configured, tags present).
- **Evidence collection**: automated snapshots of control state are captured and stored in the compliance evidence repository.
- **Policy-as-code**: compliance policies are expressed in machine-readable formats (e.g., Open Policy Agent Rego, Azure Policy) and evaluated against infrastructure state.
- **Audit report generation**: audit reports are generated from the evidence repository on demand, reducing manual effort during assessments.

The governance and compliance capability (Capability 9) provides the policy engine and evidence store that supports automation.

## Centralized vs. Federated Compliance

| Dimension | Centralized | Federated |
|---|---|---|
| Policy Authority | Central compliance team owns all policies | Central team owns framework policies; domain teams implement and extend |
| Audit Scope | Single audit covers the entire platform | Domain-level audits rolled up to enterprise audit |
| Evidence Management | Centralised evidence repository | Domain teams submit evidence to central repository |
| Remediation | Central team drives remediation | Domain teams own remediation; centre tracks and escalates |
| Regulatory Liaison | Central team manages regulator relationships | Domain leads for sector-specific regulators; centre coordinates |

See [Centralized Operating Model](../03a-Centralized-Operating-Model/Overview.md) and [Federated Operating Model](../03b-Federated-Operating-Model/Overview.md).

## Data Subject Rights and AI

When agents process personal data, data subject rights must be operationally fulfillable:

- **Right of access**: the platform must be able to produce a record of what data about an individual was processed by which agent, and when.
- **Right to erasure**: deletion requests must propagate through the data pipeline, including vector indexes where the individual's data may be embedded.
- **Right to explanation**: for automated decisions, the platform must be able to produce a human-readable explanation of the factors that influenced the outcome.
- **Right to object**: opt-out from automated processing must be technically enforceable—not just a policy commitment.

These rights require the data lineage (see [Data Governance](./Data-Governance.md)) and audit trail (see [Observability](./Observability.md)) capabilities to be fully operational.

## Incident and Breach Management

Compliance obligations impose specific timelines on incident response:

| Framework | Notification Requirement |
|---|---|
| GDPR | 72 hours to supervisory authority; without undue delay to data subjects |
| HIPAA | 60 days to HHS; immediate notification if urgent risk |
| EU AI Act (Art. 62) | 15 working days for serious incidents involving high-risk AI |
| NIS2 | 24-hour early warning; 72-hour incident report |

The platform incident response runbook (see [Security](./Security.md)) must include compliance notification workflows with defined timelines, responsible owners, and template communications.

## Human Oversight Requirements

Several frameworks require that automated decisions with significant effects on individuals are subject to human review:

- Deploy human-in-the-loop workflows for high-risk AI use cases.
- Implement override mechanisms that allow human reviewers to correct or reject agent decisions.
- Log all human oversight actions (review, approval, rejection, override) in the audit trail.
- Test override mechanisms regularly to ensure they function as intended.

## Summary

Compliance for the enterprise agent platform is a programme that spans regulation mapping, control implementation, automation, audit readiness, and continuous monitoring. By building compliance controls into the platform infrastructure rather than individual agents, the organisation achieves consistent, scalable compliance coverage that grows with the agent estate without proportionally growing compliance overhead.
