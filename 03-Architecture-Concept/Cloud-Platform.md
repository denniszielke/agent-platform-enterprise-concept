---
layout: default
title: Cloud Platform
parent: Architecture Concept
nav_order: 2
---

# Cloud platform

The Cloud Platform provides the multi-region Azure foundation and enforces controls that
must remain consistent across every platform service and Agent Project. It owns resource
hierarchy, shared connectivity, identity foundations, policy, security posture, cost
governance and resilience patterns. Agent teams consume these capabilities through a
prepared project boundary rather than designing a landing zone for each solution.

## Service contract

| Platform area | Reference implementation | Contract to platform and project teams |
| --- | --- | --- |
| Tenant and hierarchy | Microsoft Entra tenant, management groups, subscriptions and resource groups | Approved resource boundary with named owner, environment and cost centre |
| Network topology | Virtual WAN or hub VNets, spoke VNets, peering, Private Link and Private DNS | Routable project subnets, private name resolution and declared ingress and egress paths |
| Identity foundation | Entra groups, managed identities, workload identity federation, PIM and access reviews | Project groups and identities with least-privilege assignments and accountable owners |
| Policy and security baseline | Azure Policy, Defender for Cloud, Key Vault and resource locks | Inherited guardrails, compliance status and an explicit exception process |
| Cost and resource governance | Cost Management, budgets, tags and Resource Graph | Budget, alert recipients and project-level cost views |
| Resilience foundation | Availability Zones, selected secondary regions and backup policies | Published availability, recovery and regional dependency commitments |

## Ownership boundary

The Cloud Platform team owns the management hierarchy, hub connectivity, shared DNS,
policy definitions and identity foundation. Application, AI and Data Platform teams own
their spoke resources and comply with inherited controls. Agent Project teams receive
delegated access to their boundary but cannot alter enterprise policy, hub routing or
shared DNS.

## Regional foundation

Hub-and-spoke connectivity is established per supported geography or region. Shared DNS,
inspection and egress controls reside in the hub; platform services and projects attach
through spokes or delegated subnets. Private connectivity is the default where Azure
services support it. Public exposure is deliberate, authenticated and terminated at an
approved edge or application ingress service.

Resilience is expressed through service tiers rather than one maximum design. Every
platform and project dependency declares zone behavior, regional failover or degradation,
backup and recovery expectations, and a tested ownership path for an outage.
