---
layout: default
title: Data Platform
parent: Architecture Concept
nav_order: 5
---

# Data platform

Agents are only as good as the enterprise knowledge they can reach — and only as safe as
the controls that travel with it. The Data Platform makes trusted data, semantic meaning,
retrieval, session state and memory consumable through governed contracts rather than
allowing every Agent Project to build unmanaged point-to-point access.

In the Microsoft Cloud realisation, Microsoft Fabric provides the centre of gravity for
governed data products, semantic models and analytical context. The agent platform
consumes those assets through governed interfaces and can also use Azure data, streaming,
search and storage services where the scenario requires them.

## Service contract

| Platform area | Reference implementation | Contract to Agent Projects |
| --- | --- | --- |
| Analytical data products | Fabric, OneLake and governed workspaces | Owned data product with classification, lineage, freshness and access policy |
| Semantic foundation | Fabric semantic models, Purview glossary, ontologies and graph stores | Versioned business concepts, metrics and relationships with named owners |
| Retrieval and vector services | Azure AI Search | Search endpoint, index contract, freshness target and authorization behavior |
| Operational stores | Azure SQL, Cosmos DB, ADLS and approved databases | Data contract, transaction semantics and recovery commitment |
| Session and memory | Redis, Cosmos DB, SQL or AI Search | Scoped store with purpose, retention class and deletion interface |
| Data agents and data MCP | Fabric data agents or governed container-hosted MCP servers | Versioned tool schema, authorization scopes, output classification and SLO |

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

## Session, workflow state and memory

Projects classify state before selecting a store. Redis holds low-latency, replaceable
session context with short expiry. Cosmos DB supports durable JSON state and elastic or
multi-region scale. SQL is preferred when transactions, relational constraints and
auditable workflow state dominate. AI Search holds derived semantic memory retrieved by
meaning; it is not the authoritative business record.

Durable Functions or Logic Apps own workflow progression, approvals, retries and
compensating actions. Their state is not reconstructed from conversational memory. Raw
conversation history is not automatically long-term memory: durable memory requires an
approved purpose, identity scope, retention rule, inspection path and deletion behavior.
Each datum has one authoritative store and one retention class even when a project uses
several storage technologies.

## Quality signals

Retrieval quality problems usually present as model problems. Instrument retrieval
directly: hit rate, citation coverage, retrieved-chunk relevance, and the share of
answers that fall back to model knowledge without a source. These signals belong in the
same dashboards as latency and cost.

## Responsibilities

| Responsibility | Data Platform | Agent Project |
| --- | --- | --- |
| Source ingestion and quality | Owns | Consumes |
| Access control definition | Owns | Enforces at query time |
| Semantic definitions | Co-owns | Applies in prompts and tools |
| Chunking and embedding strategy | Advises | Owns |
| Retrieval evaluation | Contributes datasets | Owns harness and gates |
| Retention and deletion | Owns policy | Propagates to indexes, caches and memory |

Clear split of ownership avoids the common failure where retrieval quality is everyone's
concern and nobody's job.
