---
layout: default
title: Capability Model
parent: Enterprise Capabilities
nav_order: 1
---

# Capability model

The capability model is the common language of the platform. It describes *what* the
enterprise must be able to do to build, run and govern agents at scale, without
committing to *how* any of it is implemented. Agreeing this model first is what allows
the same conversation to serve an executive sponsor, a security officer and a domain
engineer.

![Capability model]({{ site.baseurl }}/assets/diagrams/capability-model.svg)
*Six capability areas containing the 24 committed capabilities.*

## Structure

| # | Capability area | Capabilities | Question it answers |
| --- | --- | --- | --- |
| 1 | Enterprise Platform Foundations | 1-6 | Where do agents run, and on whose account? |
| 2 | Governance & Security | 7-10 | Who is allowed to do what, and how is it proven? |
| 3 | Runtime & Experience | 11-14 | How are agents executed and delivered to users? |
| 4 | Intelligence | 15-18 | How are models, meaning and knowledge supplied? |
| 5 | Interoperability | 19-21 | How are tools, agents and services discovered and reused? |
| 6 | Operations | 22-24 | How is the platform run, funded and improved? |

The 24 capabilities are *committed*: they are the shared foundation the enterprise
agrees to provide. They are not a delivery backlog — several will be satisfied by
existing enterprise services long before the agent platform exists.

## How to use the model

1. **Baseline.** For each capability, record whether it exists today, who owns it and
   how mature it is (see the [capability maturity model](Capability-Maturity-Model.md)).
2. **Scope.** Map the business scenarios in flight to the capabilities they depend on.
   Capabilities that no in-flight scenario needs are deferred, not deleted.
3. **Assign.** Decide, per capability, whether it is owned centrally, owned by domains,
   or shared. This assignment *is* the operating model choice.
4. **Realise.** Only now map capabilities to products and services, using the
   [capability to architecture mapping]({{ site.baseurl }}/03-Architecture-Concept/Capability-to-Architecture-Mapping.html).
5. **Review.** Re-baseline at each roadmap phase boundary.

{: .highlight }
> Step 3 is the decision most often made by accident. Writing the ownership column
> explicitly turns an implicit product decision into a deliberate operating-model choice.

## Why capabilities precede implementation

- **Comparability.** Centralised and federated designs can be evaluated against the same
  list rather than against each other's technology preferences.
- **Reversibility.** When a runtime or model vendor changes, the capability stays; only
  its realisation moves.
- **Coverage.** Gaps become visible as empty cells rather than as production incidents.
- **Shared language.** Business, security and engineering stakeholders can all locate
  their concerns in the same model.

## Ownership patterns

| Capability area | Typically centralised | Typically federated |
| --- | --- | --- |
| Enterprise Platform Foundations | Almost always central | Rarely |
| Governance & Security | Central policy, distributed enforcement | Enforcement in domain pipelines |
| Runtime & Experience | Central shared runtimes | Domain-owned runtimes on shared templates |
| Intelligence | Central gateway and evaluation standards | Domain-owned prompts, data products |
| Interoperability | Central registry and marketplace | Domain-published components |
| Operations | Central standards and tooling | Domain-owned on-call and budgets |

Most enterprises end up with a mixed picture, and that is expected. What matters is that
each capability has exactly one accountable owner.
