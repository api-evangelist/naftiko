---
title: "Specs Age Like Bananas — Spec-Driven Integration as the Cure"
url: "https://naftiko.io/blog/specs-age-like-bananas-spec-driven-integration-as-the-cure/"
date: "2026-04-30T00:00:00+00:00"
author: "Kin Lane"
feed_url: "https://naftiko.io/feed"
---
<p>Every API team has the same problem.</p>

<p>The spec ships clean on day one. The team launches the API, the OpenAPI doc gets posted to the developer portal, the integration partners read it, build against it, and ship.</p>

<p>By month three, somebody adds a field. By month six, somebody renames an endpoint. By month nine, the doc and the production behavior have drifted far enough that an agent calling the documented endpoints starts hallucinating.</p>

<p>Specs age like bananas.</p>

<p>The reason is not that engineers are lazy. The reason is structural: in most API toolchains, the spec is a side document — a description of what the runtime does — and side documents drift from the thing they describe the moment somebody changes the thing without changing the side document. There is no enforcement mechanism. There is no executable consequence. The runtime never reads the spec.</p>

<p>The cure is not better documentation hygiene. The cure is making the spec the runtime artifact, not a side document.</p>

<p>That is what <strong>Spec-Driven Integration</strong> means. And it is the doctrine the Naftiko Framework is built around.</p>

<h2 id="what-changes-when-the-spec-is-the-runtime">What changes when the spec is the runtime</h2>

<p>In the Naftiko Framework, a capability YAML carries everything: the upstream API it consumes, the operations it exposes (REST, MCP, Agent Skills), the typed input and output parameters, the governance rules, the tags, the identity bindings.</p>

<p>The runtime does not look at a separate spec to figure out what to do. The capability YAML <em>is</em> the spec. It is also the deployable artifact. It is also the thing the IDE renders. It is also the thing the governance engine validates.</p>

<p>When a developer changes a field, they change it in the capability YAML. The spec does not drift, because there is nothing to drift from. The runtime executes the spec.</p>

<p>This is a small change in framing. It is a large change in what your team can stop worrying about.</p>

<h2 id="the-wiki-argument">The wiki argument</h2>

<p>The framework wiki documents the doctrine. The Spec-Driven Integration page is the long-form version of the argument: that observability, governance, discoverability, and agent-readiness are all spec concerns, not deployment concerns. If you cannot describe how a capability should be measured, governed, or discovered in the same artifact that describes what it does, you have already conceded that those concerns will live somewhere else — in a wiki page, in a Confluence doc, in someone’s head.</p>

<p>Each one of those somewhere-elses is a place the spec can drift from. Each drift is a future incident.</p>

<p>The Naftiko Framework Wiki turns the doctrine into a written, citable position your platform team can hand to your security team, your compliance team, and your buyer-side procurement team. All three of those audiences are answering the same question — <em>can we trust what this thing tells us in production</em> — and the wiki gives them a written answer instead of a sales-deck assertion.</p>

<h2 id="three-dimensions-of-the-cure">Three dimensions of the cure</h2>

<p><strong>Technology:</strong> the capability YAML is one source of truth — the runtime executes from it, the IDE renders from it, the governance engine validates against it, the metrics endpoint maps fields back to it. There is nowhere for drift to hide.</p>

<p><strong>Business:</strong> the cost of drift is paid by every downstream consumer. Every integration partner that breaks because a field got renamed without coordination. Every agent run that fails because the documented schema is no longer the actual schema. Every audit that finds a gap because the spec said one thing and the runtime did another. Spec-Driven Integration moves that cost to zero.</p>

<p><strong>Politics:</strong> the spec is no longer a documentation deliverable owned by whichever team got stuck with it. The spec is the work. The team that ships the capability ships the spec, because they are the same artifact. Documentation is not separable from the thing being documented anymore — and that small structural shift is what removes the never-ending “who owns the docs?” argument.</p>

<h2 id="what-to-take-away">What to take away</h2>

<p>If your team is still treating the OpenAPI spec as a deliverable that gets written after the API ships, you are setting up your future self for a drift incident. That is not a discipline problem. It is a tooling problem.</p>

<p>The Naftiko Framework gives you the tooling to solve it. The Naftiko Framework Wiki gives you the doctrine to defend the choice. And the result is an integration layer where specs cannot age, because the spec <em>is</em> the runtime — and the runtime is fresh by definition.</p>

<p>Bananas age. Specs do not have to.</p>
