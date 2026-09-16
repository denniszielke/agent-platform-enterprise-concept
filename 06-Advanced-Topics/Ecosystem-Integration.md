---
layout: default
title: Ecosystem Integration
parent: Advanced Topics
nav_order: 3
---

# Ecosystem Integration

The enterprise agent platform does not operate in isolation. It exists within a broader ecosystem of developer tools, data platforms, enterprise applications, cloud services, and emerging AI standards. Ecosystem integration is the discipline of connecting the platform to this ecosystem in a governed, maintainable, and interoperable way—enabling the organisation to leverage the best available capabilities without creating brittle, ungoverned point-to-point integrations.

## Supported Capabilities

| Capability | Ecosystem Integration Role |
|---|---|
| **19 – Tool & MCP Connectivity** | Protocol layer for ecosystem tool integration |
| **20 – Agent & MCP Registry** | Discovery and governance of ecosystem integrations |
| **21 – Enterprise Capability Marketplace** | Shared library of ecosystem connectors and building blocks |
| **16 – Semantic Foundation** | Shared ontology enables semantic interoperability across ecosystem components |
| **12 – Workflow Orchestration** | Orchestration of cross-ecosystem workflows |
| **24 – Enterprise AI Enablement Operating Model** | Organisational structures that govern ecosystem relationships |

## Ecosystem Landscape

The enterprise AI ecosystem spans several layers:

### Developer Tooling and Frameworks

| Category | Examples | Integration Approach |
|---|---|---|
| Agent orchestration frameworks | LangChain, Semantic Kernel, LlamaIndex, CrewAI | Publish instrumented base libraries; enforce OTel integration |
| IDE and development tools | VS Code Copilot, Cursor, GitHub Copilot | MCP servers expose platform capabilities to developer tools |
| CI/CD platforms | GitHub Actions, Azure DevOps, GitLab CI | Pipeline templates for agent deployment, evaluation, and registry publication |
| Container platforms | Kubernetes, Azure Container Apps, AWS ECS | Standard Helm charts and manifests for agent deployment |

### Data and Knowledge Ecosystem

| Category | Examples | Integration Approach |
|---|---|---|
| Data platforms | Databricks, Snowflake, Azure Synapse | Lakehouse connectors; MCP servers for query interfaces |
| Document management | SharePoint, Confluence, Notion | Ingestion pipelines to knowledge platform; MCP read tools |
| Communication platforms | Microsoft Teams, Slack, email | Agent surface integration; event-driven triggers |
| CRM / ERP | Salesforce, SAP, Dynamics 365 | MCP servers wrapping REST/OData APIs; event stream integration |

### AI and Model Ecosystem

| Category | Examples | Integration Approach |
|---|---|---|
| Model providers | Azure OpenAI, Anthropic, Cohere, Meta (open-weight) | Routed through Model Gateway; normalised via OpenAI-compatible API |
| Vector stores | Azure AI Search, Pinecone, Weaviate, pgvector | Abstracted behind Knowledge Platform (Capability 17) |
| Embedding services | Platform-hosted and provider-hosted | Managed through Model Gateway embedding endpoint |
| Evaluation platforms | Azure AI Evaluation, Langfuse, Ragas | Evaluation harness integration; metric export to observability pipeline |

## MCP as the Ecosystem Integration Protocol

Model Context Protocol (MCP) is the primary mechanism for exposing ecosystem capabilities to agents in a structured, discoverable way. Its adoption across a wide range of vendors and open-source projects makes it the recommended integration protocol for the platform.

### MCP Integration Patterns

**Vendor-provided MCP servers**: growing numbers of enterprise software vendors publish official MCP servers for their platforms. The platform's procurement process should evaluate MCP support as a factor in vendor selection.

**Platform-published MCP servers**: the platform team publishes MCP servers for shared enterprise capabilities (HR systems, finance systems, ITSM platforms) in the Enterprise Capability Marketplace.

**Community MCP servers**: open-source MCP servers are available for many common integrations. These must pass the platform's security vetting process before use (see [Third-Party Platforms](./Third-Party-Platforms.md)).

**Custom MCP servers**: domain teams author MCP servers for their own systems, following platform standards and registering them in the registry.

### MCP Server Quality Standards

All MCP servers published to the marketplace must meet:

- Accurate, unambiguous tool descriptions that agents can rely on for correct tool selection
- Comprehensive input validation with informative error messages
- Idempotency declarations (tools that have side effects are clearly labelled)
- Authentication and authorisation implementation verified by security review
- Semantic versioning with change logs
- Unit and integration tests with coverage requirements
- OpenTelemetry instrumentation for observability

## Semantic Interoperability

As the number of ecosystem integrations grows, semantic consistency becomes a challenge: different systems use different terminology for the same concepts. The Semantic Foundation (Capability 16) addresses this by providing a shared ontology that:

- Defines canonical entity types (Customer, Product, Order, Employee) and their attributes
- Maps between the naming conventions of different systems (e.g., Salesforce `Account` = SAP `Customer` = platform `Organisation`)
- Enables agents to reason about cross-system concepts without needing to know each system's internal terminology
- Supports federated search across multiple data sources using a common vocabulary

Ecosystem integrations should map their data models to the platform ontology at the integration layer, rather than expecting agent prompts to handle the mapping.

## Developer Ecosystem and Toolchain Integration

The platform should provide first-class support for the developer tools that agent developers use daily:

### IDE Integration via MCP

Expose platform capabilities (agent registry, evaluation results, knowledge search) as MCP servers consumable by AI-powered IDEs. This enables developers to:

- Query the agent registry for available sub-agents while writing orchestrator code
- Review evaluation results for a model version while deciding whether to upgrade
- Search the knowledge platform while authoring prompts

### CI/CD Pipeline Integration

Provide reusable pipeline templates for:

- Agent container image build, scan, and sign
- Automated evaluation on a held-out test set as a quality gate
- Registry publication on successful promotion
- Policy compliance check (IaC scanning, secret detection, approved base image verification)

### GitHub / Source Control Integration

- Agent registry entries link to their source repository for traceability
- Pull request checks include evaluation regression detection
- Dependabot or equivalent keeps agent dependencies current

## Centralized vs. Federated Ecosystem Integration

| Dimension | Centralized | Federated |
|---|---|---|
| Integration Ownership | Central platform team owns all ecosystem integrations | Domain teams own integrations for their systems; publish to marketplace |
| Marketplace Curation | Central team curates all marketplace assets | Domain teams publish; central team quality-gates "approved" tier |
| Ontology Maintenance | Central team maintains the shared ontology | Domain teams contribute domain extensions; central team governs the core |
| Vendor Relationships | Central team manages all vendor relationships | Domain teams manage domain vendors; centre manages strategic platform vendors |

See [Centralized Operating Model](../03a-Centralized-Operating-Model/Overview.md) and [Federated Operating Model](../03b-Federated-Operating-Model/Overview.md).

## Ecosystem Health Management

As the ecosystem grows, active management prevents integration sprawl:

- **Registry hygiene**: quarterly review of registry entries; deprecate and remove unused integrations.
- **Dependency tracking**: maintain a graph of which agents depend on which ecosystem integrations. Use this for impact analysis before deprecating an integration.
- **Vendor health monitoring**: track the health, SLA, and roadmap of key ecosystem vendors; flag risks in the platform risk register.
- **Standards evolution**: monitor MCP, OpenTelemetry, and other protocol standards for breaking changes; plan platform updates on each major release.

## Summary

Ecosystem integration, anchored on MCP for tool connectivity, semantic interoperability through the platform ontology, and a curated Enterprise Capability Marketplace, enables the organisation to benefit from the richness of the AI and enterprise software ecosystem without losing governance control. Active management of the integration estate—registry hygiene, dependency tracking, and vendor health monitoring—ensures the platform remains maintainable as the ecosystem evolves.
