# Naftiko

<!-- API-EVANGELIST-PROVENANCE:BEGIN -->
> ### About this repository
>
> **This is not our API.** This repository is an independent, third-party profile of a company's
> **publicly available** API surface, maintained by [API Evangelist](https://apievangelist.com).
> API Evangelist does not operate, host, resell, or support this company's APIs, and is not
> affiliated with or endorsed by the company unless stated on the profile.
>
> **Where the information came from.** Everything here is assembled from material a member of the
> public can reach with a browser and no credentials — the company's own website, developer portal
> and documentation, the specifications it publishes for public use (OpenAPI, AsyncAPI, JSON Schema,
> `apis.json`, `llms.txt` and similar), its public repositories, and its public status, pricing and
> changelog pages. **Nothing here is obtained by breaching a system, defeating an access control, or
> using credentials of any kind.**
>
> **The rating is an independent assessment.** The Kin Score and Agent Readiness rating are
> independently calculated scores of a company's *public* API artifacts, produced by API Evangelist
> against a published rubric. They are not certifications, endorsements, security assessments, or
> audits, and they score published artifacts — not the quality, safety, or security of the software.
>
> **Corrections, re-scores, and removal are free.** No partnership, contract, or purchase is
> required, and you do not need to justify the request.
>
> - **Something wrong?** Open an issue on this repository, or email
>   [info@apievangelist.com](mailto:info@apievangelist.com).
> - **Published something new?** Ask for a re-score and we will re-run the rating.
> - **Want the listing taken down?** Say so and we will honor it. The profile is reduced to your
>   company name, a factual description, and a link to your own site, and the company is recorded as
>   **unrated** — never scored zero for having asked.
>
> **Response times.** Acknowledgement within **one business day**; removal or restriction within
> **two business days**; corrections and re-scores within **five business days**.
>
> **Not from the company, and here with a question?** You are welcome here — we would rather be the
> front line and point you the right way than have a good report go nowhere. What this repository
> can answer is narrow, though, so it is worth knowing who you are actually looking for:
>
> - **A question about how the API works, an account, billing, or a bug in the service** — that is
>   the company's own support, not us. We profile this API; we do not operate it and cannot see
>   your account.
> - **A bug in an open-source project we only catalog** — file it on that project's own repository.
>   This has happened with a real and correct bug report that reached us instead of the people who
>   could fix it, which helped nobody.
> - **Anything about this listing itself** — the description, the tags, the rating, a missing or
>   wrong artifact — is ours. Open an issue here.
> - **Not sure, or something general about API Evangelist or APIs.io** — open an issue on the
>   [APIs.io Inbox](https://github.com/api-search/inbox) and we will route it.
>
> **This repository contains no software, and we will never ask you to download anything.** There is
> no build, release, installer, or binary here — only text and machine-readable API descriptions, so
> there is nothing here that can be "corrupt" or need "repairing". Any issue, comment, or email
> claiming otherwise and offering a download link is not from us and is hostile. Do not follow the
> link; it is a lure. Report it to GitHub and, if you like, tell us at
> [info@apievangelist.com](mailto:info@apievangelist.com) so we can take it down.
>
> **On a security or compliance team?** Email
> [info@apievangelist.com](mailto:info@apievangelist.com) with *security* in the subject line and
> you will get a person, not a form. We will tell you exactly which public URLs this profile was
> built from so your team can see the same surface we did, and we will take the listing down on
> request while you work through it.
>
> Full detail: **[Where this data comes from](https://apievangelist.com/about/where-our-data-comes-from)**
<!-- API-EVANGELIST-PROVENANCE:END -->

Naftiko builds spec-driven integration software for AI agents. Its two Apache 2.0 Java engines — Ikanos, a capability engine, and Polychro, a polyglot spec linter — let a team declare a slice of its business in a single YAML capability file and serve it simultaneously over MCP, Agent Skill, REST and a Control port without writing or compiling code. A capability consumes the HTTP APIs an organisation already has, structures their responses into clean JSON, then aggregates and orchestrates those operations into one governed contract for agents, applications and partners. Naftiko operates no hosted API of its own: it distributes software customers run on their own infrastructure, so this profile catalogs a published JSON Schema, a governance ruleset, a CLI, container images and Maven artifacts rather than endpoints and API keys. Four editions are published — Community (free forever, Apache 2.0), Developer and Team (managed hosting, in development) and Enterprise (Warden and Skipper, design partners) — none of them carrying a published price.

**Areas:** Company · Ai Data · API Integration · API Governance · MCP · Agent Skills · Open Source · Developer Tools · API Specification · Spec-Driven Integration · Capability Engine · API Linting

## Naftiko on APIs.io

Everything profiled for Naftiko is published on APIs.io rather than in this repository. Start at the provider page — **[apis.io/providers/naftiko/](https://apis.io/providers/naftiko/)** — or go straight to one of the APIs or properties below.

### Published properties

Each property below is a page on APIs.io carrying the machine-readable artifact and what it says.

#### MCP servers (1)

Model Context Protocol servers that expose the API to agents.

- [MCP posture — Naftiko generates MCP servers, operates none](https://apis.io/servers/naftiko/mcp-posture-naftiko-generates-mcp-servers-operates-none/)

#### JSON Schema (1)

The data models the API reads and writes.

- [Naftiko Ikanos Capability](https://apis.io/schemas/naftiko/naftiko-ikanos-capability/)

#### Governance rules (2)

Spectral rulesets for the provider's contracts.

- [Naftiko — ikanos rules](https://apis.io/rules/naftiko/naftiko-ikanos-ruleset/)
- [Naftiko — polychro rules](https://apis.io/rules/naftiko/naftiko-polychro-spectral-rules/)

#### Security (1)

Authentication, trust centre, and vulnerability disclosure.

- [Naftiko Domain Security](https://apis.io/security/naftiko/naftiko-domain-security/)

#### Plans and pricing (1)

Published plans, tiers and prices.

- [Naftiko Plans Pricing](https://apis.io/plans/naftiko/naftiko-plans-pricing/)

#### Rate limits (1)

Published request limits.

- [Naftiko Rate Limits](https://apis.io/rate-limits/naftiko/naftiko-rate-limits/)

## Where to find this API

Links to the company's own pages. This repository publishes no copy of their specifications — the canonical document is theirs and lives at the URLs below.

- [Website](https://naftiko.io/)
- [Documentation](https://shipyard.naftiko.io/ikanos/1.0.0-beta3/)
- [Getting started](https://shipyard.naftiko.io/ikanos/1.0.0-beta3/installation/)
- [API reference](https://shipyard.naftiko.io/ikanos/1.0.0-beta3/spec/)
- [Pricing](https://naftiko.io/platform/editions/)
- [Support](https://naftiko.io/company/contact-us/)
- [Changelog](https://github.com/naftiko/ikanos/releases)
- [Blog](https://naftiko.io/resources/blog)
- [Terms of service](https://naftiko.io/legal/terms/)
- [Privacy policy](https://naftiko.io/legal/privacy/)
- [GitHub](https://github.com/naftiko)

## API Evangelist rating

**Kin Score 35.9** — thin · rubric 0.24.0 · scored 2026-10-17

| Facet | Score |
|---|---|
| Discoverability | 48.3 |
| Contract quality | 0.0 |
| Contract governance | 31.8 |
| Operational transparency | 31.6 |
| Developer ergonomics | 50.0 |
| Access clarity | 63.2 |

**Agent readiness 9.6** — agent-aware

| Agent-readiness dimension | Result |
|---|---|
| Spec Presence | — |
| Agentic Access | — |
| Reversibility Documented | — |
| MCP Server | documented (partial credit) |
| Auth Clarity | — |
| Idempotency | — |
| Error Semantics | — |
| OpenAPI Examples | — |
| Rate Limit Signal | documented (partial credit) |
| Event Surface Described | — |
| Agent Skills | yes |
| Well Known Catalog | — |
| Consent Identity | — |
| Agent Card | — |
| Dry Run Mode | — |
| Delegated Identity | — |
| Protected Resource Metadata | — |
| Dynamic Client Registration | — |
| Agentic Commerce | — |

**Regulatory alignment 18.6** — Horizontal (data, software, accessibility, platform)

The rubric is published at [github.com/api-evangelist/kin-score](https://github.com/api-evangelist/kin-score). The per-check breakdown behind these numbers — which checks were earned, partial or missed — along with score history and how this provider compares to its peers, is available at [apis.io/providers/naftiko/](https://apis.io/providers/naftiko/).

## How this profile is made

API Evangelist profiles a company's public API surface the way a developer would find it: the website, developer portal and documentation, the specifications it publishes, its public repositories, and its pricing, status and changelog pages. From that it derives the properties above — contracts split per API, workflows, schemas, collections, governance rules, plans and rate limits — and scores them against the published [Kin Score rubric](https://github.com/api-evangelist/kin-score).

The artifacts are published on [APIs.io](https://apis.io), not in this repository. This repository is the public record for Naftiko: the place to say something is wrong, missing or out of date, and to ask for a re-score or a removal.

## Open an issue

- **Something about this listing** — a wrong description, a missing API, a stale artifact or a question about the rating: [open an issue on this repository](https://github.com/api-evangelist/naftiko/issues/new).
- **A new API, or one that is not in the catalog yet** — [submit it on APIs.io](https://apis.io/add/).
- **Anything else** — [the APIs.io inbox](https://github.com/api-search/inbox/issues/new) or [info@apievangelist.com](mailto:info@apievangelist.com).
