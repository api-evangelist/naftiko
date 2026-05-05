---
title: "Naftiko Fleet v1.0.0 Alpha 2 — “Catamaran” Is Here"
url: "https://naftiko.io/blog/naftiko-fleet-v1-alpha-2-catamaran-released/"
date: "2026-05-01T00:00:00+00:00"
author: "Kin Lane"
feed_url: "https://naftiko.io/feed"
---
<p>The second alpha of the Naftiko Fleet — code name <strong>“Catamaran”</strong> — is out. The release was announced today by Jerome Louvel in <a href="https://github.com/orgs/naftiko/discussions/7">Naftiko Discussions #7</a>, and it lands as the biggest single update yet for the open-source Framework, the VS Code extension, and the Backstage templates.</p>

<p>Two-hundred-sixty-four commits across the Fleet since First Alpha. 477 changed files in the Framework alone, with +54,952 / −11,011 lines moved. Four contributors. Two scripting language additions. One new transport layer. The shape of Spec-Driven Integration is firming up.</p>

<p>This post is the operator’s-eye view of what shipped — what to install, what changed under the hood, and which limitations to plan around before Alpha 3.</p>

<h2 id="what-is-naftiko-fleet-in-one-paragraph">What is Naftiko Fleet, in one paragraph</h2>

<p>The <a href="https://github.com/naftiko/fleet">Naftiko Fleet</a> is the leading product for <a href="https://github.com/naftiko/framework/wiki/Spec%E2%80%90Driven-Integration">Spec-Driven Integration</a> — a set of tools, documentation, and services to design, create, deploy, and manage capabilities that reinvent API integration for the AI era. Capabilities are declared entirely in YAML; the engine parses them and exposes them via MCP, Agent Skill, or REST out of the box. The Community Edition shipping today bundles the open-source Naftiko Framework, the VS Code extension, and the Backstage templates — everything needed to get from idea to running capability without leaving the open-source stack.</p>

<h2 id="whats-new-in-alpha-2">What’s new in Alpha 2</h2>

<p>The headline list — each item ties to a real shift in how capabilities get built and operated:</p>

<ul>
  <li><strong>OpenAPI interoperability.</strong> Import OpenAPI 3.x and Swagger 2.0 specs directly into Naftiko capabilities. Export back to OAS 3.0. Two-way means existing API estates start contributing capabilities on day one — no rewrite required.</li>
  <li><strong>Inline scripting.</strong> Groovy, JavaScript, and Python steps inside a capability with sandboxed execution and Control Port governance. The data-shaping that used to require a sidecar service can now live in the spec.</li>
  <li><strong>Observability.</strong> Distributed tracing on the W3C Trace Context standard, RED metrics, a Prometheus endpoint, and Grafana dashboards. SRE-grade telemetry is now built into the engine instead of bolted on.</li>
  <li><strong>OAuth 2.1 server auth.</strong> JWT validation with OIDC discovery for exposed REST and MCP endpoints. The “how do I secure my MCP server?” question now has a one-line answer in the YAML.</li>
  <li><strong>DDD aggregates.</strong> Define a domain function once, project it through REST and MCP adapters via <code class="language-plaintext highlighter-rouge">ref</code>. The same business operation gets exposed through every protocol consumers actually use without copy-pasting.</li>
  <li><strong>Unified MCP transport.</strong> The MCP runtime moved from Jetty to Restlet, picked up HTTPS support, and now passes MCP tool hints through as <code class="language-plaintext highlighter-rouge">ToolAnnotations</code>. One transport, both stdio and streamable HTTP.</li>
  <li><strong>VS Code modeline detection.</strong> Capability files no longer have to live as <code class="language-plaintext highlighter-rouge">*.naftiko.yml</code>. A modeline header is enough — fits how teams already organize repos.</li>
  <li><strong>Spectral linting upgrades.</strong> Custom rule functions for aggregate semantics, Control Port presence, script defaults, and unique namespaces. The ruleset now enforces the patterns the spec was designed around.</li>
  <li><strong>Backstage catalog linking.</strong> A custom scaffolder action links consumed APIs to generated components — the catalog gets the wiring, not just the file.</li>
  <li><strong>Linux ARM64 CLI.</strong> Native binary now ships alongside macOS ARM64, Linux AMD64, and Windows AMD64.</li>
</ul>

<h2 id="naftiko-framework--biggest-update-yet">Naftiko Framework — biggest update yet</h2>

<p>The open-source foundation gets the lion’s share of the changes. Full notes are in the <a href="https://github.com/naftiko/framework/releases/tag/v1.0.0-alpha2">Framework v1.0.0-alpha2 release</a>. The areas that matter most for adoption:</p>

<ul>
  <li><strong>OpenAPI import/export</strong> moves the framework from “you write Naftiko YAML by hand” to “you bring your existing OAS and the framework meets you where you are.”</li>
  <li><strong>Inline scripting steps</strong> unlock the data-mediation work that previously fell out of capability scope. Groovy for JVM teams, JavaScript for the broad audience, Python for data-engineering shops — same execution model, same governance.</li>
  <li><strong>OpenTelemetry-based observability with a Control Port</strong> is the quiet but load-bearing change. Capabilities now emit traces, metrics, and logs by default; the Control Port is the governance hook that decides what gets emitted, sampled, and forwarded.</li>
  <li><strong>OAuth 2.1 resource server</strong> closes the loop for production deployments — REST and MCP endpoints can now be put behind a real identity provider with one declarative block.</li>
  <li><strong>DDD-inspired aggregates</strong> move the spec from “one HTTP operation per capability” toward a richer domain model where the same business function can be projected through multiple protocols. This is the path to capabilities that are bigger than a single endpoint.</li>
  <li><strong>Restlet-based MCP transport</strong> with HTTPS and <code class="language-plaintext highlighter-rouge">ToolAnnotations</code> support brings the MCP surface up to parity with what the latest MCP clients expect.</li>
  <li><strong>Enriched JSON Schema and 15+ engine bug fixes</strong> round out the polish.</li>
</ul>

<h2 id="naftiko-extension-for-vs-code">Naftiko Extension for VS Code</h2>

<p>The free <a href="https://github.com/naftiko/fleet/releases/tag/v1.0.0-alpha2">VS Code extension</a> gets the changes that matter most when authoring capabilities at the keyboard:</p>

<ul>
  <li><strong>Modeline-based detection</strong> so the extension activates on any YAML file with the right header, not just files named <code class="language-plaintext highlighter-rouge">*.naftiko.yml</code>.</li>
  <li><strong>Inline Spectral validation</strong> with the Alpha 2 custom rules — the same governance the CI runs.</li>
  <li><strong>Automatic schema and rules sync from Framework CI</strong> — the editor stays in lockstep with the spec without a manual upgrade dance.</li>
  <li><strong>Broader IDE compatibility</strong> — minimum required VS Code version was downgraded to widen the install base.</li>
</ul>

<p>The extension is not on the Marketplace yet. Download the VSIX from the <a href="https://github.com/naftiko/fleet/releases/tag/v1.0.0-alpha2">release assets</a> and install via Extensions → ⋯ → Install from VSIX…</p>

<h2 id="naftiko-templates-for-backstage">Naftiko Templates for Backstage</h2>

<p>The Backstage templates pick up three changes:</p>

<ul>
  <li><strong>Custom scaffolder action</strong> that links the consumed APIs of a capability through Backstage’s Catalog API — components and APIs get wired together at scaffold time.</li>
  <li><strong>Modeline header in scaffolded files</strong> so the VS Code extension lights up on freshly generated capabilities.</li>
  <li><strong>Skeleton aligned with the latest spec</strong> — newly scaffolded capabilities reflect the Alpha 2 schema, not the Alpha 1 one.</li>
</ul>

<p>TechDocs and Kubernetes plugin integration are tracked for a later release.</p>

<h2 id="coming-in-alpha-3--naftiko-operator-for-kubernetes">Coming in Alpha 3 — Naftiko Operator for Kubernetes</h2>

<p>The Kubernetes Operator has been prototyped but is not part of this release. It is planned for <strong>Alpha 3</strong>, and it is what closes the loop on the Fleet’s Kubernetes-native story — capabilities as CRDs, the Operator reconciling them into running engines, and the same Control Port governance applied at cluster scale. If you want to influence the shape of the Operator API, the <a href="https://github.com/naftiko/fleet/wiki/Roadmap">Fleet Roadmap</a> is where that conversation is happening.</p>

<h2 id="by-the-numbers">By the numbers</h2>

<table>
  <thead>
    <tr>
      <th>Metric</th>
      <th>Value</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Commits since Alpha 1</td>
      <td>~264 across Fleet repositories</td>
    </tr>
    <tr>
      <td>Framework changes</td>
      <td>477 files, +54,952 / −11,011 lines</td>
    </tr>
    <tr>
      <td>Contributors</td>
      <td>4</td>
    </tr>
    <tr>
      <td>Scripting languages</td>
      <td>Groovy, JavaScript, Python</td>
    </tr>
    <tr>
      <td>Transports</td>
      <td>MCP Stdio, MCP Streamable HTTP (+ HTTPS), REST</td>
    </tr>
    <tr>
      <td>Data formats</td>
      <td>JSON, XML, Protobuf, CSV, Avro, YAML</td>
    </tr>
  </tbody>
</table>

<h2 id="known-limitations">Known limitations</h2>

<p>Honest list — what is not in your hands yet:</p>

<ul>
  <li><strong>VS Code extension is not on the Marketplace.</strong> VSIX install from release assets only. Marketplace listing is in flight.</li>
  <li><strong>Backstage TechDocs and Kubernetes plugin integration</strong> are not yet in the templates.</li>
  <li><strong>Kubernetes Operator</strong> is prototyped but unreleased — Alpha 3.</li>
  <li><strong>Windows CLI installation</strong> is not yet straightforward; macOS, Linux AMD64, and Linux ARM64 are smoother today.</li>
</ul>

<p>If any of these are in your critical path, raise it in <a href="https://github.com/orgs/naftiko/discussions">Discussions</a> — the prioritization for Alpha 3 is being shaped now.</p>

<h2 id="links">Links</h2>

<ul>
  <li><a href="https://github.com/naftiko/fleet/releases/tag/v1.0.0-alpha2">Fleet v1.0.0-alpha2 release notes</a> — the full Fleet changelog</li>
  <li><a href="https://github.com/naftiko/framework/releases/tag/v1.0.0-alpha2">Framework v1.0.0-alpha2 release notes</a> — the full Framework changelog</li>
  <li><a href="https://github.com/naftiko/fleet/wiki">Fleet Wiki</a> — overview, install paths, fleet-level architecture</li>
  <li><a href="https://github.com/naftiko/framework/wiki">Framework Wiki</a> — specification, tutorials, use cases</li>
  <li><a href="https://github.com/naftiko/fleet/wiki/Roadmap">Fleet Roadmap</a> — what is coming next</li>
  <li><a href="https://github.com/orgs/naftiko/discussions">Naftiko Discussions</a> — where to ask, share, and contribute</li>
</ul>

<p>This is still a pre-release. Breaking changes will continue between alphas as the spec converges toward 1.0 GA. That is the deal — early visibility for early feedback. If you are running Naftiko in any non-trivial environment, <a href="https://github.com/naftiko/framework/blob/main/CONTRIBUTING.md">CONTRIBUTING.md</a> is the door in.</p>

<p>Catamaran is in your hands. Tell us what runs, what breaks, and what you want next.</p>
