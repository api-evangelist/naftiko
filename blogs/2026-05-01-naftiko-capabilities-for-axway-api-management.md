---
title: "Naftiko Capabilities for Axway API Management"
url: "https://naftiko.io/blog/axway-gateway-capabilities/"
date: "2026-05-01T00:00:00+00:00"
author: ""
feed_url: "https://naftiko.io/feed"
---
<p><a href="https://github.com/api-evangelist/axway#capabilities">Axway API Management</a> is a full-lifecycle API management platform with deep enterprise integration, identity federation, and multi-organization management capabilities. The Naftiko capability set for Axway is one of the most comprehensive in the fleet, covering applications, analytics, identity, and organization management across four capability files.</p>

<h2 id="what-the-capability-covers">What the Capability Covers</h2>

<p>Four workflow capabilities: Application and Analytics, Identity and Access, Identity Provider Management, and Organization Management — covering app lifecycle, authentication, SAML/OIDC federation, and multi-org hierarchy.</p>

<h2 id="key-mcp-tools">Key MCP Tools</h2>

<p><strong>Organizations:</strong> <code class="language-plaintext highlighter-rouge">org-findOne</code>, <code class="language-plaintext highlighter-rouge">org-userFind</code>, <code class="language-plaintext highlighter-rouge">org-userCreate</code>, <code class="language-plaintext highlighter-rouge">org-userUpdate</code>, <code class="language-plaintext highlighter-rouge">org-stats</code>, <code class="language-plaintext highlighter-rouge">org-findSubscriptions</code>, <code class="language-plaintext highlighter-rouge">org-findEnvs</code></p>

<p><strong>Teams:</strong> <code class="language-plaintext highlighter-rouge">team-create</code>, <code class="language-plaintext highlighter-rouge">team-find</code>, <code class="language-plaintext highlighter-rouge">team-userAdd</code>, <code class="language-plaintext highlighter-rouge">team-userRemove</code>, <code class="language-plaintext highlighter-rouge">team-userUpdateRole</code></p>

<p><strong>Identity Providers:</strong> <code class="language-plaintext highlighter-rouge">provider-idpCreateSAML</code>, <code class="language-plaintext highlighter-rouge">provider-idpCreateOIDC</code>, <code class="language-plaintext highlighter-rouge">org-idpAssociate</code>, <code class="language-plaintext highlighter-rouge">org-idpFind</code></p>

<p><strong>Analytics:</strong> <code class="language-plaintext highlighter-rouge">analytics-query</code>, <code class="language-plaintext highlighter-rouge">analytics-error</code>, <code class="language-plaintext highlighter-rouge">usage-find</code>, <code class="language-plaintext highlighter-rouge">activity-find</code></p>

<p><strong>Auth:</strong> <code class="language-plaintext highlighter-rouge">auth-login</code>, <code class="language-plaintext highlighter-rouge">auth-logout</code>, <code class="language-plaintext highlighter-rouge">auth-mfaVerify</code>, <code class="language-plaintext highlighter-rouge">auth-sessionFind</code></p>

<h2 id="capability-specification">Capability Specification</h2>

<div class="language-yaml highlighter-rouge"><div class="highlight"><pre class="highlight"><code><span class="na">naftiko</span><span class="pi">:</span> <span class="s">1.0.0-alpha1</span>

<span class="na">info</span><span class="pi">:</span>
  <span class="na">label</span><span class="pi">:</span> <span class="s">Axway Organization Management</span>
  <span class="na">description</span><span class="pi">:</span> <span class="s">Manage organizations, subscriptions, domains, entitlements, and environments on the Axway Amplify platform.</span>
  <span class="na">tags</span><span class="pi">:</span>
    <span class="pi">-</span> <span class="s">Axway</span>
    <span class="pi">-</span> <span class="s">Organization</span>
    <span class="pi">-</span> <span class="s">Enterprise</span>
    <span class="pi">-</span> <span class="s">Administration</span>

<span class="na">binds</span><span class="pi">:</span>
  <span class="pi">-</span> <span class="na">namespace</span><span class="pi">:</span> <span class="s">env</span>
    <span class="na">keys</span><span class="pi">:</span>
      <span class="na">AXWAY_BEARER_TOKEN</span><span class="pi">:</span> <span class="s">AXWAY_BEARER_TOKEN</span>

<span class="na">capability</span><span class="pi">:</span>
  <span class="na">consumes</span><span class="pi">:</span>
    <span class="pi">-</span> <span class="na">namespace</span><span class="pi">:</span> <span class="s">amplify-platform</span>
      <span class="na">type</span><span class="pi">:</span> <span class="s">http</span>
      <span class="na">baseUri</span><span class="pi">:</span> <span class="s">https://platform.axway.com</span>
      <span class="na">authentication</span><span class="pi">:</span>
        <span class="na">type</span><span class="pi">:</span> <span class="s">bearer</span>
        <span class="na">token</span><span class="pi">:</span> <span class="s2">"</span><span class="s">"</span>
      <span class="na">resources</span><span class="pi">:</span>
        <span class="pi">-</span> <span class="na">name</span><span class="pi">:</span> <span class="s">organizations</span>
          <span class="na">path</span><span class="pi">:</span> <span class="s2">"</span><span class="s">/api/v1/org/{orgId}"</span>
          <span class="na">operations</span><span class="pi">:</span>
            <span class="pi">-</span> <span class="na">name</span><span class="pi">:</span> <span class="s">org-findOne</span>
              <span class="na">method</span><span class="pi">:</span> <span class="s">GET</span>
            <span class="pi">-</span> <span class="na">name</span><span class="pi">:</span> <span class="s">org-stats</span>
              <span class="na">method</span><span class="pi">:</span> <span class="s">GET</span>
            <span class="pi">-</span> <span class="na">name</span><span class="pi">:</span> <span class="s">org-findSubscriptions</span>
              <span class="na">method</span><span class="pi">:</span> <span class="s">GET</span>
        <span class="pi">-</span> <span class="na">name</span><span class="pi">:</span> <span class="s">org-users</span>
          <span class="na">path</span><span class="pi">:</span> <span class="s2">"</span><span class="s">/api/v1/org/{orgId}/user"</span>
          <span class="na">operations</span><span class="pi">:</span>
            <span class="pi">-</span> <span class="na">name</span><span class="pi">:</span> <span class="s">org-userFind</span>
              <span class="na">method</span><span class="pi">:</span> <span class="s">GET</span>
        <span class="pi">-</span> <span class="na">name</span><span class="pi">:</span> <span class="s">teams</span>
          <span class="na">path</span><span class="pi">:</span> <span class="s2">"</span><span class="s">/api/v1/team"</span>
          <span class="na">operations</span><span class="pi">:</span>
            <span class="pi">-</span> <span class="na">name</span><span class="pi">:</span> <span class="s">team-find</span>
              <span class="na">method</span><span class="pi">:</span> <span class="s">GET</span>

  <span class="na">exposes</span><span class="pi">:</span>
    <span class="pi">-</span> <span class="na">type</span><span class="pi">:</span> <span class="s">mcp</span>
      <span class="na">port</span><span class="pi">:</span> <span class="m">9081</span>
      <span class="na">namespace</span><span class="pi">:</span> <span class="s">axway-org-mcp</span>
      <span class="na">description</span><span class="pi">:</span> <span class="s2">"</span><span class="s">MCP</span><span class="nv"> </span><span class="s">tools</span><span class="nv"> </span><span class="s">for</span><span class="nv"> </span><span class="s">Axway</span><span class="nv"> </span><span class="s">organization</span><span class="nv"> </span><span class="s">and</span><span class="nv"> </span><span class="s">identity</span><span class="nv"> </span><span class="s">management."</span>
      <span class="na">tools</span><span class="pi">:</span>
        <span class="pi">-</span> <span class="na">name</span><span class="pi">:</span> <span class="s">org-findOne</span>
          <span class="na">description</span><span class="pi">:</span> <span class="s2">"</span><span class="s">Get</span><span class="nv"> </span><span class="s">details</span><span class="nv"> </span><span class="s">for</span><span class="nv"> </span><span class="s">an</span><span class="nv"> </span><span class="s">Axway</span><span class="nv"> </span><span class="s">organization."</span>
          <span class="na">hints</span><span class="pi">:</span>
            <span class="na">readOnly</span><span class="pi">:</span> <span class="no">true</span>
          <span class="na">call</span><span class="pi">:</span> <span class="s">amplify-platform.org-findOne</span>
          <span class="na">outputParameters</span><span class="pi">:</span>
            <span class="pi">-</span> <span class="na">type</span><span class="pi">:</span> <span class="s">object</span>
              <span class="na">mapping</span><span class="pi">:</span> <span class="s">$.</span>

        <span class="pi">-</span> <span class="na">name</span><span class="pi">:</span> <span class="s">org-userFind</span>
          <span class="na">description</span><span class="pi">:</span> <span class="s2">"</span><span class="s">List</span><span class="nv"> </span><span class="s">users</span><span class="nv"> </span><span class="s">within</span><span class="nv"> </span><span class="s">an</span><span class="nv"> </span><span class="s">Axway</span><span class="nv"> </span><span class="s">organization."</span>
          <span class="na">hints</span><span class="pi">:</span>
            <span class="na">readOnly</span><span class="pi">:</span> <span class="no">true</span>
          <span class="na">call</span><span class="pi">:</span> <span class="s">amplify-platform.org-userFind</span>
          <span class="na">outputParameters</span><span class="pi">:</span>
            <span class="pi">-</span> <span class="na">type</span><span class="pi">:</span> <span class="s">object</span>
              <span class="na">mapping</span><span class="pi">:</span> <span class="s">$.</span>

        <span class="pi">-</span> <span class="na">name</span><span class="pi">:</span> <span class="s">org-stats</span>
          <span class="na">description</span><span class="pi">:</span> <span class="s2">"</span><span class="s">Get</span><span class="nv"> </span><span class="s">usage</span><span class="nv"> </span><span class="s">statistics</span><span class="nv"> </span><span class="s">for</span><span class="nv"> </span><span class="s">an</span><span class="nv"> </span><span class="s">Axway</span><span class="nv"> </span><span class="s">organization."</span>
          <span class="na">hints</span><span class="pi">:</span>
            <span class="na">readOnly</span><span class="pi">:</span> <span class="no">true</span>
          <span class="na">call</span><span class="pi">:</span> <span class="s">amplify-platform.org-stats</span>
          <span class="na">outputParameters</span><span class="pi">:</span>
            <span class="pi">-</span> <span class="na">type</span><span class="pi">:</span> <span class="s">object</span>
              <span class="na">mapping</span><span class="pi">:</span> <span class="s">$.</span>

        <span class="pi">-</span> <span class="na">name</span><span class="pi">:</span> <span class="s">team-find</span>
          <span class="na">description</span><span class="pi">:</span> <span class="s2">"</span><span class="s">List</span><span class="nv"> </span><span class="s">all</span><span class="nv"> </span><span class="s">teams</span><span class="nv"> </span><span class="s">within</span><span class="nv"> </span><span class="s">an</span><span class="nv"> </span><span class="s">organization."</span>
          <span class="na">hints</span><span class="pi">:</span>
            <span class="na">readOnly</span><span class="pi">:</span> <span class="no">true</span>
          <span class="na">call</span><span class="pi">:</span> <span class="s">amplify-platform.team-find</span>
          <span class="na">outputParameters</span><span class="pi">:</span>
            <span class="pi">-</span> <span class="na">type</span><span class="pi">:</span> <span class="s">object</span>
              <span class="na">mapping</span><span class="pi">:</span> <span class="s">$.</span>

        <span class="pi">-</span> <span class="na">name</span><span class="pi">:</span> <span class="s">org-findSubscriptions</span>
          <span class="na">description</span><span class="pi">:</span> <span class="s2">"</span><span class="s">List</span><span class="nv"> </span><span class="s">all</span><span class="nv"> </span><span class="s">subscriptions</span><span class="nv"> </span><span class="s">associated</span><span class="nv"> </span><span class="s">with</span><span class="nv"> </span><span class="s">the</span><span class="nv"> </span><span class="s">organization."</span>
          <span class="na">hints</span><span class="pi">:</span>
            <span class="na">readOnly</span><span class="pi">:</span> <span class="no">true</span>
          <span class="na">call</span><span class="pi">:</span> <span class="s">amplify-platform.org-findSubscriptions</span>
          <span class="na">outputParameters</span><span class="pi">:</span>
            <span class="pi">-</span> <span class="na">type</span><span class="pi">:</span> <span class="s">object</span>
              <span class="na">mapping</span><span class="pi">:</span> <span class="s">$.</span>
</code></pre></div></div>

<h2 id="running-it">Running It</h2>

<div class="language-bash highlighter-rouge"><div class="highlight"><pre class="highlight"><code>docker pull ghcr.io/naftiko/framework:latest

<span class="c"># Set AXWAY_BEARER_TOKEN in your .env file</span>
docker run <span class="nt">-p</span> 9081:9081 <span class="se">\</span>
  <span class="nt">-v</span> ./capabilities/organization-management.yaml:/app/capability.yaml <span class="se">\</span>
  <span class="nt">--env-file</span> .env <span class="se">\</span>
  ghcr.io/naftiko/framework:latest /app/capability.yaml
</code></pre></div></div>

<p>The Axway capabilities are particularly useful for identity and access governance workflows — an AI agent can audit organization membership, check team roles, validate SSO provider configuration, and surface anomalies across your organizational hierarchy. The Axway capability set is part of the <a href="https://github.com/naftiko/fleet?utm_source=naftiko-blog&amp;utm_medium=blog&amp;utm_campaign=product-api-gateway-capabilities&amp;utm_content=gateway-capabilities-axway">Naftiko Fleet</a>.</p>
