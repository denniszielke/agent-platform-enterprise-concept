---
layout: default
title: Model Governance
parent: Cross-Cutting Topics
nav_order: 7
---

# Model Governance

Model governance defines the policies, processes, and controls that ensure AI models deployed on the enterprise platform behave as intended, remain fit for purpose over time, and are used in accordance with organisational, legal, and ethical standards. It bridges the concerns of model selection, evaluation, deployment, monitoring, and retirement—covering both models the organisation consumes as a service and those it fine-tunes or develops internally.

## Supported Capabilities

| Capability | Model Governance Role |
|---|---|
| **9 – Governance & Compliance** | Policy framework for model approval, use restrictions, and audit |
| **15 – Model Gateway** | Enforcement point for approved model list, version pinning, and usage policies |
| **18 – Evaluation Engineering** | Measurement infrastructure for quality, safety, and bias assessment |
| **10 – Lifecycle Automation** | Model promotion pipelines with governance gates |
| **7 – Identity & Trust** | Model access restricted to authorised agents and principals |
| **22 – Observability** | Ongoing monitoring of model behaviour in production |
| **16 – Semantic Foundation** | Ontology for model metadata and capability description |

## The Model Lifecycle

Model governance applies across the full lifecycle:

```
Evaluate → Approve → Deploy → Monitor → Review → Retire
    ↑                                        |
    └─── Update / Fine-tune ←────────────────┘
```

Each stage has defined governance gates:

| Stage | Gate | Owner |
|---|---|---|
| Evaluate | Safety and quality baselines met; no prohibited capabilities detected | AI Safety team + Evaluation Engineering |
| Approve | Legal review (IP, data provenance); Compliance sign-off; Risk acceptance | Legal, Compliance, CISO |
| Deploy | Canary rollout; rollback plan documented; monitoring alerts configured | Platform SRE + Agent developer |
| Monitor | Drift detection; ongoing safety evaluation; cost tracking | AIOps + FinOps |
| Review | Periodic re-evaluation against updated baselines; newer alternatives assessed | Model governance board |
| Retire | Migration plan for dependent agents; deprecation notice period; audit trail closed | Platform team + owning teams |

## Approved Model Catalogue

The Model Gateway (Capability 15) enforces an **approved model catalogue**: only models that have passed governance review may be invoked through the platform. The catalogue records:

- Model identifier and version (semantic versioning where available)
- Provider and hosting location
- Data residency and sovereignty constraints
- Approved use cases and prohibited use cases
- Evaluation scores (accuracy, safety, bias, cost-per-token)
- Licence and intellectual property status
- Deprecation timeline (if applicable)

{: .note }
> Version pinning is essential: agents should reference a specific approved model version, not a floating alias like `gpt-4-latest`. Floating aliases can introduce unexpected behaviour changes when providers update underlying models.

## Safety and Ethics Evaluation

Before a model is approved, the governance team must conduct:

- **Safety evaluation**: red-teaming for harmful outputs, jailbreak resistance, refusal behaviour on prohibited topics.
- **Bias and fairness assessment**: evaluation across demographic groups and protected characteristics relevant to the intended use cases.
- **Hallucination benchmarking**: factual accuracy on domain-relevant question sets, calibrated against the organisation's risk tolerance.
- **Capability boundary assessment**: documenting what the model can and cannot reliably do, to inform use case restrictions.

Evaluation results must be documented and retained as part of the governance record.

## Fine-Tuning Governance

When the organisation fine-tunes base models, additional governance obligations arise:

- **Training data governance**: all training data must comply with the Data Governance policy (see [Data Governance](./Data-Governance.md)). Provenance, consent, and classification must be documented.
- **Differential privacy**: for training on sensitive data, evaluate whether differential privacy techniques are required.
- **Evals before and after fine-tuning**: demonstrate that fine-tuning improved target capabilities without degrading safety baselines.
- **Model card generation**: produce a standardised model card documenting intended use, limitations, training data summary, and evaluation results.
- **Weight storage and access control**: fine-tuned weights are an intellectual property asset; store in a governed model registry with access policies tied to approved principals.

## Drift and Degradation Monitoring

Models deployed in production can degrade due to:

- **Concept drift**: the real-world distribution of inputs changes, diverging from the training distribution.
- **Provider-side updates**: hosted model providers silently update model weights, changing behaviour.
- **Data pipeline changes**: upstream data changes affect the knowledge context retrieved by agents.

The platform must monitor:

- Quality metric trends (from evaluation engineering, Capability 18) compared to approval-time baselines.
- Output distribution changes (response length, refusal rate, structured output validity rate).
- User feedback signals where available.

Alerts should trigger when metrics deviate beyond defined thresholds, prompting a governance review.

## Centralized vs. Federated Model Governance

| Dimension | Centralized | Federated |
|---|---|---|
| Catalogue Authority | Central model governance board maintains single approved catalogue | Central board approves base models; domain teams may approve domain-specific fine-tunes |
| Evaluation Responsibility | Central AI safety team conducts all evaluations | Domain teams conduct use-case evaluations; central team audits |
| Deployment Control | Central platform team controls model rollout | Domain teams deploy from approved catalogue; subject to central policy |
| Monitoring Ownership | Central AIOps monitors all model performance | Domain teams monitor their models; anomalies escalated to centre |
| Incident Response | Central team handles model-related incidents | Domain teams handle initial response; coordinate with central on systemic issues |

See [Centralized Operating Model](../03a-Centralized-Operating-Model/Overview.md) and [Federated Operating Model](../03b-Federated-Operating-Model/Overview.md).

## Regulatory and Legal Considerations

Model governance intersects with an increasing volume of regulation:

- **EU AI Act**: high-risk AI systems require conformity assessments, transparency obligations, and human oversight mechanisms. The governance framework must map agent use cases to risk categories.
- **GDPR / data protection**: models trained on personal data require lawful basis documentation; "right to explanation" may apply to automated decisions.
- **Sector-specific regulation**: financial services (MiFID, SR 11-7), healthcare (FDA guidance on AI/ML-based software), and public sector (algorithmic accountability) all impose additional requirements.
- **Intellectual property**: the provenance of training data affects whether model outputs may infringe third-party IP.

The compliance programme (see [Compliance](./Compliance.md)) should map model governance controls to applicable regulatory requirements.

## Model Cards and Transparency

Every model in the approved catalogue should have a **model card** that is accessible to agent developers. A model card covers:

- Intended use cases and out-of-scope uses
- Known limitations and failure modes
- Evaluation results (summarised; detailed results in the governance record)
- Training data summary
- Fairness and bias considerations
- Contact information for the governance owner

Model cards support responsible use by helping developers select the appropriate model and design agents with awareness of model limitations.

## Summary

Model governance is a continuous programme, not a one-time approval exercise. By maintaining an approved catalogue, conducting rigorous evaluation, monitoring production behaviour, and aligning with regulatory requirements, the enterprise ensures that its AI models remain safe, compliant, and fit for purpose as both the technology and the regulatory landscape evolve.
