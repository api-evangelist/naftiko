---
title: "Capability YAML vs x-mcp: Why Vendor Extensions Aren’t the Answer"
url: "https://naftiko.io/blog/capability-yaml-vs-x-mcp-why-vendor-extensions-arent-the-answer/"
date: "2026-05-01T00:00:00+00:00"
author: "Kin Lane"
feed_url: "https://naftiko.io/feed"
---
<p>Two new OpenAPI extensions hit the developer portal market this year.</p>

<p>Zuplo shipped <code class="language-plaintext highlighter-rouge">x-mcp</code>. Redocly shipped <code class="language-plaintext highlighter-rouge">x-mcp</code>. They look almost identical because they are solving the same problem from the same angle: extending OpenAPI to describe MCP server tools so the same documentation pipeline that publishes REST API docs can also publish MCP server docs.</p>

<p>This is good for the vendors. They get to say “MCP support” in their feature list. It is good for customers who already use Zuplo or Redocly — they get a marginal extension to a tool they already pay for.</p>

<p>It is not good for the rest of us.</p>

<p>Last week I had a Slack DM with an engineering lead at a Fortune-50 enterprise. He asked whether there was a spec for documenting MCP servers analogous to OpenAPI. I sent him to the Naftiko Framework. He came back with both <code class="language-plaintext highlighter-rouge">x-mcp</code> extension links and a working description of his portal: a custom Swagger-UI client backed by a legacy database that gets populated by a build process whenever they deploy OpenAPI files.</p>

<p>His problem was not “does an extension exist?” — there are now two. His problem was “how do I get MCP docs into the build pipeline I already have?” Neither vendor extension answered that. Both required adopting either Zuplo or Redocly’s renderer. Neither rendered in his existing in-house Swagger UI client. Neither traveled cleanly through his build → DB pipeline.</p>

<p>Vendor extensions are an upgrade path, not a portability path. They lock the documentation surface to the vendor that ships them.</p>

<h2 id="what-capability-yaml-does-instead">What capability YAML does instead</h2>

<p>A Naftiko capability YAML carries the same information <code class="language-plaintext highlighter-rouge">x-mcp</code> extensions try to carry — and more — without coupling to any portal vendor.</p>

<p>In one self-contained artifact, a capability YAML declares:</p>

<ul>
  <li><strong><code class="language-plaintext highlighter-rouge">consumes:</code></strong> — the upstream API operation, with full HTTP semantics, auth, parameters</li>
  <li><strong><code class="language-plaintext highlighter-rouge">exposes:</code></strong> — REST and MCP and Agent Skills surfaces over the same operation</li>
  <li><strong>typed inputs and outputs</strong> — JSONPath mappings that reshape the upstream payload into task-ready context</li>
  <li><strong>governance metadata</strong> — Spectral rules, policy tags, sensitivity classifications</li>
  <li><strong>identity bindings</strong> — who the agent runs as when it calls the operation</li>
</ul>

<p>The same artifact ships into your Naftiko Engine, into your Backstage catalog, into your build pipeline as an OpenAPI emit, and into your MCP catalog. One source of truth, many rendering targets, no vendor coupling.</p>

<p>If you already render OpenAPI in a custom Swagger UI client, your Naftiko-shaped capability YAML emits valid OpenAPI as a build artifact. It renders unchanged. The MCP half is a sibling artifact your portal can render alongside it without picking a vendor’s Redoc fork.</p>

<h2 id="the-integration-platform-argument">The integration platform argument</h2>

<p>The other thing vendor extensions miss is the integration platform layer.</p>

<p>Zuplo and Redocly are documentation portals. They do not run your capabilities. They do not enforce your governance rules at runtime. They do not capture the metrics from your agents calling your APIs. The <code class="language-plaintext highlighter-rouge">x-mcp</code> extension is documentation only — it tells a reader what an MCP server <em>should</em> look like, not what one <em>does</em>.</p>

<p>Capability YAML is the runtime artifact. The Naftiko Framework reads it, executes the upstream call, exposes the MCP tool, captures the metrics, applies the governance gates, and emits the audit trail. Documentation is one of many things the artifact produces — it is not the only thing, and it is not the primary thing.</p>

<p>That distinction matters more in 2026 than it did in 2024. MCP traffic is moving from prototype to production. The artifacts that describe MCP servers need to do more than render in a portal. They need to run.</p>

<h2 id="three-dimensions">Three dimensions</h2>

<p><strong>Technology:</strong> capability YAML is open, vendor-neutral, and rendering-pipeline-agnostic. It emits valid OpenAPI for any portal that already speaks OpenAPI. It also emits MCP catalogs without an extension fork.</p>

<p><strong>Business:</strong> the cost of MCP adoption is not the protocol — it is the integration cost of getting MCP into pipelines you already have. Capability YAML lowers that cost by speaking the format you already render.</p>

<p><strong>Politics:</strong> vendor extensions are governance off-ramps. Once your MCP docs live in <code class="language-plaintext highlighter-rouge">x-mcp</code>, your portal vendor effectively owns the spec for MCP at your company. Capability YAML keeps the spec in your repo, in your build pipeline, in your governance rules.</p>

<h2 id="what-to-do-about-it">What to do about it</h2>

<p>If you are using Zuplo or Redocly today, <code class="language-plaintext highlighter-rouge">x-mcp</code> works. Use it where it fits.</p>

<p>If you are running a custom developer portal — or planning to publish MCP through any pipeline that is not coupled to one of those two vendors — capability YAML is the path that does not lock you in. It travels through whatever build process you already have. It feeds whatever portal you already render. It runs the capability whether the portal is up or not.</p>

<p>If the registry is the catalog and the capability is the contract, the contract is the thing that travels.</p>
