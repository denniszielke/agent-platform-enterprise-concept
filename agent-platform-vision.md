# Agent Platform Vision

*A narrative for why the agent platform matters, what it will enable, and how the committed 24 capabilities frame the work.*

> **Vision statement.** The Agent Platform will give the enterprise a governed, reusable and resilient foundation for building and operating agents on Microsoft Cloud. It will reduce the amount of foundational work each agent project team has to solve on its own, while increasing the trust, observability, security and accountability with which agents, agent-enabled applications, MCP servers, models, tools and data products are created and operated. The goal is not to standardise every agent or solution. The goal is to standardise the conditions under which many different agents and solutions can be built safely, quickly and repeatedly.

## Executive intent

The Agent Platform should establish an open, scalable ecosystem in which business and technology teams can compose agents, models, tools, data products and enterprise services across multiple runtimes and delivery channels. Rather than prescribing a single technology path, the platform provides shared standards, reusable capabilities and governed interoperability so that teams can select the right components for each business scenario while remaining aligned with enterprise-wide security, identity, data and operational requirements.

Its executive purpose is to accelerate value delivery in core business scenarios and enable AI transformation at enterprise scale. The platform turns common technical and governance requirements into ready-to-use foundations, allowing domain teams to focus on customer outcomes, operational improvement, new digital services and measurable business impact instead of repeatedly rebuilding infrastructure and controls.

This acceleration depends on solid IT foundations. Central platform teams provide secure cloud landing zones. Project teams consume these capabilities through streamlined onboarding and delivery paths, making the governed route the fastest route from business opportunity to production value. The 24 committed capabilities define the shared foundation required to sustain this ecosystem without constraining innovation or creating a central delivery bottleneck.

## 1. Why the Agent Platform matters

Enterprises are moving from isolated AI experiments towards portfolios of agent-enabled products, services and processes that operate across business units, data platforms, user channels and enterprise systems. The challenge is no longer to build one good agent, but to create an operating environment in which many teams can deliver many solutions without repeatedly rediscovering the same security, identity, networking, model management, observability and governance patterns. As portfolios grow, the decisive scaling constraint becomes reusable enterprise context rather than the number of available models.

Every agent needs trusted data and tools, shared business semantics, memory of tasks and history, governed actions, and decision logic for planning and approval. This context spans domain data, real-time events, documents, APIs, workflows, business processes and organisational ownership boundaries; it cannot be embedded reliably inside a model or reconstructed independently by every project. Yet teams commonly define their own protocols, discovery mechanisms, orchestration patterns, roles, accountability and interpretation of business meaning. Without a shared enterprise contract across reusable composition, organisational projections and semantics, bespoke integration, local interpretation and limited reuse become the default.

Fragmentation also weakens control. When each project makes local decisions about subscriptions, network access, model endpoints, secrets, tool connections, telemetry, cost attribution and lifecycle ownership, identity, security, governance, observability and compliance cannot consistently follow every interaction from source to context, agent and action. The enterprise then struggles to answer fundamental questions: who owns an agentic asset, what data it can reach, which identity and model it used, what it cost, how it was evaluated and what evidence exists for compliance. As more agents are introduced, point-to-point connections, duplicated interpretations and isolated control mechanisms multiply.

The Agent Platform addresses this challenge by turning foundational work and enterprise context into shared, reusable and governed capabilities. It does not remove responsibility from project teams; it allows them to focus on domain-specific value by consuming prepared building blocks for infrastructure, composition, semantics, organisational scope and controls. The acceleration mechanism is therefore not simply faster infrastructure deployment, but the governed conversion of enterprise knowledge, data, tools and process understanding into agentic solutions that can be reused and scaled with confidence.

## 2. What the agent platform will enable

| Faster solution delivery The agent platform will provide standardised onboarding paths, patterns and templates so that AI projects do not start from an empty cloud environment. Teams should receive the minimum viable foundation for identity, network integration, runtime hosting, model access, telemetry and cost attribution before they begin building solution-specific logic. | Trusted AI execution The agent platform will make identity, policy, auditability and lifecycle status visible for users, applications, agents, MCP servers, models, data products and tools. This matters because AI systems increasingly act across systems and therefore require the same level of governance discipline as other enterprise workloads, with additional controls for agentic behaviour. |
| --- | --- |
| Reusable enterprise capabilities The agent platform will make models, tools, APIs, MCP servers, data products, semantic models and agents discoverable and consumable as managed enterprise assets. Reuse should become the natural default because it is easier to consume an approved capability than to rebuild or reconnect the same capability in every project. | Measurable business impact The agent platform will capture operating signals from the beginning: usage, model consumption, token and execution patterns, cost allocation, quality, safety, performance, tool invocation, workflow state and security events. These signals allow the enterprise to move beyond adoption anecdotes and manage AI impact as an operating discipline. |

## 3. Scope of the committed 24 capabilities

The current initiative is anchored in the 24 capabilities already committed for the Enterprise Agent Platform. This is an important scoping decision. The capabilities are broader than a model gateway, broader than a Foundry setup and broader than an application runtime. They describe the full operating surface required to build, govern, secure, observe and scale AI workloads on Azure and adjacent enterprise platforms.

The capabilities can be read as a progression. The first group establishes the general cloud foundation. The second group creates the trust and governance model. The third group provides execution environments and user interaction patterns. The fourth group brings models, knowledge, data and quality engineering into the platform. The fifth group makes tools, APIs, MCP servers and agents discoverable and reusable. The final group ensures that the platform can be operated, optimised and adopted as a product.

| Domain | No. | Capability narrative |
| --- | --- | --- |
| Enterprise Platform Foundations | 1-6 | These capabilities establish the commercial, organisational, access, connectivity, operating and resilience foundation on which AI workloads depend. They make the agent platform manageable as a cloud platform rather than a collection of unrelated resources. |
| Governance & Security | 7-10 | These capabilities define the trust model for humans, applications, agents, MCP servers, tools and data sources; they also establish runtime protection, compliance evidence and lifecycle automation for AI assets. |
| Runtime & Experience | 11-14 | These capabilities provide the execution and interaction layer. They cover AI runtime choices, workflow orchestration, controlled memory and the channels through which users consume AI capabilities. |
| Intelligence | 15-18 | These capabilities provide the model, knowledge, data and evaluation foundation. They make approved models accessible through a governed gateway, ground AI behaviour in trusted data and business semantics, and establish systematic quality engineering. |
| Interoperability | 19-21 | These capabilities make enterprise functionality composable. They provide governed connectivity to APIs, tools and MCP servers, create inventories and registries for reusable AI assets, and expose those assets through a marketplace experience. |
| Operations | 22-24 | These capabilities ensure that the platform can be understood and improved over time. They cover end-to-end telemetry, AI-specific FinOps and the enablement model required for adoption across teams. |

## 4. The operating-model choice: centralised or federated

The agent platform should be positioned as an operating model, not as a single technology deployment. It supports two viable options built on the same 24 capabilities and architecture. The formal choice is made at the end of Phase 2, after the first production scenarios provide evidence about demand, risk, central capacity, domain skills and control maturity.

In the centralised model, one platform team owns the enterprise foundations and the full agent lifecycle. This maximises consistency, direct accountability and control, and remains viable where central capacity can meet demand or risk requires concentrated delivery ownership.

In the federated model, the central platform owns the responsibilities that must remain consistent: identity policy, subscription hierarchy, baseline security, network guardrails, model and tool mediation, registries, telemetry standards, compliance evidence, lifecycle automation and cost attribution. Domain teams choose appropriate solution patterns, runtimes and user experiences, then build and operate agents within those guardrails.

Both options remain viable; neither is an assumed maturity destination. The selected model should enable reusable building blocks, reference architectures and onboarding paths while producing consistent evidence for governance, operations and cost management. It should also support scenarios ranging from simple model access to container-hosted agents, MCP-based tools, workflow orchestration, human approvals, data pipelines and multi-agent collaboration without allowing each project to invent its own control plane.

## 5. Platform building blocks

The Enterprise Agent Platform is composed from six building blocks. These building blocks distribute accountability across platform teams while keeping the experience for agent project teams coherent.

Cloud Platform. The Cloud Platform provides the enterprise cloud foundation across regions. It includes subscriptions, management groups, policy, identity, Defender, cost management, DNS, routing, virtual networks, firewalls, private connectivity and resilience patterns. Its role is to make the Agent Platform inherit the same enterprise-grade foundation expected for regulated cloud workloads.

Application Platform. The Application Platform provides the runtime and integration foundation for AI workloads. It includes Azure Kubernetes Service, Azure Container Apps, integration services, messaging services, API Management and ingress patterns. Its role is to give project teams a safe place to host AI applications, agents, MCP servers and integration components inside the enterprise network.

AI Platform. The AI Platform provides governed access to models, AI services and evaluation capabilities. It includes Foundry accounts and projects, model deployments, model capacity, model gateway integration, telemetry and evaluation artefacts. Its role is to abstract raw model endpoints and provide stable, policy-controlled access to approved model capabilities.

Data Platform. The Data Platform provides the trusted data and semantic foundation. It includes Microsoft Fabric, data products, streaming and storage services, semantic models, ontologies, vector indexes, search indexes and data agents or MCP servers. Its role is to turn enterprise data into governed context that AI systems can safely retrieve, interpret and use.

Agent Control Plane. The Agent Control Plane governs agents and shared tools. It includes Agent 365 registry concepts, agent identities, metadata, lifecycle state, tool registries, MCP inventories and security or observability signals. Its role is to ensure that agents are treated as managed enterprise entities rather than hidden project artefacts.

Agent Project. The Agent Project is the delivery boundary in which a team composes approved models, tools, data, runtime components and user experiences into a real agent-enabled solution. Its role is to focus on the business outcome, the user journey, agent behavior, solution logic and the evidence that the solution is safe, useful and measurable.

## 6. What changes for agent project teams

The most important outcome is that agent project teams should no longer have to ask basic platform questions from scratch. They should not need to invent their own model endpoint strategy, decide independently how secrets are handled, create unmanaged telemetry, design their own cost attribution, register tools manually in isolated documents or negotiate basic network patterns for every workload. Those concerns should be prepared by the agent platform and exposed through clear onboarding paths.

In the target state, a project team begins with a known agent platform pattern. It receives an approved runtime option, a managed identity model, private connectivity patterns, access to the model gateway, approved data or tool interfaces, logging and tracing requirements, deployment automation and clear ownership metadata. The team’s real work then becomes the high-value work: selecting the relevant business process, designing the user experience, composing agents or workflows, grounding the solution in trusted data, validating quality and proving impact.

This is also where the vision becomes compelling for an enterprise organisation betting on Microsoft Cloud. Azure provides the foundation for secure workload execution and integration, Foundry provides the centre of gravity for model and AI development, Fabric provides the data and semantic foundation, and Agent 365, Entra, Purview and Defender concepts frame the governance and control model for agents and AI assets. The agent platform connects these capabilities into one operating environment rather than leaving every project to assemble them independently.

## 7. Assumptions and design boundaries

Regional foundation with multi-region consumption patterns. The cloud platform is assumed to be available across multiple regions, while individual workloads are deployed and consumed as regional resources. For AI model consumption, the model gateway can abstract model hosting across regions where capacity and redundancy require it.

Private enterprise network as the default. The design assumes private network consumption for most enterprise resources, including models, gateways, agents, MCP servers, data stores and platform services, unless a specific scenario requires public exposure for user access or external integration.

Container-hosted agents are in scope. The agent platform assumes that application hosting for custom agents and MCP servers will primarily use containers, with Azure Kubernetes Service and Azure Container Apps as the main runtime options. Foundry prompt agents are not treated as the primary scoped runtime where current limitations make container-hosted agents the required pattern.

Managed identities over shared secrets. Key-based authentication for models, storage accounts and other resources is assumed to be denied by policy where possible. User-assigned managed identities, workload identities and Entra-based authentication patterns are the preferred control model.

Hub-and-spoke topology with shared control points. The network topology is assumed to follow a hub-and-spoke model. Centrally managed components such as firewalls and shared egress live in the hub, while application, data and AI platforms are implemented as spokes with defined ingress and egress patterns.

Gateway-mediated access where it creates control value. The model gateway and API/MCP gateway patterns are assumed to be the supported path for many model, tool and MCP consumption scenarios. Exceptions can exist, but they should be deliberate architecture decisions with explicit control, telemetry and cost trade-offs.

## 8. Success criteria for the vision

The vision should be considered successful when the Agent Platform is not perceived as extra governance overhead, but as the fastest and safest path to production. AI project teams should experience the platform as an accelerator. Risk, security, data, platform and operations teams should experience it as a way to make control evidence consistent and repeatable. Business sponsors should experience it as a way to turn AI investment into visible outcomes rather than isolated experiments.

- There is a clear onboarding path for new AI projects, including which runtime, identity, network, telemetry, model gateway and data access patterns are available.

- The 24 committed capabilities are mapped to accountable platform owners, implementation artefacts and decision points, so that scope is not only documented but operationally actionable.

- Every reusable AI asset, including models, MCP servers, tools, data products and agents, has ownership, classification, lifecycle state, approved consumer scope and telemetry expectations.

- Model and tool consumption can be attributed to projects, teams, agents or workloads with enough fidelity to support FinOps, optimisation and governance discussions.

- Quality, safety and performance evaluation are integrated into the lifecycle rather than treated as after-the-fact documentation.

- The agent platform evolves as a product with a roadmap, feedback loops, service catalogue, enablement material and reusable templates.

## Closing narrative

The Agent Platform is the enterprise mechanism for scaling AI with confidence. It creates the foundation that lets teams move from proof-of-concept thinking to repeatable delivery. It respects the reality that different AI scenarios require different runtimes, channels, models, data products and orchestration patterns. At the same time, it draws a clear line around the responsibilities that cannot be left to each project: identity, policy, security, governance, lifecycle, telemetry, cost and evidence.

For an enterprise betting on Microsoft Cloud, this is where the platform story becomes powerful. The agent platform connects Azure, Foundry, Fabric, Agent 365, Entra, Purview, Defender, API Management and the surrounding developer and operational tooling into a coordinated operating environment. The result is not just a better way to host AI workloads. It is a better way to turn enterprise data, process knowledge, reusable tools and agent capabilities into governed business impact at scale.