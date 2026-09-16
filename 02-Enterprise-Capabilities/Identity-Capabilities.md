---
layout: default
title: Identity Capabilities
parent: Enterprise Capabilities
nav_order: 5
---

# Identity capabilities

Identity and trust (capability 7) is the control that every other control depends on. In
an agent platform, identity has to answer a harder question than in a traditional
application: *who asked, which agent acted, on whose behalf, and with what scope?*

![Agent identity chain]({{ site.baseurl }}/assets/diagrams/agent-identity.svg)
*The identity chain from user through agent and tool to enterprise resource.*

## Identity types

| Identity | Represents | Typical lifetime | Key requirement |
| --- | --- | --- | --- |
| Human identity | The requesting user | Employment | Strong authentication, conditional access |
| Agent identity | A deployed agent instance | Deployment | Distinct, discoverable, revocable |
| Tool / MCP server identity | A capability provider | Deployment | Scoped, auditable permissions |
| Platform service identity | A shared platform component | Long-lived | Least privilege, rotated credentials |

Agents must never share a human's credentials, and must never share a single
"application" identity across unrelated agents. Without distinct agent identities,
attribution and revocation are impossible.

## Delegation patterns

**On-behalf-of.** The agent acts with the requesting user's permissions. The effective
permission is the intersection of what the user may do and what the agent is allowed to
do. This is the default for productivity and self-service scenarios because it preserves
existing data access controls automatically.

**Autonomous.** The agent acts under its own identity with an explicitly granted, narrow
permission set. Used for background processing where no user is present. Requires
compensating controls: approval workflows, action limits and heightened monitoring.

**Hybrid.** The agent uses the user's identity for reads and its own identity for a small
set of pre-approved writes. Common in operational scenarios and demands very clear
audit records.

{: .highlight }
> Record both identities on every action. An audit entry showing only the agent, or only
> the user, cannot answer an incident question.

## Authorisation

Authorisation is evaluated at three points: entry (may this user use this agent?), tool
invocation (may this agent call this tool with these parameters, for this user?), and
data access (may this effective principal read this record?). Permission trimming at the
data layer is essential — filtering results after retrieval leaks information through
citations, summaries and even latency.

## Lifecycle

- **Provisioning** happens through the pipeline, alongside the agent it belongs to.
- **Registration** links the identity to the agent entry in the registry (capability 20)
  so ownership is always resolvable.
- **Rotation** of secrets is automated; prefer workload identity federation over stored
  secrets wherever the runtime supports it.
- **Revocation** is part of decommissioning. Orphaned agent identities with standing
  permissions are one of the most common findings in platform reviews.

## Operating model differences

| Aspect | Centralised | Federated |
| --- | --- | --- |
| Identity creation | Central team provisions | Domain pipelines provision within central policy |
| Permission granting | Central approval | Delegated within pre-approved scopes |
| Review | Central periodic review | Domain review, central audit sampling |
| Revocation | Central process | Automated on decommission, centrally verified |
