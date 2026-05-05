---
title: "Naftiko Capabilities for Apinizer"
url: "https://naftiko.io/blog/apinizer-gateway-capabilities/"
date: "2026-04-29T00:00:00+00:00"
author: ""
feed_url: "https://naftiko.io/feed"
---
<p><a href="https://github.com/api-evangelist/apinizer#capabilities">Apinizer</a> is an API management and gateway platform focused on enterprise API governance, security, and monitoring. It provides a centralized view of API traffic with deep policy management, endpoint inspection, and metrics collection. The Naftiko capability for Apinizer exposes the full management surface as MCP tools — gateway configuration, endpoint inventory, policy inspection, and real-time metrics.</p>

<h2 id="what-the-capability-covers">What the Capability Covers</h2>

<p>The <code class="language-plaintext highlighter-rouge">gateway-management</code> capability wraps the Apinizer API and covers the four core operational surfaces: the gateways themselves, the endpoints registered on each gateway, the policies applied to each gateway, and the monitoring metrics produced by the platform.</p>

<h2 id="mcp-tools-available">MCP Tools Available</h2>

<ul>
  <li><code class="language-plaintext highlighter-rouge">list-gateways</code> — List all configured Apinizer API gateways with status and configuration</li>
  <li><code class="language-plaintext highlighter-rouge">create-gateway</code> — Create a new Apinizer API gateway</li>
  <li><code class="language-plaintext highlighter-rouge">list-endpoints</code> — List all API endpoints registered on a specific gateway</li>
  <li><code class="language-plaintext highlighter-rouge">list-policies</code> — List all security and traffic policies applied to a gateway</li>
  <li><code class="language-plaintext highlighter-rouge">get-metrics</code> — Retrieve monitoring metrics including request counts, latency, and error rates</li>
</ul>

<h2 id="capability-specification">Capability Specification</h2>

<div class="language-yaml highlighter-rouge"><div class="highlight"><pre class="highlight"><code><span class="na">naftiko</span><span class="pi">:</span> <span class="s2">"</span><span class="s">1.0.0-alpha1"</span>

<span class="na">info</span><span class="pi">:</span>
  <span class="na">name</span><span class="pi">:</span> <span class="s">Apinizer Gateway Management</span>
  <span class="na">description</span><span class="pi">:</span> <span class="s">Manage gateways, inspect endpoints and policies, and monitor metrics across the Apinizer platform</span>
  <span class="na">version</span><span class="pi">:</span> <span class="s">1.0.0</span>

<span class="na">capability</span><span class="pi">:</span>
  <span class="na">consumes</span><span class="pi">:</span>
    <span class="pi">-</span> <span class="na">namespace</span><span class="pi">:</span> <span class="s">apinizer</span>
      <span class="na">type</span><span class="pi">:</span> <span class="s">http</span>
      <span class="na">baseUri</span><span class="pi">:</span> <span class="s">https://api.apinizer.com</span>
      <span class="na">authentication</span><span class="pi">:</span>
        <span class="na">type</span><span class="pi">:</span> <span class="s">bearer</span>
        <span class="na">token</span><span class="pi">:</span> <span class="s2">"</span><span class="s">"</span>
      <span class="na">resources</span><span class="pi">:</span>
        <span class="pi">-</span> <span class="na">name</span><span class="pi">:</span> <span class="s">gateways</span>
          <span class="na">path</span><span class="pi">:</span> <span class="s2">"</span><span class="s">/v1/gateways/{gatewayId}"</span>
          <span class="na">operations</span><span class="pi">:</span>
            <span class="pi">-</span> <span class="na">name</span><span class="pi">:</span> <span class="s">listGateways</span>
              <span class="na">method</span><span class="pi">:</span> <span class="s">GET</span>
            <span class="pi">-</span> <span class="na">name</span><span class="pi">:</span> <span class="s">createGateway</span>
              <span class="na">method</span><span class="pi">:</span> <span class="s">POST</span>
        <span class="pi">-</span> <span class="na">name</span><span class="pi">:</span> <span class="s">endpoints</span>
          <span class="na">path</span><span class="pi">:</span> <span class="s2">"</span><span class="s">/v1/gateways/{gatewayId}/endpoints"</span>
          <span class="na">operations</span><span class="pi">:</span>
            <span class="pi">-</span> <span class="na">name</span><span class="pi">:</span> <span class="s">listEndpoints</span>
              <span class="na">method</span><span class="pi">:</span> <span class="s">GET</span>
        <span class="pi">-</span> <span class="na">name</span><span class="pi">:</span> <span class="s">policies</span>
          <span class="na">path</span><span class="pi">:</span> <span class="s2">"</span><span class="s">/v1/gateways/{gatewayId}/policies"</span>
          <span class="na">operations</span><span class="pi">:</span>
            <span class="pi">-</span> <span class="na">name</span><span class="pi">:</span> <span class="s">listPolicies</span>
              <span class="na">method</span><span class="pi">:</span> <span class="s">GET</span>
        <span class="pi">-</span> <span class="na">name</span><span class="pi">:</span> <span class="s">metrics</span>
          <span class="na">path</span><span class="pi">:</span> <span class="s2">"</span><span class="s">/v1/metrics"</span>
          <span class="na">operations</span><span class="pi">:</span>
            <span class="pi">-</span> <span class="na">name</span><span class="pi">:</span> <span class="s">getMetrics</span>
              <span class="na">method</span><span class="pi">:</span> <span class="s">GET</span>

  <span class="na">exposes</span><span class="pi">:</span>
    <span class="pi">-</span> <span class="na">type</span><span class="pi">:</span> <span class="s">mcp</span>
      <span class="na">address</span><span class="pi">:</span> <span class="s2">"</span><span class="s">0.0.0.0"</span>
      <span class="na">port</span><span class="pi">:</span> <span class="m">9121</span>
      <span class="na">namespace</span><span class="pi">:</span> <span class="s">apinizer-management</span>
      <span class="na">description</span><span class="pi">:</span> <span class="s2">"</span><span class="s">Apinizer</span><span class="nv"> </span><span class="s">gateway</span><span class="nv"> </span><span class="s">management</span><span class="nv"> </span><span class="s">—</span><span class="nv"> </span><span class="s">inspect</span><span class="nv"> </span><span class="s">gateways,</span><span class="nv"> </span><span class="s">endpoints,</span><span class="nv"> </span><span class="s">policies,</span><span class="nv"> </span><span class="s">and</span><span class="nv"> </span><span class="s">retrieve</span><span class="nv"> </span><span class="s">monitoring</span><span class="nv"> </span><span class="s">metrics"</span>
      <span class="na">tools</span><span class="pi">:</span>
        <span class="pi">-</span> <span class="na">name</span><span class="pi">:</span> <span class="s">list-gateways</span>
          <span class="na">description</span><span class="pi">:</span> <span class="s2">"</span><span class="s">List</span><span class="nv"> </span><span class="s">all</span><span class="nv"> </span><span class="s">configured</span><span class="nv"> </span><span class="s">Apinizer</span><span class="nv"> </span><span class="s">API</span><span class="nv"> </span><span class="s">gateways</span><span class="nv"> </span><span class="s">with</span><span class="nv"> </span><span class="s">their</span><span class="nv"> </span><span class="s">status</span><span class="nv"> </span><span class="s">and</span><span class="nv"> </span><span class="s">configuration"</span>
          <span class="na">hints</span><span class="pi">:</span>
            <span class="na">readOnly</span><span class="pi">:</span> <span class="no">true</span>
          <span class="na">call</span><span class="pi">:</span> <span class="s">apinizer.listGateways</span>
          <span class="na">outputParameters</span><span class="pi">:</span>
            <span class="pi">-</span> <span class="na">type</span><span class="pi">:</span> <span class="s">array</span>
              <span class="na">mapping</span><span class="pi">:</span> <span class="s2">"</span><span class="s">$."</span>
              <span class="na">items</span><span class="pi">:</span>
                <span class="na">type</span><span class="pi">:</span> <span class="s">object</span>

        <span class="pi">-</span> <span class="na">name</span><span class="pi">:</span> <span class="s">create-gateway</span>
          <span class="na">description</span><span class="pi">:</span> <span class="s2">"</span><span class="s">Create</span><span class="nv"> </span><span class="s">a</span><span class="nv"> </span><span class="s">new</span><span class="nv"> </span><span class="s">Apinizer</span><span class="nv"> </span><span class="s">API</span><span class="nv"> </span><span class="s">gateway"</span>
          <span class="na">hints</span><span class="pi">:</span>
            <span class="na">readOnly</span><span class="pi">:</span> <span class="no">false</span>
            <span class="na">destructive</span><span class="pi">:</span> <span class="no">false</span>
            <span class="na">idempotent</span><span class="pi">:</span> <span class="no">false</span>
          <span class="na">call</span><span class="pi">:</span> <span class="s">apinizer.createGateway</span>
          <span class="na">inputParameters</span><span class="pi">:</span>
            <span class="pi">-</span> <span class="na">name</span><span class="pi">:</span> <span class="s">body</span>
              <span class="na">type</span><span class="pi">:</span> <span class="s">object</span>
              <span class="na">required</span><span class="pi">:</span> <span class="no">true</span>
              <span class="na">description</span><span class="pi">:</span> <span class="s2">"</span><span class="s">Gateway</span><span class="nv"> </span><span class="s">configuration</span><span class="nv"> </span><span class="s">including</span><span class="nv"> </span><span class="s">name,</span><span class="nv"> </span><span class="s">type,</span><span class="nv"> </span><span class="s">and</span><span class="nv"> </span><span class="s">connection</span><span class="nv"> </span><span class="s">settings"</span>
          <span class="na">outputParameters</span><span class="pi">:</span>
            <span class="pi">-</span> <span class="na">type</span><span class="pi">:</span> <span class="s">object</span>
              <span class="na">mapping</span><span class="pi">:</span> <span class="s2">"</span><span class="s">$."</span>

        <span class="pi">-</span> <span class="na">name</span><span class="pi">:</span> <span class="s">list-endpoints</span>
          <span class="na">description</span><span class="pi">:</span> <span class="s2">"</span><span class="s">List</span><span class="nv"> </span><span class="s">all</span><span class="nv"> </span><span class="s">API</span><span class="nv"> </span><span class="s">endpoints</span><span class="nv"> </span><span class="s">registered</span><span class="nv"> </span><span class="s">on</span><span class="nv"> </span><span class="s">a</span><span class="nv"> </span><span class="s">specific</span><span class="nv"> </span><span class="s">Apinizer</span><span class="nv"> </span><span class="s">gateway"</span>
          <span class="na">hints</span><span class="pi">:</span>
            <span class="na">readOnly</span><span class="pi">:</span> <span class="no">true</span>
          <span class="na">call</span><span class="pi">:</span> <span class="s">apinizer.listEndpoints</span>
          <span class="na">inputParameters</span><span class="pi">:</span>
            <span class="pi">-</span> <span class="na">name</span><span class="pi">:</span> <span class="s">gatewayId</span>
              <span class="na">type</span><span class="pi">:</span> <span class="s">string</span>
              <span class="na">required</span><span class="pi">:</span> <span class="no">true</span>
              <span class="na">description</span><span class="pi">:</span> <span class="s2">"</span><span class="s">Gateway</span><span class="nv"> </span><span class="s">ID"</span>
          <span class="na">outputParameters</span><span class="pi">:</span>
            <span class="pi">-</span> <span class="na">type</span><span class="pi">:</span> <span class="s">array</span>
              <span class="na">mapping</span><span class="pi">:</span> <span class="s2">"</span><span class="s">$."</span>
              <span class="na">items</span><span class="pi">:</span>
                <span class="na">type</span><span class="pi">:</span> <span class="s">object</span>

        <span class="pi">-</span> <span class="na">name</span><span class="pi">:</span> <span class="s">list-policies</span>
          <span class="na">description</span><span class="pi">:</span> <span class="s2">"</span><span class="s">List</span><span class="nv"> </span><span class="s">all</span><span class="nv"> </span><span class="s">security</span><span class="nv"> </span><span class="s">and</span><span class="nv"> </span><span class="s">traffic</span><span class="nv"> </span><span class="s">policies</span><span class="nv"> </span><span class="s">applied</span><span class="nv"> </span><span class="s">to</span><span class="nv"> </span><span class="s">a</span><span class="nv"> </span><span class="s">specific</span><span class="nv"> </span><span class="s">Apinizer</span><span class="nv"> </span><span class="s">gateway"</span>
          <span class="na">hints</span><span class="pi">:</span>
            <span class="na">readOnly</span><span class="pi">:</span> <span class="no">true</span>
          <span class="na">call</span><span class="pi">:</span> <span class="s">apinizer.listPolicies</span>
          <span class="na">inputParameters</span><span class="pi">:</span>
            <span class="pi">-</span> <span class="na">name</span><span class="pi">:</span> <span class="s">gatewayId</span>
              <span class="na">type</span><span class="pi">:</span> <span class="s">string</span>
              <span class="na">required</span><span class="pi">:</span> <span class="no">true</span>
              <span class="na">description</span><span class="pi">:</span> <span class="s2">"</span><span class="s">Gateway</span><span class="nv"> </span><span class="s">ID"</span>
          <span class="na">outputParameters</span><span class="pi">:</span>
            <span class="pi">-</span> <span class="na">type</span><span class="pi">:</span> <span class="s">array</span>
              <span class="na">mapping</span><span class="pi">:</span> <span class="s2">"</span><span class="s">$."</span>
              <span class="na">items</span><span class="pi">:</span>
                <span class="na">type</span><span class="pi">:</span> <span class="s">object</span>

        <span class="pi">-</span> <span class="na">name</span><span class="pi">:</span> <span class="s">get-metrics</span>
          <span class="na">description</span><span class="pi">:</span> <span class="s2">"</span><span class="s">Retrieve</span><span class="nv"> </span><span class="s">monitoring</span><span class="nv"> </span><span class="s">metrics</span><span class="nv"> </span><span class="s">including</span><span class="nv"> </span><span class="s">request</span><span class="nv"> </span><span class="s">counts,</span><span class="nv"> </span><span class="s">latency,</span><span class="nv"> </span><span class="s">and</span><span class="nv"> </span><span class="s">error</span><span class="nv"> </span><span class="s">rates</span><span class="nv"> </span><span class="s">from</span><span class="nv"> </span><span class="s">Apinizer"</span>
          <span class="na">hints</span><span class="pi">:</span>
            <span class="na">readOnly</span><span class="pi">:</span> <span class="no">true</span>
          <span class="na">call</span><span class="pi">:</span> <span class="s">apinizer.getMetrics</span>
          <span class="na">outputParameters</span><span class="pi">:</span>
            <span class="pi">-</span> <span class="na">type</span><span class="pi">:</span> <span class="s">object</span>
              <span class="na">mapping</span><span class="pi">:</span> <span class="s2">"</span><span class="s">$."</span>
</code></pre></div></div>

<h2 id="running-it">Running It</h2>

<div class="language-bash highlighter-rouge"><div class="highlight"><pre class="highlight"><code>docker pull ghcr.io/naftiko/framework:latest

<span class="c"># Set APINIZER_API_KEY in your .env file</span>
docker run <span class="nt">-p</span> 9121:9121 <span class="se">\</span>
  <span class="nt">-v</span> ./capabilities/gateway-management.yaml:/app/capability.yaml <span class="se">\</span>
  <span class="nt">--env-file</span> .env <span class="se">\</span>
  ghcr.io/naftiko/framework:latest /app/capability.yaml
</code></pre></div></div>

<p>The combination of <code class="language-plaintext highlighter-rouge">list-endpoints</code>, <code class="language-plaintext highlighter-rouge">list-policies</code>, and <code class="language-plaintext highlighter-rouge">get-metrics</code> gives an AI agent everything it needs to run a policy compliance check — enumerate endpoints, verify the expected policies are applied to each, and correlate against traffic metrics to surface uncovered endpoints. The Apinizer capability is part of the <a href="https://github.com/naftiko/fleet?utm_source=naftiko-blog&amp;utm_medium=blog&amp;utm_campaign=product-api-gateway-capabilities&amp;utm_content=gateway-capabilities-apinizer">Naftiko Fleet</a>.</p>
