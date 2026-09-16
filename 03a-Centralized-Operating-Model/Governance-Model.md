---
layout: default
title: Governance Model
parent: Centralized Operating Model
nav_order: 3
---

# Governance Model

Governance in the centralized operating model is both a design principle and an operational discipline. Because a single platform team controls the entire agent lifecycle, it is uniquely positioned to enforce governance with consistency and minimal friction for consuming teams. This document defines how policies are authored, enforced, reviewed, and audited across all 24 platform capabilities.

---

## Governance Philosophy

The centralized model treats governance as infrastructure — not as a post-deployment checklist. Controls are codified in policy engines, embedded in CI/CD pipelines, and enforced at runtime. The goal is to make compliance the path of least resistance: a team that follows the platform's paved road automatically satisfies the majority of enterprise control requirements.

{: .highlight }
> Governance in the centralized model should be invisible to domain teams during normal operation and highly visible only when a violation or exception is detected.

---

## Governance Domains

### 1. Policy Authoring and Management

All policies are maintained as code in a central repository, reviewed via pull request, and deployed through a GitOps pipeline. Policy categories include:

- **Access policies** — Who can invoke which agents, access which data sources, and modify which configurations.
- **Content policies** — Permitted and prohibited content categories for model inputs and outputs, aligned to the organisation's acceptable use policy.
- **Data policies** — Which data classification levels can be used in agent context windows, RAG retrievals, and tool outputs.
- **Operational policies** — Quota limits, rate limits, approved model versions, required observability tags.

| Policy Category | Enforcement Point | Review Cadence |
|---|---|---|
| Access | Identity provider + API gateway | Per change |
| Content | Model gateway content filter | Quarterly + incident-triggered |
| Data | Knowledge platform row security + DLP | Per change |
| Operational | Quota service + deployment pipeline | Monthly |

### 2. Lifecycle Governance

Every agent passes through a standardised lifecycle gate:

```
Intake → Design Review → Security Review → Evaluation Gate → Staging → Production → Active Monitoring → Deprecation
```

| Gate | Owner | Pass Criteria |
|---|---|---|
| Intake | Platform team (product) | Business case documented, data sources identified, compliance classification assigned |
| Design Review | Platform engineering + domain liaison | Architecture aligned to reference patterns, no unapproved external dependencies |
| Security Review | Security & compliance team | Threat model completed, identity model reviewed, data handling approved |
| Evaluation Gate | AI/ML engineering | Quality, safety, and latency benchmarks met; red-team pass |
| Staging | Platform SRE | Load test passed, observability configured, runbook written |
| Production | Platform team (change management) | Change advisory board approval for high-risk agents; expedited for standard patterns |
| Active Monitoring | Operations | Continuous — drift alerts, quality degradation alerts, cost anomaly alerts |
| Deprecation | Platform team + domain owner | Migration plan agreed, consumers notified, traffic drain confirmed |

### 3. AI-Specific Governance

AI workloads introduce governance requirements beyond traditional software:

- **Model versioning** — Approved model versions are listed in the platform registry. Using an unapproved version fails the deployment pipeline.
- **Prompt governance** — System prompts for registered agents are stored in version control. Changes require a design review.
- **Grounding source governance** — Documents and data used for RAG retrieval must be ingested through the governed knowledge platform, not uploaded ad hoc.
- **Output validation** — Structured outputs (e.g., function call arguments, JSON) are validated against schemas before being passed to downstream systems.
- **Human-in-the-loop triggers** — High-consequence actions (financial transactions above threshold, external communications, data deletion) require human approval, enforced at the orchestration layer.

### 4. Regulatory Compliance Mapping

The platform team maintains a controls matrix mapping platform capabilities to applicable regulatory frameworks:

| Framework | Relevant Capabilities | Control Examples |
|---|---|---|
| ISO 27001 | Identity, logging, access control | A.9 (access), A.12 (operations), A.18 (compliance) |
| SOC 2 Type II | Observability, change management, availability | CC6, CC7, A1 |
| GDPR / UK GDPR | Data platform, content filtering, retention | Data minimisation, purpose limitation, retention schedules |
| EU AI Act (high-risk) | Evaluation, human oversight, transparency | Conformity assessment, audit trail, human oversight mechanisms |
| DORA (financial) | Resilience, incident management, vendor risk | ICT risk, testing, third-party risk |

{: .note }
> The regulatory mapping is a living document. As new AI-specific regulations emerge (EU AI Act implementation dates, national AI strategies), the platform team updates the controls matrix and communicates impact to consuming business units.

### 5. Exception Management

When a business unit requires a deviation from standard policy (e.g., access to an unapproved model, use of a non-standard data source), the exception process is:

1. **Exception request** submitted via the internal developer portal with business justification and risk assessment.
2. **Risk review** by the security & compliance team — typically a 5-business-day SLA.
3. **Time-limited approval** — Exceptions are granted for a fixed period (30, 90, or 180 days) with automatic expiry.
4. **Compensating controls** — The exception approval specifies compensating controls (e.g., enhanced logging, restricted user scope) that must be implemented before the exception is activated.
5. **Exception register** — All approved exceptions are recorded and reviewed quarterly.

---

## Governance Tooling

| Tool | Purpose | Integration |
|---|---|---|
| Azure Policy / OPA Rego | Infrastructure and runtime policy enforcement | Deployed via GitOps, evaluated at admission and runtime |
| Microsoft Purview | Data lineage, classification, sensitivity labelling | Connected to knowledge platform and agent memory stores |
| Azure Monitor + Sentinel | Security event detection and response | Feeds from all platform layers; automated alert routing |
| GitHub Actions gates | CI/CD pipeline policy gates | Prevents non-compliant agents from reaching staging |
| Internal developer portal | Exception requests, capability requests, audit trail | Self-service governance interface for domain teams |

---

## Audit and Reporting

The central platform team produces the following governance artefacts on a defined cadence:

| Artefact | Audience | Frequency |
|---|---|---|
| Governance dashboard | CISO, CTO, platform team | Real-time (dashboarded) |
| Compliance posture report | Risk committee, internal audit | Monthly |
| AI risk register | Platform team, CISO | Quarterly review |
| Exception register review | Security & compliance, risk committee | Quarterly |
| Regulatory update briefing | All stakeholders | As needed (significant changes) |
| Annual penetration test report | CISO, board risk committee | Annual |

---

## Governance Maturity Progression

Governance capability should evolve alongside the platform. The following maturity levels guide investment priorities:

| Level | Description | Key Investments |
|---|---|---|
| **1 — Manual** | Policies documented; enforcement relies on process and review | Checklists, design review templates, policy wiki |
| **2 — Automated gates** | CI/CD pipeline enforces policy checks; deployment blocked on violation | Policy-as-code in pipeline, automated security scanning |
| **3 — Runtime enforcement** | Policies enforced at runtime, not just at deployment | OPA sidecar, API gateway policies, content filter integration |
| **4 — Continuous compliance** | Continuous monitoring, drift detection, automated remediation | Policy drift alerts, auto-remediation runbooks, compliance dashboards |
| **5 — Adaptive governance** | ML-assisted anomaly detection, dynamic policy adjustment, self-healing | AI-assisted threat detection, dynamic quota adjustment, proactive risk scoring |

Most organisations starting with the centralized model will begin at Level 1–2 and target Level 3 by the end of the [MVP Roadmap](./MVP-Roadmap.md) Phase 2.
