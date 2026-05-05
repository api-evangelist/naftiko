---
title: "Elevate an Existing API: From Pass-Thru to Capability"
url: "https://naftiko.io/blog/elevate-an-existing-api-from-pass-thru-to-capability/"
date: "2026-04-30T00:00:00+00:00"
author: "Kin Lane"
feed_url: "https://naftiko.io/feed"
---
<p>I keep getting pulled into rooms where the proposed answer to “our API is hard to consume” is to rewrite it. Some working API that has been in production for six or eight or twelve years, owned by some team that half the meeting attendees can’t name, and the fix on the whiteboard is a new service. A new data model. A new set of endpoints. Six months of work. Minimum.</p>

<p>That is almost never what the situation actually calls for. The API is not the problem. The API is uncurated. The third use case in the <a href="https://github.com/naftiko/framework/wiki/Guide-%E2%80%90-Use-Cases#3-elevate-an-existing-api">Naftiko Framework</a> is the one I lean on hardest in those rooms — <strong>elevate an existing API</strong>. Wrap it as a capability. Keep the API. Change the surface.</p>

<p>This is post three in the nine-part walk through the framework use cases.</p>

<h2 id="the-rewrite-reflex">The rewrite reflex</h2>

<p>The rewrite reflex is a very old pattern in the API space. An existing API is inconsistent with some newer convention. It has endpoints nobody remembers shipping. It has a handful of POSTs that really should have been GETs. It returns XML on one path and JSON on another. It has seven auth schemes because seven different teams added one each between 2014 and 2022.</p>

<p>None of that is broken. It is just uncurated. And the industry response, for about as long as I have been writing about this, has been the same: throw it out, build a new one, tell everybody to migrate. That migration then takes the rest of the decade, because the existing consumers are real.</p>

<p>What changes when you have a capability spec is you can stop asking the question “how do we replace this?” and start asking “how do we wrap it?”</p>

<h2 id="the-pass-thru-source-capability">The pass-thru source capability</h2>

<p>The Naftiko capability spec gives you a straightforward shape for this. You declare the existing API as a source in <code class="language-plaintext highlighter-rouge">consumes</code> — every operation you actually want to elevate, with its real method, real path, real parameters. Then you expose a curated set of paths outward, under a clean namespace, through a <code class="language-plaintext highlighter-rouge">rest</code> adapter, an <code class="language-plaintext highlighter-rouge">mcp</code> adapter, or both. The engine runs it. Nothing changes upstream.</p>

<p>Here is the shape of a pass-thru capability for a legacy inventory API:</p>

<div class="language-yaml highlighter-rouge"><div class="highlight"><pre class="highlight"><code><span class="na">naftiko</span><span class="pi">:</span> <span class="s2">"</span><span class="s">1.0.0-alpha1"</span>

<span class="na">info</span><span class="pi">:</span>
  <span class="na">title</span><span class="pi">:</span> <span class="s">Inventory</span>
  <span class="na">description</span><span class="pi">:</span> <span class="s2">"</span><span class="s">Elevated</span><span class="nv"> </span><span class="s">capability</span><span class="nv"> </span><span class="s">over</span><span class="nv"> </span><span class="s">the</span><span class="nv"> </span><span class="s">legacy</span><span class="nv"> </span><span class="s">warehouse</span><span class="nv"> </span><span class="s">inventory</span><span class="nv"> </span><span class="s">API"</span>

<span class="na">capability</span><span class="pi">:</span>
  <span class="na">consumes</span><span class="pi">:</span>
    <span class="pi">-</span> <span class="na">namespace</span><span class="pi">:</span> <span class="s">warehouse-legacy</span>
      <span class="na">type</span><span class="pi">:</span> <span class="s">http</span>
      <span class="na">baseUri</span><span class="pi">:</span> <span class="s2">"</span><span class="s">https://inventory.internal.legacy"</span>
      <span class="na">authentication</span><span class="pi">:</span>
        <span class="na">type</span><span class="pi">:</span> <span class="s">basic</span>
        <span class="na">username</span><span class="pi">:</span> <span class="s2">"</span><span class="s">"</span>
        <span class="na">password</span><span class="pi">:</span> <span class="s2">"</span><span class="s">"</span>
      <span class="na">resources</span><span class="pi">:</span>
        <span class="pi">-</span> <span class="na">name</span><span class="pi">:</span> <span class="s">stock</span>
          <span class="na">path</span><span class="pi">:</span> <span class="s2">"</span><span class="s">/ws/StockQuery"</span>
          <span class="na">operations</span><span class="pi">:</span>
            <span class="pi">-</span> <span class="na">name</span><span class="pi">:</span> <span class="s">query-stock</span>
              <span class="na">method</span><span class="pi">:</span> <span class="s">POST</span>
              <span class="na">inputParameters</span><span class="pi">:</span>
                <span class="pi">-</span> <span class="na">name</span><span class="pi">:</span> <span class="s">sku</span>
                  <span class="na">in</span><span class="pi">:</span> <span class="s">body</span>
                  <span class="na">type</span><span class="pi">:</span> <span class="s">string</span>
                  <span class="na">required</span><span class="pi">:</span> <span class="no">true</span>
            <span class="pi">-</span> <span class="na">name</span><span class="pi">:</span> <span class="s">list-locations</span>
              <span class="na">method</span><span class="pi">:</span> <span class="s">GET</span>
              <span class="na">path</span><span class="pi">:</span> <span class="s2">"</span><span class="s">/ws/Locations"</span>

  <span class="na">exposes</span><span class="pi">:</span>
    <span class="pi">-</span> <span class="na">type</span><span class="pi">:</span> <span class="s">rest</span>
      <span class="na">namespace</span><span class="pi">:</span> <span class="s">inventory</span>
      <span class="na">basePath</span><span class="pi">:</span> <span class="s2">"</span><span class="s">/api/inventory"</span>
      <span class="na">endpoints</span><span class="pi">:</span>
        <span class="pi">-</span> <span class="na">method</span><span class="pi">:</span> <span class="s">GET</span>
          <span class="na">path</span><span class="pi">:</span> <span class="s2">"</span><span class="s">/stock/{sku}"</span>
          <span class="na">description</span><span class="pi">:</span> <span class="s2">"</span><span class="s">Get</span><span class="nv"> </span><span class="s">stock</span><span class="nv"> </span><span class="s">level</span><span class="nv"> </span><span class="s">for</span><span class="nv"> </span><span class="s">a</span><span class="nv"> </span><span class="s">SKU"</span>
          <span class="na">call</span><span class="pi">:</span> <span class="s">warehouse-legacy.query-stock</span>
          <span class="na">inputParameters</span><span class="pi">:</span>
            <span class="pi">-</span> <span class="na">name</span><span class="pi">:</span> <span class="s">sku</span>
              <span class="na">in</span><span class="pi">:</span> <span class="s">path</span>
              <span class="na">type</span><span class="pi">:</span> <span class="s">string</span>
              <span class="na">required</span><span class="pi">:</span> <span class="no">true</span>
              <span class="na">mapping</span><span class="pi">:</span> <span class="s2">"</span><span class="s">body.sku"</span>

        <span class="pi">-</span> <span class="na">method</span><span class="pi">:</span> <span class="s">GET</span>
          <span class="na">path</span><span class="pi">:</span> <span class="s2">"</span><span class="s">/locations"</span>
          <span class="na">description</span><span class="pi">:</span> <span class="s2">"</span><span class="s">List</span><span class="nv"> </span><span class="s">warehouse</span><span class="nv"> </span><span class="s">locations"</span>
          <span class="na">call</span><span class="pi">:</span> <span class="s">warehouse-legacy.list-locations</span>

    <span class="pi">-</span> <span class="na">type</span><span class="pi">:</span> <span class="s">mcp</span>
      <span class="na">namespace</span><span class="pi">:</span> <span class="s">inventory</span>
      <span class="na">tools</span><span class="pi">:</span>
        <span class="pi">-</span> <span class="na">name</span><span class="pi">:</span> <span class="s">get-stock</span>
          <span class="na">description</span><span class="pi">:</span> <span class="s2">"</span><span class="s">Look</span><span class="nv"> </span><span class="s">up</span><span class="nv"> </span><span class="s">the</span><span class="nv"> </span><span class="s">current</span><span class="nv"> </span><span class="s">stock</span><span class="nv"> </span><span class="s">level</span><span class="nv"> </span><span class="s">for</span><span class="nv"> </span><span class="s">a</span><span class="nv"> </span><span class="s">SKU"</span>
          <span class="na">call</span><span class="pi">:</span> <span class="s">warehouse-legacy.query-stock</span>
          <span class="na">inputParameters</span><span class="pi">:</span>
            <span class="pi">-</span> <span class="na">name</span><span class="pi">:</span> <span class="s">sku</span>
              <span class="na">type</span><span class="pi">:</span> <span class="s">string</span>
              <span class="na">required</span><span class="pi">:</span> <span class="no">true</span>
              <span class="na">mapping</span><span class="pi">:</span> <span class="s2">"</span><span class="s">body.sku"</span>
</code></pre></div></div>

<p>Two things worth noticing in that file.</p>

<p>One — the existing API is doing a read-only stock query as a POST, which is the single most common legacy sin I run into. The capability exposes it as a cacheable GET. That is a tiny detail. It changes the HTTP semantics for every downstream consumer, including any CDN, any conditional-request caching layer, and any agent runtime that is quietly batching calls. One line of declarative mapping replaces a months-long argument about whether to “fix it in the source.”</p>

<p>Two — the exposed namespace is <code class="language-plaintext highlighter-rouge">inventory</code>. Not <code class="language-plaintext highlighter-rouge">warehouse-legacy</code>. The consumer never sees the upstream name. The capability is named after the domain it serves, not the system it wraps. That namespace is part of what the <a href="https://github.com/naftiko/framework">capability schema</a> enforces.</p>

<h2 id="schema-based-validation-not-tribal-knowledge">Schema-based validation, not tribal knowledge</h2>

<p>The part of this use case I think gets underweighted is the Spectral piece.</p>

<p>Governance in the API space has, for a long time, meant either “a PDF nobody reads” or “the gatekeeper in the architecture review board.” The capability spec changes the shape of the question. The capability is a file. The schema is a file. The rules — namespace uniqueness, path conventions, security checks — run in CI.</p>

<p>When I say “elevate an existing API,” I do not just mean “put a cleaner face on it.” I mean: give it a machine-checkable contract that lives in Git next to the spec itself. That is what <code class="language-plaintext highlighter-rouge">capability-schema.json</code> and the Spectral rules are for. A namespace collision is a lint error. A path that does not follow convention is a lint error. A missing auth block is a lint error. None of this requires a human gatekeeper.</p>

<p>That matters for the politics of the thing, which I will get to in a second.</p>

<h2 id="three-dimensions-again">Three dimensions, again</h2>

<p>I use the same three-dimensional lens for every API topic — technology, business, politics. This use case touches all three in different ways than AI Integration did.</p>

<p><strong>Technology.</strong> The elevated capability is declarative pass-thru with curated paths. The source API stays as-is. The engine handles HTTP calls, auth, body shaping, and response forwarding. If the upstream returns XML, Avro, CSV, or HTML, format-aware parsing converts it to JSON on the way out. You change the HTTP method where it should have been different all along. You expose the paths you want to expose, and nothing else.</p>

<p><strong>Business.</strong> Nobody has to sell a rewrite. The existing API keeps serving its existing consumers. The new capability layer serves new consumers — including AI agents via MCP — without negotiating migration timelines with every team that depends on the old thing. The capability is a file in Git. It goes through review. It lints. It ships. The cost of extending the API’s reach drops by an order of magnitude.</p>

<p><strong>Politics.</strong> This is where the rewrite reflex really lives. Rewrites are expensive because they are political, not because they are hard. Every rewrite requires someone to tell every consumer “you will migrate.” Capabilities skip that fight. The platform team owns the capability layer. The legacy owners keep owning the upstream. Nobody has to agree on a sunset date. The surface area for disagreement shrinks to the capability file, which is a diff anyone can read.</p>

<p>That last bit is what makes this pattern stick in organizations that have tried the rewrite path and failed.</p>

<h2 id="features-that-matter-here">Features that matter here</h2>

<p>Pulling from the wiki, the features that carry this use case:</p>

<ul>
  <li><strong>Pass-thru source capability</strong> with one or many HTTP adapters</li>
  <li><strong>Declarative source HTTP adapter and target REST adapter</strong></li>
  <li><strong>Focus on source APIs with JSON payloads</strong> — with format-aware parsing for XML, Avro, CSV, TSV, PSV, HTML, Markdown</li>
  <li><strong>Change HTTP methods</strong> to expose proper semantics — read-only POST queries become cacheable GETs</li>
  <li><strong>Schema-based validation with Spectral rules</strong> — namespace uniqueness, path conventions, security checks</li>
  <li><strong>Forward proxy for selective pass-through routing</strong> — trusted header forwarding for the routes you haven’t curated yet</li>
</ul>

<p>The forward proxy detail is the one that lets this pattern work in messy environments. You do not have to curate every path on day one. You curate the ones that matter, you let the rest pass through with header forwarding, and you keep iterating.</p>

<h2 id="where-this-is-showing-up-right-now">Where this is showing up right now</h2>

<p>Most of the partner capabilities I have been writing lately fall into this shape. An existing vendor API. A few paths the consumer actually cares about. A capability file that takes the vendor’s schema and re-exposes it under a cleaner namespace with saner HTTP semantics. The MCP surface and the REST surface come from the same file.</p>

<p>The conversation in the room changes when the artifact is a capability instead of a proposed rewrite. Instead of “here is our estimate for rebuilding the inventory API,” it is “here is the capability file — which paths should we curate first?”</p>

<p>That is a better conversation to be in.</p>

<h2 id="next">Next</h2>

<p>Next post in the series is <strong>Elevate Google Sheets API</strong> — the same pattern applied to something most organizations don’t think of as an API at all. A spreadsheet. Same three-dimensional lens.</p>

<ul>
  <li>Wiki: <a href="https://github.com/naftiko/framework/wiki/Guide-%E2%80%90-Use-Cases">Guide — Use Cases</a></li>
  <li>GitHub: <a href="https://github.com/naftiko/framework">github.com/naftiko/framework</a></li>
  <li>Fleet Community Edition: <a href="https://github.com/naftiko/fleet">github.com/naftiko/fleet</a></li>
</ul>

<p>I will keep walking the list.</p>
