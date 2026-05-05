---
title: "Naftiko Capabilities for Azure API Management"
url: "https://naftiko.io/blog/azure-api-management-gateway-capabilities/"
date: "2026-04-30T00:00:00+00:00"
author: ""
feed_url: "https://naftiko.io/feed"
---
<p><a href="https://github.com/api-evangelist/azure-api-management#capabilities">Azure API Management</a> is Microsoft’s enterprise API gateway platform, tightly integrated with the Azure ecosystem. It handles API lifecycle management, developer onboarding, subscription and product management, policy enforcement, and AI gateway routing for Azure OpenAI and other AI services. The Naftiko capability set for Azure APIM covers these surfaces across four workflow capabilities.</p>

<h2 id="what-the-capability-covers">What the Capability Covers</h2>

<p>The Azure APIM capability set spans four workflow capabilities: AI Gateway Management, API Lifecycle Management, Developer Onboarding, and Gateway Operations — covering the full operational surface from AI model routing to certificate management.</p>

<h2 id="key-operations">Key Operations</h2>

<p>API lifecycle, product and subscription management, developer portal, AI gateway configuration for Azure OpenAI, backend management, named values, diagnostics, and certificate management.</p>

<h2 id="capability-specification">Capability Specification</h2>

<div class="language-yaml highlighter-rouge"><div class="highlight"><pre class="highlight"><code><span class="na">naftiko</span><span class="pi">:</span> <span class="s2">"</span><span class="s">1.0.0-alpha1"</span>

<span class="na">info</span><span class="pi">:</span>
  <span class="na">label</span><span class="pi">:</span> <span class="s">API Lifecycle Management</span>
  <span class="na">description</span><span class="pi">:</span> <span class="pi">&gt;-</span>
    <span class="s">Workflow capability for API Platform Admins to manage the full API lifecycle</span>
    <span class="s">including creating and versioning APIs, configuring policies, managing</span>
    <span class="s">products and subscriptions, organizing with tags, and maintaining backends</span>
    <span class="s">and certificates across Azure API Management.</span>
  <span class="na">tags</span><span class="pi">:</span>
    <span class="pi">-</span> <span class="s">API Lifecycle</span>
    <span class="pi">-</span> <span class="s">Platform Administration</span>
    <span class="pi">-</span> <span class="s">API Governance</span>
    <span class="pi">-</span> <span class="s">Products</span>
    <span class="pi">-</span> <span class="s">Subscriptions</span>
    <span class="pi">-</span> <span class="s">Policies</span>

<span class="na">capability</span><span class="pi">:</span>
  <span class="na">consumes</span><span class="pi">:</span>
    <span class="pi">-</span> <span class="na">type</span><span class="pi">:</span> <span class="s">http</span>
      <span class="na">namespace</span><span class="pi">:</span> <span class="s">azure-apim-rest</span>
      <span class="na">baseUri</span><span class="pi">:</span> <span class="s">https://management.azure.com</span>
      <span class="na">auth</span><span class="pi">:</span>
        <span class="na">type</span><span class="pi">:</span> <span class="s">oauth2</span>
        <span class="na">scopes</span><span class="pi">:</span>
          <span class="pi">-</span> <span class="s">user_impersonation</span>
      <span class="na">resources</span><span class="pi">:</span>
        <span class="pi">-</span> <span class="na">name</span><span class="pi">:</span> <span class="s">api</span>
          <span class="na">label</span><span class="pi">:</span> <span class="s">APIs</span>
          <span class="na">path</span><span class="pi">:</span> <span class="s">/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.ApiManagement/service/{serviceName}/apis/{apiId}</span>
          <span class="na">operations</span><span class="pi">:</span>
            <span class="pi">-</span> <span class="na">name</span><span class="pi">:</span> <span class="s">Api_ListByService</span>
              <span class="na">label</span><span class="pi">:</span> <span class="s">List APIs</span>
              <span class="na">method</span><span class="pi">:</span> <span class="s">GET</span>
            <span class="pi">-</span> <span class="na">name</span><span class="pi">:</span> <span class="s">Api_Get</span>
              <span class="na">label</span><span class="pi">:</span> <span class="s">Get API</span>
              <span class="na">method</span><span class="pi">:</span> <span class="s">GET</span>
            <span class="pi">-</span> <span class="na">name</span><span class="pi">:</span> <span class="s">Api_CreateOrUpdate</span>
              <span class="na">label</span><span class="pi">:</span> <span class="s">Create or Update API</span>
              <span class="na">method</span><span class="pi">:</span> <span class="s">PUT</span>
            <span class="pi">-</span> <span class="na">name</span><span class="pi">:</span> <span class="s">Api_Delete</span>
              <span class="na">label</span><span class="pi">:</span> <span class="s">Delete API</span>
              <span class="na">method</span><span class="pi">:</span> <span class="s">DELETE</span>

        <span class="pi">-</span> <span class="na">name</span><span class="pi">:</span> <span class="s">product</span>
          <span class="na">label</span><span class="pi">:</span> <span class="s">Products</span>
          <span class="na">path</span><span class="pi">:</span> <span class="s">/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.ApiManagement/service/{serviceName}/products/{productId}</span>
          <span class="na">operations</span><span class="pi">:</span>
            <span class="pi">-</span> <span class="na">name</span><span class="pi">:</span> <span class="s">Product_ListByService</span>
              <span class="na">label</span><span class="pi">:</span> <span class="s">List Products</span>
              <span class="na">method</span><span class="pi">:</span> <span class="s">GET</span>
            <span class="pi">-</span> <span class="na">name</span><span class="pi">:</span> <span class="s">Product_CreateOrUpdate</span>
              <span class="na">label</span><span class="pi">:</span> <span class="s">Create or Update Product</span>
              <span class="na">method</span><span class="pi">:</span> <span class="s">PUT</span>

        <span class="pi">-</span> <span class="na">name</span><span class="pi">:</span> <span class="s">subscription</span>
          <span class="na">label</span><span class="pi">:</span> <span class="s">Subscriptions</span>
          <span class="na">path</span><span class="pi">:</span> <span class="s">/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.ApiManagement/service/{serviceName}/subscriptions/{sid}</span>
          <span class="na">operations</span><span class="pi">:</span>
            <span class="pi">-</span> <span class="na">name</span><span class="pi">:</span> <span class="s">Subscription_List</span>
              <span class="na">label</span><span class="pi">:</span> <span class="s">List Subscriptions</span>
              <span class="na">method</span><span class="pi">:</span> <span class="s">GET</span>
            <span class="pi">-</span> <span class="na">name</span><span class="pi">:</span> <span class="s">Subscription_CreateOrUpdate</span>
              <span class="na">label</span><span class="pi">:</span> <span class="s">Create Subscription</span>
              <span class="na">method</span><span class="pi">:</span> <span class="s">PUT</span>

  <span class="na">exposes</span><span class="pi">:</span>
    <span class="pi">-</span> <span class="na">type</span><span class="pi">:</span> <span class="s">mcp</span>
      <span class="na">port</span><span class="pi">:</span> <span class="m">9100</span>
      <span class="na">namespace</span><span class="pi">:</span> <span class="s">azure-apim-lifecycle-mcp</span>
      <span class="na">description</span><span class="pi">:</span> <span class="s2">"</span><span class="s">MCP</span><span class="nv"> </span><span class="s">tools</span><span class="nv"> </span><span class="s">for</span><span class="nv"> </span><span class="s">Azure</span><span class="nv"> </span><span class="s">API</span><span class="nv"> </span><span class="s">Management</span><span class="nv"> </span><span class="s">lifecycle</span><span class="nv"> </span><span class="s">operations."</span>
      <span class="na">tools</span><span class="pi">:</span>
        <span class="pi">-</span> <span class="na">name</span><span class="pi">:</span> <span class="s">list-apis</span>
          <span class="na">description</span><span class="pi">:</span> <span class="s2">"</span><span class="s">List</span><span class="nv"> </span><span class="s">all</span><span class="nv"> </span><span class="s">APIs</span><span class="nv"> </span><span class="s">in</span><span class="nv"> </span><span class="s">the</span><span class="nv"> </span><span class="s">Azure</span><span class="nv"> </span><span class="s">API</span><span class="nv"> </span><span class="s">Management</span><span class="nv"> </span><span class="s">service."</span>
          <span class="na">hints</span><span class="pi">:</span>
            <span class="na">readOnly</span><span class="pi">:</span> <span class="no">true</span>
          <span class="na">call</span><span class="pi">:</span> <span class="s">azure-apim-rest.Api_ListByService</span>
          <span class="na">outputParameters</span><span class="pi">:</span>
            <span class="pi">-</span> <span class="na">type</span><span class="pi">:</span> <span class="s">object</span>
              <span class="na">mapping</span><span class="pi">:</span> <span class="s2">"</span><span class="s">$."</span>

        <span class="pi">-</span> <span class="na">name</span><span class="pi">:</span> <span class="s">list-products</span>
          <span class="na">description</span><span class="pi">:</span> <span class="s2">"</span><span class="s">List</span><span class="nv"> </span><span class="s">all</span><span class="nv"> </span><span class="s">API</span><span class="nv"> </span><span class="s">products</span><span class="nv"> </span><span class="s">configured</span><span class="nv"> </span><span class="s">in</span><span class="nv"> </span><span class="s">Azure</span><span class="nv"> </span><span class="s">API</span><span class="nv"> </span><span class="s">Management."</span>
          <span class="na">hints</span><span class="pi">:</span>
            <span class="na">readOnly</span><span class="pi">:</span> <span class="no">true</span>
          <span class="na">call</span><span class="pi">:</span> <span class="s">azure-apim-rest.Product_ListByService</span>
          <span class="na">outputParameters</span><span class="pi">:</span>
            <span class="pi">-</span> <span class="na">type</span><span class="pi">:</span> <span class="s">object</span>
              <span class="na">mapping</span><span class="pi">:</span> <span class="s2">"</span><span class="s">$."</span>

        <span class="pi">-</span> <span class="na">name</span><span class="pi">:</span> <span class="s">list-subscriptions</span>
          <span class="na">description</span><span class="pi">:</span> <span class="s2">"</span><span class="s">List</span><span class="nv"> </span><span class="s">all</span><span class="nv"> </span><span class="s">subscriptions</span><span class="nv"> </span><span class="s">granting</span><span class="nv"> </span><span class="s">access</span><span class="nv"> </span><span class="s">to</span><span class="nv"> </span><span class="s">API</span><span class="nv"> </span><span class="s">products."</span>
          <span class="na">hints</span><span class="pi">:</span>
            <span class="na">readOnly</span><span class="pi">:</span> <span class="no">true</span>
          <span class="na">call</span><span class="pi">:</span> <span class="s">azure-apim-rest.Subscription_List</span>
          <span class="na">outputParameters</span><span class="pi">:</span>
            <span class="pi">-</span> <span class="na">type</span><span class="pi">:</span> <span class="s">object</span>
              <span class="na">mapping</span><span class="pi">:</span> <span class="s2">"</span><span class="s">$."</span>
</code></pre></div></div>

<h2 id="running-it">Running It</h2>

<div class="language-bash highlighter-rouge"><div class="highlight"><pre class="highlight"><code>docker pull ghcr.io/naftiko/framework:latest

<span class="c"># Set AZURE_SUBSCRIPTION_ID, AZURE_RESOURCE_GROUP, AZURE_APIM_NAME, AZURE_ACCESS_TOKEN in .env</span>
docker run <span class="nt">-p</span> 9100:9100 <span class="se">\</span>
  <span class="nt">-v</span> ./capabilities/api-lifecycle-management.yaml:/app/capability.yaml <span class="se">\</span>
  <span class="nt">--env-file</span> .env <span class="se">\</span>
  ghcr.io/naftiko/framework:latest /app/capability.yaml
</code></pre></div></div>

<p>Azure APIM customers building AI workflows have a powerful combination available: the AI gateway capability manages Azure OpenAI routing and policies, while the API lifecycle capability gives agents full visibility into what APIs those AI services are exposing downstream. The Azure API Management capability set is part of the <a href="https://github.com/naftiko/fleet?utm_source=naftiko-blog&amp;utm_medium=blog&amp;utm_campaign=product-api-gateway-capabilities&amp;utm_content=gateway-capabilities-azure-api-management">Naftiko Fleet</a>.</p>
