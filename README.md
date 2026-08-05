# Enterprise Agent Platform — concept

A conceptual reference that guides an enterprise from **vision → enterprise capabilities →
architecture choices → implementation roadmap** for building and operating agents at
scale. Centralised and federated operating models are treated as first-class architecture
patterns, and a common capability model is agreed before branching into implementation
choices.

The content is published as a website with GitHub Pages (Jekyll + the just-the-docs
theme). Every page is plain markdown, so the repository is equally usable directly on
GitHub.

## Contents

| Section | Purpose |
| --- | --- |
| [`01-Vision/`](01-Vision/) | Vision, strategic objectives, design principles, personas, building blocks, assumptions, roadmap overview |
| [`02-Enterprise-Capabilities/`](02-Enterprise-Capabilities/) | The 24 committed capabilities, catalog and maturity model |
| [`03-Architecture-Concept/`](03-Architecture-Concept/) | Principles, shared services, control plane, hub and spoke, capability mapping |
| [`03a-Centralized-Operating-Model/`](03a-Centralized-Operating-Model/) | Centralised operating model as a complete pattern |
| [`03b-Federated-Operating-Model/`](03b-Federated-Operating-Model/) | Federated operating model as a complete pattern |
| [`04-Cross-Cutting-Topics/`](04-Cross-Cutting-Topics/) | Identity, security, observability, cost, data and model governance, compliance |
| [`05-Implementation-Patterns/`](05-Implementation-Patterns/) | Model gateway, runtime, agent, integration, API and landing zone patterns |
| [`06-Advanced-Topics/`](06-Advanced-Topics/) | Third-party platforms, multi-tenant governance, ecosystem integration, future roadmap |
| [`07-Implementation-Roadmap/`](07-Implementation-Roadmap/) | Phased plan, MVP definition, operating model decision, success metrics |
| [`assets/`](assets/) | Diagrams, images and prompt assets used across the site |

## The 24 committed capabilities

| Area | # | Capabilities |
| --- | --- | --- |
| Enterprise Platform Foundations | 1-6 | Billing and commercial management, resource organisation, roles and access, network topology, platform management, resilience |
| Governance & Security | 7-10 | Identity and trust, AI runtime protection, governance and compliance, lifecycle automation |
| Runtime & Experience | 11-14 | AI runtimes, workflow orchestration, agent memory, user experience integration |
| Intelligence | 15-18 | Model gateway, semantic foundation, knowledge and data platform, evaluation engineering |
| Interoperability | 19-21 | Tool and MCP connectivity, agent and MCP registry, enterprise capability marketplace |
| Operations | 22-24 | Observability, AI FinOps, enterprise AI enablement operating model |

## Running the site locally

```bash
bundle install
bundle exec jekyll serve
```

Then open <http://127.0.0.1:4000>.

## Publishing

Pushing to `main` runs the [`pages.yml`](.github/workflows/pages.yml) workflow, which
builds the site and deploys it to GitHub Pages. Enable Pages for the repository with
**Settings → Pages → Build and deployment → Source: GitHub Actions**.

## Contributing

- Keep every page in markdown with just-the-docs front matter (`layout`, `title`,
  `parent`, `nav_order`).
- Reference images with `{{ site.baseurl }}/assets/diagrams/...` so links work under the
  project Pages base URL.
- Diagrams live in [`assets/diagrams/`](assets/diagrams/) as SVG so they stay crisp and
  reviewable in diffs.
