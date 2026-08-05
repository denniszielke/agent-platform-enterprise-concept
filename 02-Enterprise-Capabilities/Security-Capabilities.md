---
layout: default
title: Security Capabilities
parent: Enterprise Capabilities
nav_order: 4
---

# Security capabilities

Security capabilities cover AI runtime protection (8), governance and compliance (9) and
lifecycle automation (10). Together with [identity capabilities](Identity-Capabilities.md)
they form the Governance & Security area of the capability model.

Agents change the security problem in three ways: untrusted content can influence
execution, agents act with real permissions, and behaviour is probabilistic rather than
deterministic. Traditional application controls remain necessary but are not sufficient.

## AI runtime protection (8)

| Threat | Control | Where enforced |
| --- | --- | --- |
| Prompt injection from retrieved or user content | Input shielding, content provenance marking, instruction isolation | Gateway and runtime |
| Harmful or non-compliant output | Output filtering and business-rule validation | Gateway |
| Excessive or unintended tool use | Scoped tool permissions, per-tool approval, rate limits | Tool broker / MCP layer |
| Data exfiltration through tool or model calls | Egress control, output inspection, DLP integration | Network and gateway |
| Model or prompt tampering | Signed prompt assets, versioned deployments | Lifecycle pipeline |
| Runaway loops and cost abuse | Step limits, timeouts, quotas | Runtime and gateway |

{: .warning }
> Treat every piece of retrieved content as untrusted input. Grounding data, tool results
> and user messages can all carry injected instructions.

Defence in depth matters because no single filter is reliable against a probabilistic
system. The combination that works in practice is: constrain what the agent *can* do
through scoped identity and tool permissions, inspect what flows in and out, and record
everything for review.

## Governance and compliance (9)

Governance is the capability to define policy, apply it automatically, and produce
evidence without a manual assembly effort. Practically this means:

- A register of AI use cases with risk classification and named accountable owners.
- Policy expressed as code and evaluated in pipelines and at runtime.
- Approval workflows proportional to risk — light for internal productivity scenarios,
  substantial for customer-facing or regulated decisions.
- Automatically generated evidence: what was deployed, by whom, with which model,
  prompt, tool set, evaluation results and approvals.
- Defined incident handling for AI-specific failures such as harmful output, hallucinated
  commitments and unintended actions.

## Lifecycle automation (10)

The pipeline is the enforcement point. If production changes can only be made through
the pipeline, then every control embedded in the pipeline is guaranteed rather than
hoped for.

A production-grade agent pipeline typically includes: source control for prompts, tools
and configuration; static and dependency scanning; automated quality and safety
evaluations with pass thresholds; policy checks; staged deployment with rollback; and
automated decommissioning of retired agents and their credentials and memory.

## Shared responsibility

| Control | Centralised model | Federated model |
| --- | --- | --- |
| Safety filters | Operated centrally for all traffic | Central defaults, domain cannot weaken them |
| Policy definition | Central | Central, with domain-specific extensions |
| Policy enforcement | Central pipeline | Domain pipelines using shared policy packs |
| Evidence production | Central reporting | Central aggregation of domain telemetry |
| Incident response | Central team | Domain first response, central coordination |

See also [Security]({{ site.baseurl }}/04-Cross-Cutting-Topics/Security.html) for the
cross-cutting treatment and [Agent Identity]({{ site.baseurl }}/04-Cross-Cutting-Topics/Agent-Identity.html)
for the identity chain.
