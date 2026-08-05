---
layout: default
title: Data Platform Integration
parent: Architecture Concept
nav_order: 5
---

# Data platform integration

Agents are only as good as the enterprise knowledge they can reach — and only as safe as
the controls that travel with it. This page describes how the agent platform integrates
with the enterprise data estate rather than building a parallel one.

## Principle: consume data products, do not copy data

The default integration pattern is to consume governed data products that already carry
an owner, a freshness contract, lineage, retention rules and access controls. Copying
source data into an agent-specific store breaks all four properties simultaneously and
creates a second governance problem nobody owns.

| Pattern | When appropriate | Risks to manage |
| --- | --- | --- |
| Query the source system through an API | Small, structured, permission-sensitive data | Latency, rate limits |
| Consume a governed data product | Most analytical and reference data | Freshness contract must be explicit |
| Index content into a search service | Large document corpora | Permission trimming and deletion propagation |
| Graph-augmented retrieval | Relationship-heavy questions | Build and maintenance effort |
| Direct database access from an agent | Rarely appropriate | Bypasses governance; requires strong justification |

## Permission trimming

Retrieval must apply the effective principal's permissions at query time. Post-filtering
retrieved results leaks information through summaries, citations and even response
latency. When an index is used, access control metadata is indexed alongside content and
applied as a query filter, and access changes in the source must propagate on a defined
schedule.

## Freshness and deletion

Every grounding source declares how fresh it is and how quickly deletions propagate.
Users interpret an agent's answer as current, so a corpus that lags by a week must say so
in the experience. Deletion propagation is a compliance requirement: content removed at
source must disappear from indexes, caches and agent memory within a defined window.

{: .warning }
> Caches and agent memory are grounding stores too. Include them in retention, deletion
> and access-review scope.

## Quality signals

Retrieval quality problems usually present as model problems. Instrument retrieval
directly: hit rate, citation coverage, retrieved-chunk relevance, and the share of
answers that fall back to model knowledge without a source. These signals belong in the
same dashboards as latency and cost.

## Responsibilities

| Responsibility | Data platform | Agent platform |
| --- | --- | --- |
| Source ingestion and quality | Owns | Consumes |
| Access control definition | Owns | Enforces at query time |
| Semantic definitions | Co-owns | Applies in prompts and tools |
| Chunking and embedding strategy | Advises | Owns |
| Retrieval evaluation | Contributes datasets | Owns harness and gates |
| Retention and deletion | Owns policy | Propagates to indexes, caches and memory |

Clear split of ownership avoids the common failure where retrieval quality is everyone's
concern and nobody's job.
