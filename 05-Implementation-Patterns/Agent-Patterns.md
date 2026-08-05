---
layout: default
title: Agent Patterns
parent: Implementation Patterns
nav_order: 3
---

# Agent Patterns

Agent patterns are reusable architectural blueprints that describe how to structure AI agents to accomplish common classes of task. Choosing the right pattern—or composing several patterns—helps development teams deliver capable agents while maintaining the properties the enterprise requires: predictable behaviour, testability, observability, and governance compliance. This page catalogues the principal patterns available on the platform and provides guidance on their application.

## Supported Capabilities

| Capability | Agent Patterns Role |
|---|---|
| **11 – AI Runtimes** | The runtime environment in which patterns execute |
| **12 – Workflow Orchestration** | Provides the durable execution layer for orchestrator patterns |
| **13 – Agent Memory** | Memory primitives consumed by pattern implementations |
| **19 – Tool & MCP Connectivity** | Tool invocation layer used by ReAct and tool-use patterns |
| **20 – Agent & MCP Registry** | Registry where pattern-based agents are registered |
| **15 – Model Gateway** | Model inference endpoint for all patterns |

## Pattern 1 – ReAct (Reason + Act)

The ReAct pattern interleaves reasoning and action steps. The agent iterates: think → select tool → call tool → observe result → think again, until a stopping condition is met.

**When to use**: open-ended tasks where the sequence of tool calls cannot be predetermined; tasks that require adapting the plan based on intermediate results.

**Implementation structure**:
```
loop:
  1. Generate next reasoning step (Thought)
  2. If done: return final answer
  3. Select and call tool (Action)
  4. Append tool result to context (Observation)
  5. Repeat
```

**Considerations**:
- Set a maximum iteration limit to prevent runaway loops.
- Each iteration consumes tokens; monitor cost per task.
- Log each Thought/Action/Observation cycle to the reasoning trace (see [Observability](../04-Cross-Cutting-Topics/Observability.md)).

## Pattern 2 – Plan-and-Execute

The agent first generates a complete plan (a list of steps), then executes each step, potentially using sub-agents or tools.

**When to use**: tasks with a predictable structure that benefits from upfront planning; when steps can be parallelised; when the plan needs human review before execution.

**Implementation structure**:
```
1. Planner agent → produces structured plan (list of steps with dependencies)
2. Human review step (optional, for high-risk use cases)
3. Executor: for each step in topological order:
   a. Invoke step-specific agent or tool
   b. Capture result
4. Synthesiser agent → combines results into final output
```

**Considerations**:
- The planner and executor can use different models (planner uses frontier model; executor uses cheaper tier).
- Parallelism in the execution phase reduces end-to-end latency.
- Durable workflow runtime (Pattern 3 from [Runtime Patterns](./Runtime-Patterns.md)) is recommended for long-running plans.

## Pattern 3 – Orchestrator-Worker

A central orchestrator agent delegates subtasks to specialised worker agents. Each worker has a narrow, well-defined capability.

**When to use**: complex tasks that decompose naturally into specialised domains; when different domains require different model configurations, tools, or data access permissions.

**Topology**:
```
User Request → Orchestrator
                 ├─ Worker: DataAnalyst (has SQL tool access)
                 ├─ Worker: Researcher (has web search + vector retrieval)
                 ├─ Worker: Writer (optimised for long-form generation)
                 └─ Worker: Reviewer (evaluates outputs)
```

**Considerations**:
- The orchestrator should not expose sensitive data from one worker to another without authorisation.
- Each worker should have its own identity and minimum-privilege access.
- Implement a circuit breaker: if a required worker is unavailable, the orchestrator should fail gracefully rather than hanging.

## Pattern 4 – RAG (Retrieval-Augmented Generation)

The agent retrieves relevant documents from a knowledge store and includes them as context in the model prompt. This grounds the agent's responses in authoritative, up-to-date enterprise knowledge.

**When to use**: question answering over enterprise documents; policy or procedure lookup; any task where the model's training knowledge is insufficient or may be outdated.

**Implementation structure**:
```
1. Embed user query
2. Retrieve top-k chunks from vector store (applying access control)
3. Rerank results for relevance
4. Construct prompt: system + retrieved context + user query
5. Generate response
6. (Optional) Validate response against source chunks; add citations
```

**Considerations**:
- Apply document-level access control at retrieval time (see [Data Governance](../04-Cross-Cutting-Topics/Data-Governance.md)).
- Monitor retrieval quality metrics: precision, recall, and citation accuracy.
- Tune chunk size and overlap for the domain; too small loses context, too large dilutes signal.
- Semantic reranking (cross-encoder) significantly improves result quality at modest cost.

## Pattern 5 – Evaluator-Optimizer Loop

The agent generates an output, an evaluator agent scores it against defined criteria, and the optimizer uses the feedback to improve the output. The loop continues until quality thresholds are met.

**When to use**: high-stakes content generation; code generation with automated testing; structured document production where format and content requirements are well-defined.

**Implementation structure**:
```
loop (max N iterations):
  1. Generator agent → produces candidate output
  2. Evaluator agent → scores candidate against rubric
  3. If score ≥ threshold: return output
  4. Optimizer agent → generates improvement instructions
  5. Feed instructions back to Generator
```

**Considerations**:
- Define clear, measurable evaluation criteria before implementation.
- Track iteration count and cost per task in observability.
- Set a minimum quality threshold below which tasks are escalated to human review rather than looped indefinitely.

## Pattern 6 – Human-in-the-Loop

Explicit human review and approval steps are embedded in the agent workflow. Essential for high-risk decisions, regulated processes, and actions with significant real-world consequences.

**When to use**: financial transactions above a threshold; employee HR actions; actions that cannot be reversed; any use case classified as high-risk under the EU AI Act.

**Implementation**:
- Model as a durable workflow task with async human completion.
- Provide the reviewer with full context: agent reasoning trace, proposed action, confidence score.
- Implement a timeout with a safe default action (escalate, reject, or pause) if no human response is received.
- Log the human decision (approve/reject/modify) and the reviewer's identity in the audit trail.

{: .note }
> Human-in-the-loop is not a workaround for poor agent quality—it is a deliberate governance control. Design the agent to make the reviewer's task efficient: present the most relevant information clearly, pre-highlight uncertainties, and minimise the effort required to make an informed decision.

## Pattern 7 – Memory-Augmented Agent

The agent maintains persistent state across sessions using the platform's memory primitives (Capability 13).

| Memory Type | Implementation | Use Case |
|---|---|---|
| **In-context** | Include history in prompt context window | Single-session conversation continuity |
| **External episodic** | Retrieve relevant past sessions from vector store | Cross-session personalisation, continuity |
| **Semantic / entity** | Structured knowledge graph or KV store | Persistent facts about entities (users, projects, decisions) |
| **Procedural** | Retrieved tool-use examples or past plans | Improving tool selection accuracy over time |

**Considerations**:
- Apply data governance and retention policies to memory stores (see [Data Governance](../04-Cross-Cutting-Topics/Data-Governance.md)).
- Implement memory summarisation to prevent unbounded growth of stored sessions.
- Memory retrieval is a latency-critical path; use indexed vector stores with appropriate hardware.

## Centralized vs. Federated Pattern Adoption

| Dimension | Centralized | Federated |
|---|---|---|
| Pattern Library | Central platform team maintains approved pattern library | Centre publishes reference patterns; domain teams extend and contribute back |
| Implementation Standardisation | Central team enforces specific framework versions | Domain teams choose frameworks; must meet observability and security interface standards |
| Cross-domain Agents | Orchestrated through central platform | Agents from different domains collaborate via registry and published interfaces |

See [Centralized Operating Model](../03a-Centralized-Operating-Model/Overview.md) and [Federated Operating Model](../03b-Federated-Operating-Model/Overview.md).

## Summary

The seven patterns described here cover the majority of enterprise agent use cases. Start with the simplest pattern that meets the requirements—often RAG or synchronous ReAct—and graduate to more complex patterns (orchestrator-worker, evaluator-optimizer, human-in-the-loop) as complexity, risk, and scale demand. Instrument every pattern consistently and register every agent in the platform registry before promotion to production.
