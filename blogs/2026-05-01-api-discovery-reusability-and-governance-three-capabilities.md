---
title: "API Discovery, Reusability, and Governance — Three Capabilities, One MCP Surface for Claude and Copilot"
url: "https://naftiko.io/blog/api-discovery-reusability-governance-three-capabilities-one-mcp-surface/"
date: "2026-05-01T00:00:00+00:00"
author: "Kin Lane"
feed_url: "https://naftiko.io/feed"
---
<p>Every enterprise platform team has a version of the same three problems.</p>

<p>Nobody can find the APIs. They are scattered across a GitHub estate that grew faster than any catalog, an AWS API Gateway account someone configured two reorgs ago, and a developer portal that has not been updated in eighteen months. Asking <em>“what APIs do we have?”</em> produces a different answer depending on who you ask.</p>

<p>Nobody can prove which APIs are reused. Reusability is the word every API platform pitch deck used in the last ten years, and almost none of those programs ever produced a real number for it. The data exists — every gateway and APM stack records it — but the report nobody asked for in 2018 is the report nobody runs in 2026.</p>

<p>Nobody can run governance against a runnable artifact. Reviews happen against slide decks or wiki pages that drift the moment they are saved. The reviewer signs off on the description; the runtime does whatever the code happens to do.</p>

<p>Meanwhile, leadership has been asking for an AI story for six quarters. The platform team usually answers with a Slack bot or an internal copilot demo. Neither closes the loop with the existing API platform — and so the AI program and the API program continue to be two separate budgets, each underfunded, each unable to make the other’s case.</p>

<p>The fix is to stop treating them as separate things. The same capability layer that answers the API discovery, reusability, and governance questions is the layer that grounds the AI surface. Three capabilities, one runtime, one MCP namespace developers reach from inside the IDE.</p>

<h2 id="capability-one--discovery-across-every-source-where-a-spec-might-live">Capability one — Discovery across every source where a spec might live</h2>

<p>The first capability crawls every place an OpenAPI spec might live: every GitHub org and repo in the estate, every API gateway account, every internal portal that publishes machine-readable artifacts. It normalizes what it finds — a YAML in a repo, an export from a gateway, a JSON dropped in a CMS — and exposes the unified result as a single deterministic <code class="language-plaintext highlighter-rouge">find-specs</code> operation.</p>

<p>That operation answers the <em>“what APIs do we have?”</em> question the same way every time. Same input, same output. Same audit trail of where each spec came from, when it was last touched, and whether the gateway has it registered.</p>

<p>It is not a portal you have to maintain. It is a runtime capability that re-derives the answer on every call. The catalog is no longer a thing the platform team curates by hand and watches drift. It is a query.</p>

<div class="language-yaml highlighter-rouge"><div class="highlight"><pre class="highlight"><code><span class="na">naftiko</span><span class="pi">:</span> <span class="s2">"</span><span class="s">1.0.0-alpha2"</span>

<span class="na">info</span><span class="pi">:</span>
  <span class="na">title</span><span class="pi">:</span> <span class="s">API Spec Aggregation</span>
  <span class="na">description</span><span class="pi">:</span> <span class="pi">&gt;-</span>
    <span class="s">Crawls every place an OpenAPI spec might live — GitHub Search across the</span>
    <span class="s">enterprise GitHub estate, plus AWS API Gateway — and exposes the unified</span>
    <span class="s">result as REST + MCP + Agent Skills so any developer or agent can discover</span>
    <span class="s">specs across the whole estate from one tool call.</span>
  <span class="na">tags</span><span class="pi">:</span>
    <span class="pi">-</span> <span class="s">API Discovery</span>
    <span class="pi">-</span> <span class="s">OpenAPI</span>
    <span class="pi">-</span> <span class="s">GitHub</span>
    <span class="pi">-</span> <span class="s">AWS API Gateway</span>

<span class="na">binds</span><span class="pi">:</span>
  <span class="pi">-</span> <span class="na">namespace</span><span class="pi">:</span> <span class="s">github-env</span>
    <span class="na">keys</span><span class="pi">:</span> <span class="pi">{</span> <span class="nv">GITHUB_TOKEN</span><span class="pi">:</span> <span class="nv">GITHUB_TOKEN</span> <span class="pi">}</span>
  <span class="pi">-</span> <span class="na">namespace</span><span class="pi">:</span> <span class="s">aws-env</span>
    <span class="na">keys</span><span class="pi">:</span>
      <span class="na">AWS_ACCESS_KEY_ID</span><span class="pi">:</span> <span class="s">AWS_ACCESS_KEY_ID</span>
      <span class="na">AWS_SECRET_ACCESS_KEY</span><span class="pi">:</span> <span class="s">AWS_SECRET_ACCESS_KEY</span>
      <span class="na">AWS_REGION</span><span class="pi">:</span> <span class="s">AWS_REGION</span>

<span class="na">capability</span><span class="pi">:</span>
  <span class="na">consumes</span><span class="pi">:</span>
    <span class="pi">-</span> <span class="na">namespace</span><span class="pi">:</span> <span class="s">github</span>
      <span class="na">type</span><span class="pi">:</span> <span class="s">http</span>
      <span class="na">baseUri</span><span class="pi">:</span> <span class="s2">"</span><span class="s">https://api.github.com"</span>
      <span class="na">authentication</span><span class="pi">:</span> <span class="pi">{</span> <span class="nv">type</span><span class="pi">:</span> <span class="nv">bearer</span><span class="pi">,</span> <span class="nv">token</span><span class="pi">:</span> <span class="s2">"</span><span class="s">"</span> <span class="pi">}</span>
      <span class="na">resources</span><span class="pi">:</span>
        <span class="pi">-</span> <span class="na">name</span><span class="pi">:</span> <span class="s">search-code</span>
          <span class="na">path</span><span class="pi">:</span> <span class="s2">"</span><span class="s">/search/code"</span>
          <span class="na">operations</span><span class="pi">:</span>
            <span class="pi">-</span> <span class="na">name</span><span class="pi">:</span> <span class="s">search-openapi-specs</span>
              <span class="na">method</span><span class="pi">:</span> <span class="s">GET</span>
              <span class="na">inputParameters</span><span class="pi">:</span>
                <span class="pi">-</span> <span class="pi">{</span> <span class="nv">name</span><span class="pi">:</span> <span class="nv">q</span><span class="pi">,</span> <span class="nv">in</span><span class="pi">:</span> <span class="nv">query</span> <span class="pi">}</span>

    <span class="pi">-</span> <span class="na">namespace</span><span class="pi">:</span> <span class="s">apigateway</span>
      <span class="na">type</span><span class="pi">:</span> <span class="s">http</span>
      <span class="na">baseUri</span><span class="pi">:</span> <span class="s2">"</span><span class="s">https://apigateway..amazonaws.com"</span>
      <span class="na">authentication</span><span class="pi">:</span>
        <span class="na">type</span><span class="pi">:</span> <span class="s">aws-sig-v4</span>
        <span class="na">accessKeyId</span><span class="pi">:</span> <span class="s2">"</span><span class="s">"</span>
        <span class="na">secretKey</span><span class="pi">:</span> <span class="s2">"</span><span class="s">"</span>
        <span class="na">region</span><span class="pi">:</span> <span class="s2">"</span><span class="s">"</span>
        <span class="na">service</span><span class="pi">:</span> <span class="s">apigateway</span>
      <span class="na">resources</span><span class="pi">:</span>
        <span class="pi">-</span> <span class="na">name</span><span class="pi">:</span> <span class="s">rest-apis</span>
          <span class="na">path</span><span class="pi">:</span> <span class="s2">"</span><span class="s">/restapis"</span>
          <span class="na">operations</span><span class="pi">:</span>
            <span class="pi">-</span> <span class="pi">{</span> <span class="nv">name</span><span class="pi">:</span> <span class="nv">list-rest-apis</span><span class="pi">,</span> <span class="nv">method</span><span class="pi">:</span> <span class="nv">GET</span> <span class="pi">}</span>
        <span class="pi">-</span> <span class="na">name</span><span class="pi">:</span> <span class="s">rest-api-export</span>
          <span class="na">path</span><span class="pi">:</span> <span class="s2">"</span><span class="s">/restapis//stages//exports/oas30"</span>
          <span class="na">operations</span><span class="pi">:</span>
            <span class="pi">-</span> <span class="na">name</span><span class="pi">:</span> <span class="s">export-rest-api</span>
              <span class="na">method</span><span class="pi">:</span> <span class="s">GET</span>
              <span class="na">inputParameters</span><span class="pi">:</span>
                <span class="pi">-</span> <span class="pi">{</span> <span class="nv">name</span><span class="pi">:</span> <span class="nv">rest_api_id</span><span class="pi">,</span> <span class="nv">in</span><span class="pi">:</span> <span class="nv">path</span> <span class="pi">}</span>
                <span class="pi">-</span> <span class="pi">{</span> <span class="nv">name</span><span class="pi">:</span> <span class="nv">stage_name</span><span class="pi">,</span>  <span class="nv">in</span><span class="pi">:</span> <span class="nv">path</span> <span class="pi">}</span>

  <span class="na">exposes</span><span class="pi">:</span>
    <span class="pi">-</span> <span class="na">type</span><span class="pi">:</span> <span class="s">rest</span>
      <span class="na">port</span><span class="pi">:</span> <span class="m">8080</span>
      <span class="na">namespace</span><span class="pi">:</span> <span class="s">spec-aggregation-rest</span>
      <span class="na">resources</span><span class="pi">:</span>
        <span class="pi">-</span> <span class="na">name</span><span class="pi">:</span> <span class="s">specs</span>
          <span class="na">path</span><span class="pi">:</span> <span class="s2">"</span><span class="s">/find-specs"</span>
          <span class="na">operations</span><span class="pi">:</span>
            <span class="pi">-</span> <span class="na">method</span><span class="pi">:</span> <span class="s">GET</span>
              <span class="na">name</span><span class="pi">:</span> <span class="s">find-specs</span>
              <span class="na">inputParameters</span><span class="pi">:</span>
                <span class="pi">-</span> <span class="pi">{</span> <span class="nv">name</span><span class="pi">:</span> <span class="nv">org</span><span class="pi">,</span>    <span class="nv">in</span><span class="pi">:</span> <span class="nv">query</span><span class="pi">,</span> <span class="nv">type</span><span class="pi">:</span> <span class="nv">string</span> <span class="pi">}</span>
                <span class="pi">-</span> <span class="pi">{</span> <span class="nv">name</span><span class="pi">:</span> <span class="nv">source</span><span class="pi">,</span> <span class="nv">in</span><span class="pi">:</span> <span class="nv">query</span><span class="pi">,</span> <span class="nv">type</span><span class="pi">:</span> <span class="nv">string</span> <span class="pi">}</span>
                <span class="pi">-</span> <span class="pi">{</span> <span class="nv">name</span><span class="pi">:</span> <span class="nv">query</span><span class="pi">,</span>  <span class="nv">in</span><span class="pi">:</span> <span class="nv">query</span><span class="pi">,</span> <span class="nv">type</span><span class="pi">:</span> <span class="nv">string</span> <span class="pi">}</span>

    <span class="pi">-</span> <span class="na">type</span><span class="pi">:</span> <span class="s">mcp</span>
      <span class="na">port</span><span class="pi">:</span> <span class="m">3010</span>
      <span class="na">namespace</span><span class="pi">:</span> <span class="s">spec-aggregation-mcp</span>
      <span class="na">tools</span><span class="pi">:</span>
        <span class="pi">-</span> <span class="na">name</span><span class="pi">:</span> <span class="s">find-specs</span>
          <span class="na">description</span><span class="pi">:</span> <span class="s2">"</span><span class="s">Discover</span><span class="nv"> </span><span class="s">OpenAPI</span><span class="nv"> </span><span class="s">specs</span><span class="nv"> </span><span class="s">across</span><span class="nv"> </span><span class="s">GitHub</span><span class="nv"> </span><span class="s">orgs</span><span class="nv"> </span><span class="s">and</span><span class="nv"> </span><span class="s">AWS</span><span class="nv"> </span><span class="s">API</span><span class="nv"> </span><span class="s">Gateway.</span><span class="nv"> </span><span class="s">Returns</span><span class="nv"> </span><span class="s">spec_url,</span><span class="nv"> </span><span class="s">source,</span><span class="nv"> </span><span class="s">repo,</span><span class="nv"> </span><span class="s">last_updated,</span><span class="nv"> </span><span class="s">and</span><span class="nv"> </span><span class="s">gateway_registered</span><span class="nv"> </span><span class="s">for</span><span class="nv"> </span><span class="s">each</span><span class="nv"> </span><span class="s">match."</span>
          <span class="na">hints</span><span class="pi">:</span> <span class="pi">{</span> <span class="nv">readOnly</span><span class="pi">:</span> <span class="nv">true</span> <span class="pi">}</span>

    <span class="pi">-</span> <span class="na">type</span><span class="pi">:</span> <span class="s">skill</span>
      <span class="na">port</span><span class="pi">:</span> <span class="m">3011</span>
      <span class="na">namespace</span><span class="pi">:</span> <span class="s">spec-aggregation-skills</span>
      <span class="na">skills</span><span class="pi">:</span>
        <span class="pi">-</span> <span class="na">name</span><span class="pi">:</span> <span class="s">spec-discovery</span>
          <span class="na">description</span><span class="pi">:</span> <span class="s2">"</span><span class="s">Discover</span><span class="nv"> </span><span class="s">OpenAPI</span><span class="nv"> </span><span class="s">specs</span><span class="nv"> </span><span class="s">across</span><span class="nv"> </span><span class="s">the</span><span class="nv"> </span><span class="s">whole</span><span class="nv"> </span><span class="s">estate."</span>
          <span class="na">allowed-tools</span><span class="pi">:</span> <span class="s2">"</span><span class="s">find-specs"</span>
          <span class="na">tools</span><span class="pi">:</span>
            <span class="pi">-</span> <span class="na">name</span><span class="pi">:</span> <span class="s">find-specs</span>
              <span class="na">from</span><span class="pi">:</span> <span class="pi">{</span> <span class="nv">sourceNamespace</span><span class="pi">:</span> <span class="nv">spec-aggregation-mcp</span><span class="pi">,</span> <span class="nv">action</span><span class="pi">:</span> <span class="nv">find-specs</span> <span class="pi">}</span>
</code></pre></div></div>

<h2 id="capability-two--reusability-scored-from-telemetry-not-asserted-from-intent">Capability two — Reusability scored from telemetry, not asserted from intent</h2>

<p>The second capability does the thing every platform team has been <em>meaning</em> to do for ten years: it consumes API-call telemetry from Splunk, New Relic, and Datadog, and produces a deterministic reusability summary.</p>

<p>Which endpoints have more than one consumer. Which consumers are calling shadow instances of an API another team already runs. Which endpoints have zero traffic and are candidates for retirement. Which teams are duplicating work because nobody connected them to the team that already shipped it.</p>

<p>The signal exists in the observability stack. The capability is the deterministic layer that reads it, shapes it, and reports the resulting summary back to the same observability stack — so the reusability number lands in the dashboard the platform team already watches every morning, instead of in a quarterly slide deck nobody trusts.</p>

<p>The output is the artifact governance has been asking for. Not <em>“are we reusing APIs?”</em> but <em>“here are the eleven endpoints that account for sixty percent of redundant traffic, ranked by team.”</em></p>

<div class="language-yaml highlighter-rouge"><div class="highlight"><pre class="highlight"><code><span class="na">naftiko</span><span class="pi">:</span> <span class="s2">"</span><span class="s">1.0.0-alpha2"</span>

<span class="na">info</span><span class="pi">:</span>
  <span class="na">title</span><span class="pi">:</span> <span class="s">API Reusability Summary</span>
  <span class="na">description</span><span class="pi">:</span> <span class="pi">&gt;-</span>
    <span class="s">Consumes API-call telemetry from Splunk, New Relic, and Datadog and</span>
    <span class="s">produces a deterministic API-reusability summary — endpoints with multiple</span>
    <span class="s">consumers, shadow-instance duplication, zero-traffic candidates for</span>
    <span class="s">retirement — and reports the summary back to the same observability stack.</span>
  <span class="na">tags</span><span class="pi">:</span>
    <span class="pi">-</span> <span class="s">API Reusability</span>
    <span class="pi">-</span> <span class="s">Splunk</span>
    <span class="pi">-</span> <span class="s">New Relic</span>
    <span class="pi">-</span> <span class="s">Datadog</span>

<span class="na">binds</span><span class="pi">:</span>
  <span class="pi">-</span> <span class="na">namespace</span><span class="pi">:</span> <span class="s">splunk-env</span>
    <span class="na">keys</span><span class="pi">:</span> <span class="pi">{</span> <span class="nv">SPLUNK_TOKEN</span><span class="pi">:</span> <span class="nv">SPLUNK_TOKEN</span><span class="pi">,</span> <span class="nv">SPLUNK_HOST</span><span class="pi">:</span> <span class="nv">SPLUNK_HOST</span> <span class="pi">}</span>
  <span class="pi">-</span> <span class="na">namespace</span><span class="pi">:</span> <span class="s">newrelic-env</span>
    <span class="na">keys</span><span class="pi">:</span> <span class="pi">{</span> <span class="nv">NEWRELIC_API_KEY</span><span class="pi">:</span> <span class="nv">NEWRELIC_API_KEY</span><span class="pi">,</span> <span class="nv">NEWRELIC_ACCOUNT_ID</span><span class="pi">:</span> <span class="nv">NEWRELIC_ACCOUNT_ID</span> <span class="pi">}</span>
  <span class="pi">-</span> <span class="na">namespace</span><span class="pi">:</span> <span class="s">datadog-env</span>
    <span class="na">keys</span><span class="pi">:</span> <span class="pi">{</span> <span class="nv">DD_API_KEY</span><span class="pi">:</span> <span class="nv">DD_API_KEY</span><span class="pi">,</span> <span class="nv">DD_APP_KEY</span><span class="pi">:</span> <span class="nv">DD_APP_KEY</span> <span class="pi">}</span>

<span class="na">capability</span><span class="pi">:</span>
  <span class="na">consumes</span><span class="pi">:</span>
    <span class="pi">-</span> <span class="na">namespace</span><span class="pi">:</span> <span class="s">splunk</span>
      <span class="na">type</span><span class="pi">:</span> <span class="s">http</span>
      <span class="na">baseUri</span><span class="pi">:</span> <span class="s2">"</span><span class="s">https:///services"</span>
      <span class="na">authentication</span><span class="pi">:</span> <span class="pi">{</span> <span class="nv">type</span><span class="pi">:</span> <span class="nv">bearer</span><span class="pi">,</span> <span class="nv">token</span><span class="pi">:</span> <span class="s2">"</span><span class="s">"</span> <span class="pi">}</span>
      <span class="na">resources</span><span class="pi">:</span>
        <span class="pi">-</span> <span class="na">name</span><span class="pi">:</span> <span class="s">search</span>
          <span class="na">path</span><span class="pi">:</span> <span class="s2">"</span><span class="s">/search/jobs/export"</span>
          <span class="na">operations</span><span class="pi">:</span>
            <span class="pi">-</span> <span class="na">name</span><span class="pi">:</span> <span class="s">search-api-calls</span>
              <span class="na">method</span><span class="pi">:</span> <span class="s">POST</span>
              <span class="na">body</span><span class="pi">:</span> <span class="pi">|</span>
                <span class="s">search=index=apigw earliest=-7d | stats count by api_name, consumer_team, endpoint</span>

    <span class="pi">-</span> <span class="na">namespace</span><span class="pi">:</span> <span class="s">newrelic</span>
      <span class="na">type</span><span class="pi">:</span> <span class="s">http</span>
      <span class="na">baseUri</span><span class="pi">:</span> <span class="s2">"</span><span class="s">https://api.newrelic.com/graphql"</span>
      <span class="na">authentication</span><span class="pi">:</span> <span class="pi">{</span> <span class="nv">type</span><span class="pi">:</span> <span class="nv">api-key</span><span class="pi">,</span> <span class="nv">header</span><span class="pi">:</span> <span class="s2">"</span><span class="s">API-Key"</span><span class="pi">,</span> <span class="nv">value</span><span class="pi">:</span> <span class="s2">"</span><span class="s">"</span> <span class="pi">}</span>
      <span class="na">resources</span><span class="pi">:</span>
        <span class="pi">-</span> <span class="na">name</span><span class="pi">:</span> <span class="s">nrql</span>
          <span class="na">path</span><span class="pi">:</span> <span class="s2">"</span><span class="s">"</span>
          <span class="na">operations</span><span class="pi">:</span>
            <span class="pi">-</span> <span class="pi">{</span> <span class="nv">name</span><span class="pi">:</span> <span class="nv">query-traffic</span><span class="pi">,</span> <span class="nv">method</span><span class="pi">:</span> <span class="nv">POST</span> <span class="pi">}</span>

    <span class="pi">-</span> <span class="na">namespace</span><span class="pi">:</span> <span class="s">datadog</span>
      <span class="na">type</span><span class="pi">:</span> <span class="s">http</span>
      <span class="na">baseUri</span><span class="pi">:</span> <span class="s2">"</span><span class="s">https://api.datadoghq.com/api/v2"</span>
      <span class="na">authentication</span><span class="pi">:</span>
        <span class="na">type</span><span class="pi">:</span> <span class="s">headers</span>
        <span class="na">headers</span><span class="pi">:</span> <span class="pi">{</span> <span class="s2">"</span><span class="s">DD-API-KEY"</span><span class="pi">:</span> <span class="s2">"</span><span class="s">"</span><span class="pi">,</span> <span class="s2">"</span><span class="s">DD-APPLICATION-KEY"</span><span class="pi">:</span> <span class="s2">"</span><span class="s">"</span> <span class="pi">}</span>
      <span class="na">resources</span><span class="pi">:</span>
        <span class="pi">-</span> <span class="na">name</span><span class="pi">:</span> <span class="s">logs</span>
          <span class="na">path</span><span class="pi">:</span> <span class="s2">"</span><span class="s">/logs/events/search"</span>
          <span class="na">operations</span><span class="pi">:</span>
            <span class="pi">-</span> <span class="pi">{</span> <span class="nv">name</span><span class="pi">:</span> <span class="nv">search-logs</span><span class="pi">,</span> <span class="nv">method</span><span class="pi">:</span> <span class="nv">POST</span> <span class="pi">}</span>

        <span class="c1"># Reporting back: post the reusability summary as a custom metric</span>
        <span class="c1"># so the same dashboard the platform team already watches picks it up.</span>
        <span class="pi">-</span> <span class="na">name</span><span class="pi">:</span> <span class="s">metrics</span>
          <span class="na">path</span><span class="pi">:</span> <span class="s2">"</span><span class="s">/series"</span>
          <span class="na">operations</span><span class="pi">:</span>
            <span class="pi">-</span> <span class="pi">{</span> <span class="nv">name</span><span class="pi">:</span> <span class="nv">post-reusability-metric</span><span class="pi">,</span> <span class="nv">method</span><span class="pi">:</span> <span class="nv">POST</span> <span class="pi">}</span>

  <span class="na">exposes</span><span class="pi">:</span>
    <span class="pi">-</span> <span class="na">type</span><span class="pi">:</span> <span class="s">rest</span>
      <span class="na">port</span><span class="pi">:</span> <span class="m">8080</span>
      <span class="na">namespace</span><span class="pi">:</span> <span class="s">reusability-rest</span>
      <span class="na">resources</span><span class="pi">:</span>
        <span class="pi">-</span> <span class="na">name</span><span class="pi">:</span> <span class="s">summary</span>
          <span class="na">path</span><span class="pi">:</span> <span class="s2">"</span><span class="s">/reusability-summary"</span>
          <span class="na">operations</span><span class="pi">:</span>
            <span class="pi">-</span> <span class="na">method</span><span class="pi">:</span> <span class="s">GET</span>
              <span class="na">name</span><span class="pi">:</span> <span class="s">get-reusability-summary</span>
              <span class="na">inputParameters</span><span class="pi">:</span>
                <span class="pi">-</span> <span class="pi">{</span> <span class="nv">name</span><span class="pi">:</span> <span class="nv">window</span><span class="pi">,</span> <span class="nv">in</span><span class="pi">:</span> <span class="nv">query</span><span class="pi">,</span> <span class="nv">type</span><span class="pi">:</span> <span class="nv">string</span> <span class="pi">}</span>   <span class="c1"># e.g. "7d"</span>
                <span class="pi">-</span> <span class="pi">{</span> <span class="nv">name</span><span class="pi">:</span> <span class="nv">team</span><span class="pi">,</span>   <span class="nv">in</span><span class="pi">:</span> <span class="nv">query</span><span class="pi">,</span> <span class="nv">type</span><span class="pi">:</span> <span class="nv">string</span> <span class="pi">}</span>

    <span class="pi">-</span> <span class="na">type</span><span class="pi">:</span> <span class="s">mcp</span>
      <span class="na">port</span><span class="pi">:</span> <span class="m">3010</span>
      <span class="na">namespace</span><span class="pi">:</span> <span class="s">reusability-mcp</span>
      <span class="na">tools</span><span class="pi">:</span>
        <span class="pi">-</span> <span class="na">name</span><span class="pi">:</span> <span class="s">reusability-summary</span>
          <span class="na">description</span><span class="pi">:</span> <span class="s2">"</span><span class="s">Return</span><span class="nv"> </span><span class="s">endpoints</span><span class="nv"> </span><span class="s">ranked</span><span class="nv"> </span><span class="s">by</span><span class="nv"> </span><span class="s">redundant</span><span class="nv"> </span><span class="s">traffic,</span><span class="nv"> </span><span class="s">with</span><span class="nv"> </span><span class="s">consumer</span><span class="nv"> </span><span class="s">teams</span><span class="nv"> </span><span class="s">and</span><span class="nv"> </span><span class="s">shadow-instance</span><span class="nv"> </span><span class="s">flags.</span><span class="nv"> </span><span class="s">Same</span><span class="nv"> </span><span class="s">input</span><span class="nv"> </span><span class="s">→</span><span class="nv"> </span><span class="s">same</span><span class="nv"> </span><span class="s">output."</span>
          <span class="na">hints</span><span class="pi">:</span> <span class="pi">{</span> <span class="nv">readOnly</span><span class="pi">:</span> <span class="nv">true</span> <span class="pi">}</span>
        <span class="pi">-</span> <span class="na">name</span><span class="pi">:</span> <span class="s">report-reusability-to-observability</span>
          <span class="na">description</span><span class="pi">:</span> <span class="s2">"</span><span class="s">Post</span><span class="nv"> </span><span class="s">the</span><span class="nv"> </span><span class="s">latest</span><span class="nv"> </span><span class="s">reusability</span><span class="nv"> </span><span class="s">summary</span><span class="nv"> </span><span class="s">back</span><span class="nv"> </span><span class="s">to</span><span class="nv"> </span><span class="s">Splunk</span><span class="nv"> </span><span class="s">+</span><span class="nv"> </span><span class="s">New</span><span class="nv"> </span><span class="s">Relic</span><span class="nv"> </span><span class="s">+</span><span class="nv"> </span><span class="s">Datadog</span><span class="nv"> </span><span class="s">as</span><span class="nv"> </span><span class="s">a</span><span class="nv"> </span><span class="s">custom</span><span class="nv"> </span><span class="s">metric</span><span class="nv"> </span><span class="s">the</span><span class="nv"> </span><span class="s">platform</span><span class="nv"> </span><span class="s">dashboard</span><span class="nv"> </span><span class="s">already</span><span class="nv"> </span><span class="s">watches."</span>
          <span class="na">hints</span><span class="pi">:</span> <span class="pi">{</span> <span class="nv">readOnly</span><span class="pi">:</span> <span class="nv">false</span><span class="pi">,</span> <span class="nv">idempotent</span><span class="pi">:</span> <span class="nv">true</span> <span class="pi">}</span>
</code></pre></div></div>

<h2 id="capability-three--governance-against-a-runnable-spec-fed-back-into-the-platform">Capability three — Governance against a runnable spec, fed back into the platform</h2>

<p>The third capability closes the loop. It pulls specs from every gateway in the estate, normalizes them, applies the governance ruleset against the runnable artifact, and republishes the enriched, governance-checked specs back to the developer portal <em>and</em> into an MCP surface that developers consume from inside the IDE.</p>

<p>The portal is no longer the thing developers ignore. The portal is the thing the runtime keeps current.</p>

<p>The MCP surface is what makes the loop matter to the AI program. A developer in Claude or Copilot asks <em>“is there already an API that does X?”</em> — and the answer is grounded in the live spec inventory, the live reusability data, and the live governance posture, because all three live in the same capability runtime. There is no intermediate prompt-engineering layer, no agent guessing at filenames, no hallucinated endpoint. The MCP tool returns the spec the gateway is actually running.</p>

<div class="language-yaml highlighter-rouge"><div class="highlight"><pre class="highlight"><code><span class="na">naftiko</span><span class="pi">:</span> <span class="s2">"</span><span class="s">1.0.0-alpha2"</span>

<span class="na">info</span><span class="pi">:</span>
  <span class="na">title</span><span class="pi">:</span> <span class="s">Gateway OpenAPI Crawl + Governance Feedback</span>
  <span class="na">description</span><span class="pi">:</span> <span class="pi">&gt;-</span>
    <span class="s">Pulls OpenAPI specs out of every API gateway in the estate, normalizes</span>
    <span class="s">them, runs the governance ruleset against the runnable artifact, and</span>
    <span class="s">republishes the enriched, governance-checked specs to the developer</span>
    <span class="s">portal AND to an MCP surface IDE-attached agents call.</span>
  <span class="na">tags</span><span class="pi">:</span>
    <span class="pi">-</span> <span class="s">API Governance</span>
    <span class="pi">-</span> <span class="s">Gateway</span>
    <span class="pi">-</span> <span class="s">Developer Portal</span>
    <span class="pi">-</span> <span class="s">MCP</span>

<span class="na">binds</span><span class="pi">:</span>
  <span class="pi">-</span> <span class="na">namespace</span><span class="pi">:</span> <span class="s">aws-env</span>
    <span class="na">keys</span><span class="pi">:</span>
      <span class="na">AWS_ACCESS_KEY_ID</span><span class="pi">:</span> <span class="s">AWS_ACCESS_KEY_ID</span>
      <span class="na">AWS_SECRET_ACCESS_KEY</span><span class="pi">:</span> <span class="s">AWS_SECRET_ACCESS_KEY</span>
      <span class="na">AWS_REGION</span><span class="pi">:</span> <span class="s">AWS_REGION</span>
  <span class="pi">-</span> <span class="na">namespace</span><span class="pi">:</span> <span class="s">portal-env</span>
    <span class="na">keys</span><span class="pi">:</span> <span class="pi">{</span> <span class="nv">PORTAL_TOKEN</span><span class="pi">:</span> <span class="nv">PORTAL_TOKEN</span><span class="pi">,</span> <span class="nv">PORTAL_BASE</span><span class="pi">:</span> <span class="nv">PORTAL_BASE</span> <span class="pi">}</span>

<span class="na">capability</span><span class="pi">:</span>
  <span class="na">consumes</span><span class="pi">:</span>
    <span class="pi">-</span> <span class="na">namespace</span><span class="pi">:</span> <span class="s">apigateway</span>
      <span class="na">type</span><span class="pi">:</span> <span class="s">http</span>
      <span class="na">baseUri</span><span class="pi">:</span> <span class="s2">"</span><span class="s">https://apigateway..amazonaws.com"</span>
      <span class="na">authentication</span><span class="pi">:</span>
        <span class="na">type</span><span class="pi">:</span> <span class="s">aws-sig-v4</span>
        <span class="na">accessKeyId</span><span class="pi">:</span> <span class="s2">"</span><span class="s">"</span>
        <span class="na">secretKey</span><span class="pi">:</span> <span class="s2">"</span><span class="s">"</span>
        <span class="na">region</span><span class="pi">:</span> <span class="s2">"</span><span class="s">"</span>
        <span class="na">service</span><span class="pi">:</span> <span class="s">apigateway</span>
      <span class="na">resources</span><span class="pi">:</span>
        <span class="pi">-</span> <span class="na">name</span><span class="pi">:</span> <span class="s">rest-apis</span>
          <span class="na">path</span><span class="pi">:</span> <span class="s2">"</span><span class="s">/restapis"</span>
          <span class="na">operations</span><span class="pi">:</span>
            <span class="pi">-</span> <span class="pi">{</span> <span class="nv">name</span><span class="pi">:</span> <span class="nv">list-rest-apis</span><span class="pi">,</span> <span class="nv">method</span><span class="pi">:</span> <span class="nv">GET</span> <span class="pi">}</span>
        <span class="pi">-</span> <span class="na">name</span><span class="pi">:</span> <span class="s">rest-api-export</span>
          <span class="na">path</span><span class="pi">:</span> <span class="s2">"</span><span class="s">/restapis//stages//exports/oas30"</span>
          <span class="na">operations</span><span class="pi">:</span>
            <span class="pi">-</span> <span class="na">name</span><span class="pi">:</span> <span class="s">export-rest-api</span>
              <span class="na">method</span><span class="pi">:</span> <span class="s">GET</span>
              <span class="na">inputParameters</span><span class="pi">:</span>
                <span class="pi">-</span> <span class="pi">{</span> <span class="nv">name</span><span class="pi">:</span> <span class="nv">rest_api_id</span><span class="pi">,</span> <span class="nv">in</span><span class="pi">:</span> <span class="nv">path</span> <span class="pi">}</span>
                <span class="pi">-</span> <span class="pi">{</span> <span class="nv">name</span><span class="pi">:</span> <span class="nv">stage_name</span><span class="pi">,</span>  <span class="nv">in</span><span class="pi">:</span> <span class="nv">path</span> <span class="pi">}</span>

    <span class="c1"># Republish target — the developer portal becomes a thing the runtime</span>
    <span class="c1"># keeps current, not a thing the platform team curates by hand.</span>
    <span class="pi">-</span> <span class="na">namespace</span><span class="pi">:</span> <span class="s">portal</span>
      <span class="na">type</span><span class="pi">:</span> <span class="s">http</span>
      <span class="na">baseUri</span><span class="pi">:</span> <span class="s2">"</span><span class="s">"</span>
      <span class="na">authentication</span><span class="pi">:</span> <span class="pi">{</span> <span class="nv">type</span><span class="pi">:</span> <span class="nv">bearer</span><span class="pi">,</span> <span class="nv">token</span><span class="pi">:</span> <span class="s2">"</span><span class="s">"</span> <span class="pi">}</span>
      <span class="na">resources</span><span class="pi">:</span>
        <span class="pi">-</span> <span class="na">name</span><span class="pi">:</span> <span class="s">specs</span>
          <span class="na">path</span><span class="pi">:</span> <span class="s2">"</span><span class="s">/specs/"</span>
          <span class="na">operations</span><span class="pi">:</span>
            <span class="pi">-</span> <span class="na">name</span><span class="pi">:</span> <span class="s">upsert-spec</span>
              <span class="na">method</span><span class="pi">:</span> <span class="s">PUT</span>
              <span class="na">inputParameters</span><span class="pi">:</span>
                <span class="pi">-</span> <span class="pi">{</span> <span class="nv">name</span><span class="pi">:</span> <span class="nv">spec_id</span><span class="pi">,</span> <span class="nv">in</span><span class="pi">:</span> <span class="nv">path</span> <span class="pi">}</span>

  <span class="na">governance</span><span class="pi">:</span>
    <span class="na">rules</span><span class="pi">:</span>
      <span class="pi">-</span> <span class="na">id</span><span class="pi">:</span> <span class="s">requires-operation-summary</span>
        <span class="na">severity</span><span class="pi">:</span> <span class="s">error</span>
      <span class="pi">-</span> <span class="na">id</span><span class="pi">:</span> <span class="s">requires-tagged-owner</span>
        <span class="na">severity</span><span class="pi">:</span> <span class="s">error</span>
      <span class="pi">-</span> <span class="na">id</span><span class="pi">:</span> <span class="s">deprecated-operations-flagged</span>
        <span class="na">severity</span><span class="pi">:</span> <span class="s">warning</span>

  <span class="na">exposes</span><span class="pi">:</span>
    <span class="pi">-</span> <span class="na">type</span><span class="pi">:</span> <span class="s">rest</span>
      <span class="na">port</span><span class="pi">:</span> <span class="m">8080</span>
      <span class="na">namespace</span><span class="pi">:</span> <span class="s">governance-rest</span>
      <span class="na">resources</span><span class="pi">:</span>
        <span class="pi">-</span> <span class="na">name</span><span class="pi">:</span> <span class="s">crawl</span>
          <span class="na">path</span><span class="pi">:</span> <span class="s2">"</span><span class="s">/crawl-and-publish"</span>
          <span class="na">operations</span><span class="pi">:</span>
            <span class="pi">-</span> <span class="pi">{</span> <span class="nv">method</span><span class="pi">:</span> <span class="nv">POST</span><span class="pi">,</span> <span class="nv">name</span><span class="pi">:</span> <span class="nv">crawl-and-publish</span> <span class="pi">}</span>
        <span class="pi">-</span> <span class="na">name</span><span class="pi">:</span> <span class="s">posture</span>
          <span class="na">path</span><span class="pi">:</span> <span class="s2">"</span><span class="s">/posture/"</span>
          <span class="na">operations</span><span class="pi">:</span>
            <span class="pi">-</span> <span class="na">method</span><span class="pi">:</span> <span class="s">GET</span>
              <span class="na">name</span><span class="pi">:</span> <span class="s">get-governance-posture</span>
              <span class="na">inputParameters</span><span class="pi">:</span>
                <span class="pi">-</span> <span class="pi">{</span> <span class="nv">name</span><span class="pi">:</span> <span class="nv">api_id</span><span class="pi">,</span> <span class="nv">in</span><span class="pi">:</span> <span class="nv">path</span> <span class="pi">}</span>

    <span class="pi">-</span> <span class="na">type</span><span class="pi">:</span> <span class="s">mcp</span>
      <span class="na">port</span><span class="pi">:</span> <span class="m">3010</span>
      <span class="na">namespace</span><span class="pi">:</span> <span class="s">governance-mcp</span>
      <span class="na">tools</span><span class="pi">:</span>
        <span class="pi">-</span> <span class="na">name</span><span class="pi">:</span> <span class="s">get-current-spec</span>
          <span class="na">description</span><span class="pi">:</span> <span class="s2">"</span><span class="s">Return</span><span class="nv"> </span><span class="s">the</span><span class="nv"> </span><span class="s">spec</span><span class="nv"> </span><span class="s">the</span><span class="nv"> </span><span class="s">gateway</span><span class="nv"> </span><span class="s">is</span><span class="nv"> </span><span class="s">actually</span><span class="nv"> </span><span class="s">running</span><span class="nv"> </span><span class="s">for</span><span class="nv"> </span><span class="s">an</span><span class="nv"> </span><span class="s">API</span><span class="nv"> </span><span class="s">id</span><span class="nv"> </span><span class="s">—</span><span class="nv"> </span><span class="s">not</span><span class="nv"> </span><span class="s">the</span><span class="nv"> </span><span class="s">wiki</span><span class="nv"> </span><span class="s">version,</span><span class="nv"> </span><span class="s">not</span><span class="nv"> </span><span class="s">the</span><span class="nv"> </span><span class="s">slide-deck</span><span class="nv"> </span><span class="s">version."</span>
          <span class="na">hints</span><span class="pi">:</span> <span class="pi">{</span> <span class="nv">readOnly</span><span class="pi">:</span> <span class="nv">true</span> <span class="pi">}</span>
        <span class="pi">-</span> <span class="na">name</span><span class="pi">:</span> <span class="s">get-governance-posture</span>
          <span class="na">description</span><span class="pi">:</span> <span class="s2">"</span><span class="s">Return</span><span class="nv"> </span><span class="s">current</span><span class="nv"> </span><span class="s">governance</span><span class="nv"> </span><span class="s">findings</span><span class="nv"> </span><span class="s">against</span><span class="nv"> </span><span class="s">the</span><span class="nv"> </span><span class="s">live</span><span class="nv"> </span><span class="s">spec</span><span class="nv"> </span><span class="s">—</span><span class="nv"> </span><span class="s">error/warning</span><span class="nv"> </span><span class="s">counts,</span><span class="nv"> </span><span class="s">owner,</span><span class="nv"> </span><span class="s">deprecated</span><span class="nv"> </span><span class="s">operations."</span>
          <span class="na">hints</span><span class="pi">:</span> <span class="pi">{</span> <span class="nv">readOnly</span><span class="pi">:</span> <span class="nv">true</span> <span class="pi">}</span>
</code></pre></div></div>

<h2 id="the-mcp-surface-is-what-makes-this-an-ai-story-not-a-chatbot-story">The MCP surface is what makes this an AI story, not a chatbot story</h2>

<p>Every enterprise has been pitched some version of an internal AI assistant. Most of them are a model with a long tool list and a wiki crawl behind it. The wiki was already stale. The tool list was already unbounded. The agent hallucinates inside the same blast radius every prior internal “search” tool did, plus a confidence the previous tools never had.</p>

<p>A capability with an MCP surface is the opposite shape. The agent is not reasoning over crawled prose; it is calling a deterministic operation that returns a typed, audited result. The reasoning surface stays where reasoning belongs — composing the answer for the developer’s intent — and the data surface stays in the runtime where governance, identity, and cost attribution already live.</p>

<p>Three capabilities, one MCP namespace, and every developer’s IDE is suddenly grounded in the same authoritative API inventory the platform team is producing. <em>“Is anyone else calling this endpoint?”</em> returns the team behind the other consumer. <em>“Is this spec still current?”</em> returns the diff against what the gateway is actually running. <em>“What APIs do we have for ledger postings?”</em> returns a real list, with reusability scores and governance posture.</p>

<p>That is the AI story. Not a chatbot bolted onto a wiki — a grounded, deterministic surface over the same data the platform team is already producing. The AI program and the API program become the same investment.</p>

<h2 id="why-this-maps-cleanly-onto-an-enterprise-budget">Why this maps cleanly onto an enterprise budget</h2>

<p><strong>Technology.</strong> Three capabilities, one runtime, one MCP surface. The deterministic substrate is the same one every audit, identity binding, and cost attribution uses. The AI surface is a thin facade over capabilities the platform team would have built anyway. There is no “AI infrastructure” line item separate from the API platform line item, because they are the same line item.</p>

<p><strong>Business.</strong> The reusability number is finally a number. The discovery surface is finally a queryable inventory. The governance loop is finally tied to a runnable artifact. Each one of these is a thing the platform team has been losing budget arguments about for years. They are now answerable. The CFO can see redundant API spend by team. The portfolio review can see which APIs are candidates for retirement. The platform-as-product story has the metric it was always supposed to have.</p>

<p><strong>Politics.</strong> The AI checkbox is no longer separate from the API platform checkbox. Leadership gets the AI story they were asking for. The platform team gets the funding for the work they were already going to do. The security team gets a deterministic, governed surface they can audit, instead of an unbounded agent they would have to reject. Nobody has to pick a side.</p>

<h2 id="what-this-looks-like-in-practice">What this looks like in practice</h2>

<p>A platform team adopts the three capabilities. The discovery capability is pointed at the GitHub orgs and the gateway accounts. The reusability capability is pointed at Splunk and New Relic. The governance-and-feedback capability is pointed at the gateways that feed the developer portal.</p>

<p>Every developer’s Claude Code, Copilot, or other MCP-capable IDE picks up the same MCP namespace. The platform team’s quarterly report writes itself, because the data is already running through the same observability stack the team uses for everything else. The AI demo writes itself too, because the MCP surface is already grounded in the deterministic capability layer.</p>

<p>There is no second migration. There is no “now we move it to the AI platform.” The API platform <em>is</em> the AI platform, because the same capabilities answer both sets of questions.</p>

<h2 id="the-rule">The rule</h2>

<p>Three capabilities. One runtime. One MCP surface.</p>

<p>If your API platform program and your AI program are still two separate budgets, you have not yet noticed that the same capability layer answers both sets of questions. Discovery, reusability, and governance are not AI questions — but the MCP surface is what makes the answers reachable from inside the tools developers and leadership are already paying for.</p>

<p>The <a href="https://github.com/naftiko/framework">Naftiko Framework</a> runs the capability layer. The <a href="https://github.com/naftiko/fleet">Naftiko Fleet</a> manages it at scale. The MCP surface is what makes the answer reachable from inside Claude, Copilot, and every other agent the enterprise is going to keep buying.</p>

<p>That is the API governance story. That is the API reusability story. That is the API discovery story. And, almost incidentally, that is the AI story the board has been asking for.</p>
