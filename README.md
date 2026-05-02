# Naftiko (naftiko)
Naftiko provides spec-driven integration tooling that turns existing data and APIs into governed capabilities for AI. The platform centers on a YAML capability specification, an open-source framework that runs those specs as live MCP, SKILL, and REST services, and Fleet — a governed runtime that makes capabilities discoverable, composable, observable, and cost-bounded.

**URL:** [Visit APIs.json URL](https://raw.githubusercontent.com/api-evangelist/naftiko/refs/heads/main/apis.yml)

**Run:** [Capabilities Using Naftiko](https://github.com/naftiko/fleet?utm_source=api-evangelist&utm_medium=readme&utm_campaign=company-api-evangelist&utm_content=repo)

## Tags:

 - AI, API Integration, Capabilities, Governance, MCP, Spec-Driven Integration

## Timestamps

- **Created:** 2026-05-01
- **Modified:** 2026-04-28

## APIs

### Naftiko Framework
The Naftiko Framework is an open-source (Apache 2.0) engine for spec-driven integration. It interprets YAML capability specifications and exposes them as live multi-protocol services — including MCP, SKILL, and REST adapters — without requiring Java code. The framework supports OpenAPI 2.0/3.0/3.1 import, format conversion across Protobuf, XML, YAML, CSV, Avro, and JSON, OpenTelemetry tracing, authentication, and inline templating with Mustache and JSONPath.

**Human URL:** [https://github.com/naftiko/framework](https://github.com/naftiko/framework)

#### Tags:

 - Framework, MCP, Open Source, REST, SKILL, Spec-Driven

#### Properties

- [Documentation](https://github.com/naftiko/framework/wiki)
- [GitHubRepository](https://github.com/naftiko/framework)
- [GettingStarted](https://github.com/naftiko/framework/wiki/Installation)
- [Tutorials](https://github.com/naftiko/framework/wiki/Tutorial-Part-1)
- [APIReference](https://github.com/naftiko/framework/wiki/Specification)
- [ReleaseNotes](https://github.com/naftiko/framework/wiki/Release-Notes)
- [FAQ](https://github.com/naftiko/framework/wiki/FAQ)

### Naftiko Fleet
Naftiko Fleet is the governed runtime for spec-driven integration. It makes capabilities discoverable through an inventory, composable across domains, observable through telemetry, and cost-bounded through policy controls. Fleet is offered as a Community Edition (freeware) with Standard and Enterprise editions planned, and ships with a VS Code extension and Backstage templates for editing, linting, and cataloguing capabilities.

**Human URL:** [https://github.com/naftiko/fleet](https://github.com/naftiko/fleet)

#### Tags:

 - Backstage, Fleet, Governance, IDE, Runtime, VS Code

#### Properties

- [Documentation](https://naftiko.io)
- [GitHubRepository](https://github.com/naftiko/fleet)
- [IDESupport](https://github.com/naftiko/fleet)
- [Integrations](https://github.com/naftiko/fleet)

### Naftiko Capability Specification
The Naftiko Capability Specification is a YAML-based contract for declaring integration capabilities. Specs are validated by JSON Schema and committed to Git, producing versioned, reviewable artifacts that the framework executes and Fleet governs. The specification covers consume adapters (HTTP, OpenAPI), expose adapters (MCP, SKILL, REST), data transformations, authentication, and domain-driven aggregates.

**Human URL:** [https://github.com/naftiko/framework/wiki/Specification](https://github.com/naftiko/framework/wiki/Specification)

#### Tags:

 - JSON Schema, Specification, YAML

#### Properties

- [APIReference](https://github.com/naftiko/framework/wiki/Specification)
- [BestPractices](https://github.com/naftiko/framework/wiki/Linting-Guide)
- [UseCases](https://github.com/naftiko/framework/wiki/Use-Cases)

## Common Properties

- [Documentation](https://naftiko.io)
- [GitHubOrganization](https://github.com/naftiko)
- [GettingStarted](https://github.com/naftiko/framework/wiki/Installation)
- [Blog](https://naftiko.io)
- [Support](https://github.com/naftiko/framework/discussions)
- [ReleaseNotes](https://github.com/naftiko/framework/wiki/Release-Notes)
- [FAQ](https://github.com/naftiko/framework/wiki/FAQ)

## Features

| Name | Description |
|------|-------------|
| Spec-Driven Capabilities | Declare integration capabilities entirely in YAML, validated by JSON Schema, with no Java required. |
| Multi-Protocol Servers | Expose a single capability simultaneously as MCP, SKILL, and REST endpoints. |
| OpenAPI Interoperability | Import Swagger 2.0, OAS 3.0, and OAS 3.1 specifications and export REST adapters as OpenAPI documents. |
| Format Conversion | Translate between Protobuf, XML, YAML, CSV, Avro, and JSON inside capability pipelines. |
| OpenTelemetry Tracing | Cloud-native observability is built into the framework runtime for every capability invocation. |
| Governed Runtime | Fleet adds discovery, composition, observability, and cost controls on top of framework-executed capabilities. |
| IDE & Catalog Tooling | VS Code extension for editing and linting capability specs, Backstage templates for scaffolding and cataloguing. |

## Use Cases

| Name | Description |
|------|-------------|
| AI Agent Integration | Expose enterprise data and APIs to AI agents through governed MCP capabilities instead of bespoke connectors. |
| Reuse Across Domains | Convert siloed APIs into a measurable inventory of reusable capabilities that span teams and product lines. |
| Policy-Driven Discovery | Apply governance policies at discovery and runtime so AI integrations stay compliant and cost-bounded. |
| SaaS and Microservice Sprawl | Tame the integration surface that grows with every new SaaS subscription and internal microservice. |
| Right-Sized Context for Agents | Deliver only the context an agent needs into each capability invocation, reducing token cost and risk. |

## Integrations

| Name | Description |
|------|-------------|
| Model Context Protocol (MCP) | Capabilities are exposed as MCP servers consumable by Claude, agentic IDEs, and other MCP-aware clients. |
| VS Code | Official VS Code extension for editing, linting, and previewing Naftiko capability specs. |
| Backstage | Templates and plugins for scaffolding and cataloguing Naftiko capabilities inside Backstage. |
| Docker Desktop | Run the framework locally through Docker Desktop integration for development and testing. |
| OpenAPI | Import existing OpenAPI documents as consume adapters and export REST adapters back to OpenAPI. |
| OpenTelemetry | Emit traces, metrics, and logs from every capability invocation to any OTel-compatible backend. |
| Kubernetes | A Kubernetes operator is on the roadmap for running Fleet-governed capabilities as cluster workloads. |

## Solutions

| Name | Description |
|------|-------------|
| Naftiko Framework | Open-source Apache 2.0 engine that interprets capability specs and exposes them as MCP, SKILL, and REST services. |
| Naftiko Fleet — Community Edition | Freeware governed runtime with discovery, composition, and observability for spec-driven capabilities. |
| Naftiko Fleet — Standard and Enterprise | Planned commercial editions of Fleet with expanded governance, policy, and support tiers. |

## Maintainers

**FN:** Kin Lane

**Email:** kin@apievangelist.com
