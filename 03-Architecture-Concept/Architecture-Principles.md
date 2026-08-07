---
layout: default
title: Architecture Principles
parent: Architecture Concept
nav_order: 1
---

# Architecture principles

These principles extend the [design principles]({{ site.baseurl }}/01-Vision/Design-Principles.html)
into concrete architectural constraints. Each one is stated as a rule, a rationale and an
implication that a reviewer can test a design against.

## Canonical principles

| Principle | Rule | Testable implication |
| --- | --- | --- |
| Centralize systemic controls and federate solution delivery | Identity policy, model mediation, registry, network baselines, security signals and minimum telemetry are central; business logic, prompts, tools, experience and scenario evaluation remain with the Agent Project | A project can release business behavior independently but cannot weaken enterprise controls |
| Treat every agent as a workload identity | Human identity, agent identity and runtime identity are related but not interchangeable | Authorization and traces identify the initiating actor, registered agent and deployed workload |
| Separate model, agent and tool gateways logically | Each traffic domain has different policies, owners, scaling characteristics and audit requirements | Shared APIM infrastructure still uses separable products, hostnames, policy fragments and telemetry dimensions |
| Keep business process state out of prompts | Durable progression, approvals, retries and compensating actions belong in workflow and state services | A process can recover correctly without reconstructing state from conversation history |
| Use private connectivity by default | Public ingress is deliberate, authenticated and protected; platform traffic stays on private enterprise paths where supported | No model, data, tool or runtime backend is reachable directly from the Internet without an approved exception |
| Deny static keys where managed identity is supported | Workloads use managed identity or workload identity federation; remaining credentials are stored and rotated in Key Vault | No credential is embedded in code, prompts, agent instructions or deployment configuration |
| Make write actions explicit | Read and transaction tools use different scopes, policies and approval requirements | High-impact writes use deterministic validation, idempotency and human confirmation where required |
| Evaluate the system, not only the model | Acceptance covers retrieval, tool selection, authorization, workflow outcome, latency, cost and safety | A model-only benchmark cannot satisfy the release gate for a production agent |
| Design regional workloads for failure | Every regional project and dependency declares failover, degradation and recovery behavior | Recovery testing includes identity, DNS, gateways, state and data dependencies, not only compute |
| Automate registration and evidence | Deployment pipelines update inventory and attach evaluation, security and operational evidence to the released version | The deployed image, configuration, identities, dependencies and evidence resolve from one release record |

## Review checklist

| Question | Principle |
| --- | --- |
| Which behavior is central policy and which is project-owned? | Central controls, federated delivery |
| Which user, agent and workload identities perform each action? | Agent workload identity |
| Are experience, model, tool and data policies independently operable? | Gateway separation |
| Where does workflow state live and how does it recover? | State outside prompts |
| Which endpoints are public, and why? | Private connectivity |
| Can every remaining credential be justified and rotated? | Credentialless access |
| Can a proposed write be validated, approved, deduplicated and audited? | Explicit writes |
| Does evaluation cover the complete task and its dependencies? | System evaluation |
| What fails over, degrades or stops during a regional outage? | Regional failure design |
| Does the release pipeline update inventory and evidence automatically? | Automated registration |
