---
title: "From Standards to Profiles: Naftiko Capabilities and the Path to IHE-Compliant Healthcare Integration"
url: "https://naftiko.io/blog/from-standards-to-profiles-naftiko-capabilities-for-ihe-integration/"
date: "2026-04-30T00:00:00+00:00"
author: "Kin Lane"
feed_url: "https://naftiko.io/feed"
---
<p>In our last healthcare post, <a href="https://naftiko.io/blog/modernizing-healthcare-integration-with-naftiko-from-hl7-v2-to-openehr/">Modernizing Healthcare Integration with Naftiko: From HL7 v2 to OpenEHR</a>, we showed how a Naftiko Capability turns a pipe-delimited HL7 v2 ADT message into a structured OpenEHR composition. That kind of standard-to-standard translation is where most teams begin. It is also the easy part.</p>

<p>The harder, more valuable layer is what an industry initiative called <a href="https://www.ihe.net/">IHE</a> – Integrating the Healthcare Enterprise – has been doing for over twenty-five years: writing <strong>Integration Profiles</strong> that constrain the underlying standards into specific, testable, multi-vendor workflows. Profiles are how cross-organizational document sharing, patient-identity-cross-referencing, imaging workflows, and (increasingly) European Health Data Space exchanges actually work in production.</p>

<p>This post walks through how Naftiko Capabilities can be used at both layers. First, the translation patterns across HL7 v2, FHIR, DICOM, and OpenEHR. Then, the implementation patterns that let a capability deliver an actual IHE Integration Profile – as a Connectathon-testable actor, a cross-cutting substrate, or an AI-callable MCP tool that no other implementation in the market exposes.</p>

<h2 id="part-1-translation-patterns-across-the-healthcare-standards">Part 1: Translation Patterns Across the Healthcare Standards</h2>

<p>A Naftiko Capability is a YAML file that declares what it consumes, what it produces, and what it exposes to AI agents and tools. The translation use cases line up cleanly with that shape:</p>

<ul>
  <li><strong>HL7 v2 to OpenEHR</strong> – Pipe-delimited ADT, ORM, ORU, RDE messages from a hospital integration engine, turned into structured OpenEHR compositions backed by clinically-authored archetypes. We covered this in detail in the previous post.</li>
  <li><strong>HL7 v2 to FHIR</strong> – The same ADT events surfaced as FHIR Encounter, Patient, and Condition resources for modern application stacks.</li>
  <li><strong>DICOM to FHIR ImagingStudy</strong> – DICOMweb QIDO-RS metadata translated into FHIR ImagingStudy resources for cross-system image references.</li>
  <li><strong>OpenEHR to FHIR</strong> – Canonical OpenEHR compositions read out and projected as FHIR bundles for partners who only speak FHIR.</li>
  <li><strong>FHIR to FHIR</strong> – Profile-to-profile transformations between US Core, IPS, EHDS-aligned, and payer profiles.</li>
</ul>

<p>Each translation is a short capability following the consume-produce-expose pattern. Here is a compact HL7 v2 to FHIR sketch:</p>

<div class="language-yaml highlighter-rouge"><div class="highlight"><pre class="highlight"><code><span class="na">naftiko</span><span class="pi">:</span> <span class="s2">"</span><span class="s">1.0.0-alpha1"</span>

<span class="na">info</span><span class="pi">:</span>
  <span class="na">label</span><span class="pi">:</span> <span class="s">HL7 v2 ADT^A01 Patient Admit to FHIR Encounter</span>
  <span class="na">description</span><span class="pi">:</span> <span class="pi">&gt;-</span>
    <span class="s">Reads HL7 v2 ADT^A01 messages from a hospital integration engine and</span>
    <span class="s">posts FHIR R4 Encounter, Patient, and Condition resources to a target</span>
    <span class="s">FHIR server. Useful for modernizing downstream consumers without</span>
    <span class="s">touching the upstream interface engine.</span>
  <span class="na">tags</span><span class="pi">:</span> <span class="pi">[</span><span class="nv">hl7v2</span><span class="pi">,</span> <span class="nv">fhir</span><span class="pi">,</span> <span class="nv">adt-a01</span><span class="pi">,</span> <span class="nv">healthcare</span><span class="pi">]</span>

<span class="na">capability</span><span class="pi">:</span>
  <span class="na">consumes</span><span class="pi">:</span>
    <span class="pi">-</span> <span class="na">namespace</span><span class="pi">:</span> <span class="s">hl7-gateway</span>
      <span class="na">type</span><span class="pi">:</span> <span class="s">http</span>
      <span class="na">baseUri</span><span class="pi">:</span> <span class="s2">"</span><span class="s">https://hl7-gateway.hospital.example/api/v1"</span>
      <span class="na">resources</span><span class="pi">:</span>
        <span class="pi">-</span> <span class="na">name</span><span class="pi">:</span> <span class="s">adt-events</span>
          <span class="na">path</span><span class="pi">:</span> <span class="s2">"</span><span class="s">/adt/events"</span>
          <span class="na">operations</span><span class="pi">:</span>
            <span class="pi">-</span> <span class="na">name</span><span class="pi">:</span> <span class="s">get-admit-event</span>
              <span class="na">method</span><span class="pi">:</span> <span class="s">GET</span>

    <span class="pi">-</span> <span class="na">namespace</span><span class="pi">:</span> <span class="s">fhir-server</span>
      <span class="na">type</span><span class="pi">:</span> <span class="s">http</span>
      <span class="na">baseUri</span><span class="pi">:</span> <span class="s2">"</span><span class="s">https://fhir.hospital.example/r4"</span>
      <span class="na">resources</span><span class="pi">:</span>
        <span class="pi">-</span> <span class="na">name</span><span class="pi">:</span> <span class="s">encounters</span>
          <span class="na">path</span><span class="pi">:</span> <span class="s2">"</span><span class="s">/Encounter"</span>
          <span class="na">operations</span><span class="pi">:</span>
            <span class="pi">-</span> <span class="na">name</span><span class="pi">:</span> <span class="s">post-encounter</span>
              <span class="na">method</span><span class="pi">:</span> <span class="s">POST</span>

  <span class="na">exposes</span><span class="pi">:</span>
    <span class="pi">-</span> <span class="na">type</span><span class="pi">:</span> <span class="s">mcp</span>
      <span class="na">port</span><span class="pi">:</span> <span class="m">3020</span>
      <span class="na">namespace</span><span class="pi">:</span> <span class="s">hl7-fhir-tools</span>
      <span class="na">tools</span><span class="pi">:</span>
        <span class="pi">-</span> <span class="na">name</span><span class="pi">:</span> <span class="s">transform-adt-a01-to-fhir</span>
          <span class="na">description</span><span class="pi">:</span> <span class="pi">&gt;-</span>
            <span class="s">Fetches an ADT^A01 message and posts the resulting FHIR R4</span>
            <span class="s">Encounter resource to the configured FHIR server.</span>
          <span class="na">inputParameters</span><span class="pi">:</span>
            <span class="pi">-</span> <span class="na">name</span><span class="pi">:</span> <span class="s">messageId</span>
              <span class="na">type</span><span class="pi">:</span> <span class="s">string</span>
              <span class="na">required</span><span class="pi">:</span> <span class="no">true</span>
          <span class="na">steps</span><span class="pi">:</span>
            <span class="pi">-</span> <span class="na">name</span><span class="pi">:</span> <span class="s">fetch-admit</span>
              <span class="na">type</span><span class="pi">:</span> <span class="s">call</span>
              <span class="na">call</span><span class="pi">:</span> <span class="s">hl7-gateway.get-admit-event</span>
              <span class="na">with</span><span class="pi">:</span>
                <span class="na">messageId</span><span class="pi">:</span> <span class="s">hl7-fhir-tools.messageId</span>
            <span class="pi">-</span> <span class="na">name</span><span class="pi">:</span> <span class="s">post-encounter</span>
              <span class="na">type</span><span class="pi">:</span> <span class="s">call</span>
              <span class="na">call</span><span class="pi">:</span> <span class="s">fhir-server.post-encounter</span>
</code></pre></div></div>

<p>Same pattern as the OpenEHR post: consume the legacy gateway, produce the modern resource, expose an MCP tool. Multiply across HL7 v2, FHIR, DICOM, and OpenEHR and you have a translation fleet that retains existing investment without locking the future.</p>

<p>This is genuinely useful work. It is also where most healthcare integration platforms stop.</p>

<h2 id="part-2-from-standards-to-profiles">Part 2: From Standards to Profiles</h2>

<p>Translating standards moves data between formats. <strong>Profiles</strong> specify the workflows that data participates in. The distinction matters.</p>

<p>Take cross-enterprise document sharing. FHIR has a <code class="language-plaintext highlighter-rouge">DocumentReference</code> resource. HL7 v2 has MDM messages. DICOM has Structured Reports. None of them, on their own, tell you how Hospital A’s EMR shares a discharge summary with Hospital B’s primary care system across an organizational boundary. That is what an IHE Integration Profile does.</p>

<p>A profile defines:</p>

<ul>
  <li><strong>Actors</strong> – the system roles that participate, e.g., Document Source, Document Repository, Document Registry, Document Consumer.</li>
  <li><strong>Transactions</strong> – the specific exchanges between those actors, grounded in a base standard (FHIR REST call, HL7 v2 message, DICOM operation).</li>
  <li><strong>Content modules</strong> – which CDA template, which FHIR profile, which DICOM SOP class is required.</li>
  <li><strong>Conformance options</strong> – mandatory versus optional behaviors that products declare at IHE Connectathons.</li>
  <li><strong>Cross-cutting actors</strong> – ATNA (audit), CT (consistent time), XUA (cross-enterprise user assertion). Almost every IHE profile depends on these.</li>
</ul>

<p>A Naftiko Capability is operation-shaped: one consumes operation, one exposes tool, one MCP entry. An IHE profile is workflow-shaped. The interesting design work is mapping one onto the other cleanly. Here is how.</p>

<h2 id="pattern-1-one-capability-per-ihe-actor">Pattern 1: One Capability per IHE Actor</h2>

<p>The default. Each actor in a profile becomes its own Naftiko Capability. For XDS.b cross-enterprise document sharing, that means a <code class="language-plaintext highlighter-rouge">xds-document-source.naftiko.yml</code>, a <code class="language-plaintext highlighter-rouge">xds-document-repository.naftiko.yml</code>, a <code class="language-plaintext highlighter-rouge">xds-document-registry.naftiko.yml</code>, and a <code class="language-plaintext highlighter-rouge">xds-document-consumer.naftiko.yml</code>. Each is independently deployable and Connectathon-claimable.</p>

<div class="language-yaml highlighter-rouge"><div class="highlight"><pre class="highlight"><code><span class="na">naftiko</span><span class="pi">:</span> <span class="s2">"</span><span class="s">1.0.0-alpha1"</span>

<span class="na">info</span><span class="pi">:</span>
  <span class="na">label</span><span class="pi">:</span> <span class="s">IHE XDS Document Source</span>
  <span class="na">description</span><span class="pi">:</span> <span class="pi">&gt;-</span>
    <span class="s">IHE ITI Document Source actor (XDS.b). Submits a SubmissionSet of</span>
    <span class="s">clinical documents to a configured Document Repository in an XDS</span>
    <span class="s">Affinity Domain via transaction ITI-41. Emits an ATNA audit event</span>
    <span class="s">per ITI Technical Framework Volume 2 requirements.</span>
  <span class="na">ihe</span><span class="pi">:</span>
    <span class="na">profile</span><span class="pi">:</span> <span class="s">XDS.b</span>
    <span class="na">actor</span><span class="pi">:</span> <span class="s">Document Source</span>
    <span class="na">transactions</span><span class="pi">:</span> <span class="pi">[</span><span class="nv">ITI-41</span><span class="pi">]</span>
  <span class="na">tags</span><span class="pi">:</span> <span class="pi">[</span><span class="nv">ihe</span><span class="pi">,</span> <span class="nv">xds</span><span class="pi">,</span> <span class="nv">iti-41</span><span class="pi">,</span> <span class="nv">healthcare</span><span class="pi">]</span>

<span class="na">capability</span><span class="pi">:</span>
  <span class="na">consumes</span><span class="pi">:</span>
    <span class="pi">-</span> <span class="na">namespace</span><span class="pi">:</span> <span class="s">repo</span>
      <span class="na">type</span><span class="pi">:</span> <span class="s">http</span>
      <span class="na">baseUri</span><span class="pi">:</span> <span class="s2">"</span><span class="s">{{XDS_REPOSITORY_BASE_URI}}"</span>
      <span class="na">resources</span><span class="pi">:</span>
        <span class="pi">-</span> <span class="na">name</span><span class="pi">:</span> <span class="s">provide-and-register</span>
          <span class="na">path</span><span class="pi">:</span> <span class="s2">"</span><span class="s">/Repository/services/xdsrepositoryb"</span>
          <span class="na">operations</span><span class="pi">:</span>
            <span class="pi">-</span> <span class="na">name</span><span class="pi">:</span> <span class="s">submit-document-set</span>
              <span class="na">method</span><span class="pi">:</span> <span class="s">POST</span>

    <span class="pi">-</span> <span class="na">namespace</span><span class="pi">:</span> <span class="s">audit</span>
      <span class="na">type</span><span class="pi">:</span> <span class="s">capability</span>
      <span class="na">ref</span><span class="pi">:</span> <span class="s">ihe-atna-audit-record-repository</span>

  <span class="na">exposes</span><span class="pi">:</span>
    <span class="pi">-</span> <span class="na">type</span><span class="pi">:</span> <span class="s">mcp</span>
      <span class="na">port</span><span class="pi">:</span> <span class="m">3030</span>
      <span class="na">namespace</span><span class="pi">:</span> <span class="s">xds-document-source</span>
      <span class="na">tools</span><span class="pi">:</span>
        <span class="pi">-</span> <span class="na">name</span><span class="pi">:</span> <span class="s">submit-document-set</span>
          <span class="na">description</span><span class="pi">:</span> <span class="pi">&gt;-</span>
            <span class="s">Submit a SubmissionSet of clinical documents to the XDS</span>
            <span class="s">Affinity Domain Document Repository (transaction ITI-41).</span>
          <span class="na">inputParameters</span><span class="pi">:</span>
            <span class="pi">-</span> <span class="na">name</span><span class="pi">:</span> <span class="s">patientId</span>
              <span class="na">type</span><span class="pi">:</span> <span class="s">string</span>
              <span class="na">required</span><span class="pi">:</span> <span class="no">true</span>
              <span class="na">description</span><span class="pi">:</span> <span class="s2">"</span><span class="s">CX-formatted</span><span class="nv"> </span><span class="s">patient</span><span class="nv"> </span><span class="s">identifier</span><span class="nv"> </span><span class="s">with</span><span class="nv"> </span><span class="s">assigning</span><span class="nv"> </span><span class="s">authority</span><span class="nv"> </span><span class="s">OID"</span>
            <span class="pi">-</span> <span class="na">name</span><span class="pi">:</span> <span class="s">documents</span>
              <span class="na">type</span><span class="pi">:</span> <span class="s">array</span>
              <span class="na">required</span><span class="pi">:</span> <span class="no">true</span>
            <span class="pi">-</span> <span class="na">name</span><span class="pi">:</span> <span class="s">submissionSetMetadata</span>
              <span class="na">type</span><span class="pi">:</span> <span class="s">object</span>
              <span class="na">required</span><span class="pi">:</span> <span class="no">true</span>
          <span class="na">steps</span><span class="pi">:</span>
            <span class="pi">-</span> <span class="na">name</span><span class="pi">:</span> <span class="s">submit</span>
              <span class="na">type</span><span class="pi">:</span> <span class="s">call</span>
              <span class="na">call</span><span class="pi">:</span> <span class="s">repo.submit-document-set</span>
            <span class="pi">-</span> <span class="na">name</span><span class="pi">:</span> <span class="s">audit</span>
              <span class="na">type</span><span class="pi">:</span> <span class="s">call</span>
              <span class="na">call</span><span class="pi">:</span> <span class="s">audit.record-event</span>
</code></pre></div></div>

<p>Two things to notice. First, the <code class="language-plaintext highlighter-rouge">info.ihe</code> block carries the profile, actor, and transaction codes – structured metadata that a Connectathon test harness or registry can introspect. Second, the capability composes another Naftiko capability – the audit record repository – via the second <code class="language-plaintext highlighter-rouge">consumes</code> entry. That is Pattern 2.</p>

<h2 id="pattern-2-cross-cutting-capabilities-for-the-ihe-substrate">Pattern 2: Cross-Cutting Capabilities for the IHE Substrate</h2>

<p>ATNA, CT, and XUA are not workflow profiles. They are substrate. Build them once and depend on them everywhere.</p>

<div class="language-yaml highlighter-rouge"><div class="highlight"><pre class="highlight"><code><span class="na">naftiko</span><span class="pi">:</span> <span class="s2">"</span><span class="s">1.0.0-alpha1"</span>

<span class="na">info</span><span class="pi">:</span>
  <span class="na">label</span><span class="pi">:</span> <span class="s">IHE ATNA Audit Record Repository</span>
  <span class="na">description</span><span class="pi">:</span> <span class="pi">&gt;-</span>
    <span class="s">IHE Audit Record Repository actor. Receives ATNA-conformant audit</span>
    <span class="s">events emitted by other IHE actor capabilities and persists them</span>
    <span class="s">for compliance and forensic review.</span>
  <span class="na">ihe</span><span class="pi">:</span>
    <span class="na">profile</span><span class="pi">:</span> <span class="s">ATNA</span>
    <span class="na">actor</span><span class="pi">:</span> <span class="s">Audit Record Repository</span>
    <span class="na">transactions</span><span class="pi">:</span> <span class="pi">[</span><span class="nv">ITI-20</span><span class="pi">]</span>
  <span class="na">tags</span><span class="pi">:</span> <span class="pi">[</span><span class="nv">ihe</span><span class="pi">,</span> <span class="nv">atna</span><span class="pi">,</span> <span class="nv">audit</span><span class="pi">,</span> <span class="nv">security</span><span class="pi">]</span>
</code></pre></div></div>

<p>Every other IHE-profile-implementing capability adds <code class="language-plaintext highlighter-rouge">audit</code> to its <code class="language-plaintext highlighter-rouge">consumes</code> list. The fleet now has a single auditable trace of every cross-enterprise transaction, and conformance to ATNA’s audit requirements is composed in rather than re-implemented per profile.</p>

<p>The same pattern applies to a <code class="language-plaintext highlighter-rouge">consistent-time.naftiko.yml</code> for CT and an <code class="language-plaintext highlighter-rouge">xua-assertion.naftiko.yml</code> for cross-enterprise user identity.</p>

<h2 id="pattern-3-fhir-native-profiles-are-the-cleanest-fit">Pattern 3: FHIR-Native Profiles Are the Cleanest Fit</h2>

<p>The newer IHE profiles – the ones the European Health Data Space and US TEFCA initiatives are building on – are FHIR-based. Patient Identity Cross-Reference for Mobile (PIXm, transaction ITI-83) is a one-page capability:</p>

<div class="language-yaml highlighter-rouge"><div class="highlight"><pre class="highlight"><code><span class="na">naftiko</span><span class="pi">:</span> <span class="s2">"</span><span class="s">1.0.0-alpha1"</span>

<span class="na">info</span><span class="pi">:</span>
  <span class="na">label</span><span class="pi">:</span> <span class="s">IHE PIXm Patient Identifier Cross-Reference Consumer</span>
  <span class="na">description</span><span class="pi">:</span> <span class="pi">&gt;-</span>
    <span class="s">Resolves a patient's identifiers across systems within an Affinity</span>
    <span class="s">Domain using IHE PIX-on-FHIR (ITI-83). Built on FHIR R4 Patient</span>
    <span class="s">$ihe-pix operation.</span>
  <span class="na">ihe</span><span class="pi">:</span>
    <span class="na">profile</span><span class="pi">:</span> <span class="s">PIXm</span>
    <span class="na">actor</span><span class="pi">:</span> <span class="s">Patient Identifier Cross-Reference Consumer</span>
    <span class="na">transactions</span><span class="pi">:</span> <span class="pi">[</span><span class="nv">ITI-83</span><span class="pi">]</span>
  <span class="na">tags</span><span class="pi">:</span> <span class="pi">[</span><span class="nv">ihe</span><span class="pi">,</span> <span class="nv">pixm</span><span class="pi">,</span> <span class="nv">fhir</span><span class="pi">,</span> <span class="nv">iti-83</span><span class="pi">]</span>

<span class="na">capability</span><span class="pi">:</span>
  <span class="na">consumes</span><span class="pi">:</span>
    <span class="pi">-</span> <span class="na">namespace</span><span class="pi">:</span> <span class="s">pix</span>
      <span class="na">type</span><span class="pi">:</span> <span class="s">http</span>
      <span class="na">baseUri</span><span class="pi">:</span> <span class="s2">"</span><span class="s">{{PIX_MANAGER_BASE_URI}}/fhir"</span>
      <span class="na">resources</span><span class="pi">:</span>
        <span class="pi">-</span> <span class="na">name</span><span class="pi">:</span> <span class="s">patient-identifier-xref</span>
          <span class="na">path</span><span class="pi">:</span> <span class="s2">"</span><span class="s">/Patient/$ihe-pix"</span>
          <span class="na">operations</span><span class="pi">:</span>
            <span class="pi">-</span> <span class="na">name</span><span class="pi">:</span> <span class="s">cross-reference</span>
              <span class="na">method</span><span class="pi">:</span> <span class="s">GET</span>

  <span class="na">exposes</span><span class="pi">:</span>
    <span class="pi">-</span> <span class="na">type</span><span class="pi">:</span> <span class="s">mcp</span>
      <span class="na">port</span><span class="pi">:</span> <span class="m">3031</span>
      <span class="na">namespace</span><span class="pi">:</span> <span class="s">pixm-consumer</span>
      <span class="na">tools</span><span class="pi">:</span>
        <span class="pi">-</span> <span class="na">name</span><span class="pi">:</span> <span class="s">cross-reference-patient-id</span>
          <span class="na">description</span><span class="pi">:</span> <span class="pi">&gt;-</span>
            <span class="s">Resolve a patient's identifiers across systems within an</span>
            <span class="s">Affinity Domain using IHE PIX-on-FHIR.</span>
          <span class="na">inputParameters</span><span class="pi">:</span>
            <span class="pi">-</span> <span class="na">name</span><span class="pi">:</span> <span class="s">sourceIdentifier</span>
              <span class="na">type</span><span class="pi">:</span> <span class="s">string</span>
              <span class="na">required</span><span class="pi">:</span> <span class="no">true</span>
              <span class="na">description</span><span class="pi">:</span> <span class="s2">"</span><span class="s">Token</span><span class="nv"> </span><span class="s">search</span><span class="nv"> </span><span class="s">param:</span><span class="nv"> </span><span class="s">system|value"</span>
            <span class="pi">-</span> <span class="na">name</span><span class="pi">:</span> <span class="s">targetSystem</span>
              <span class="na">type</span><span class="pi">:</span> <span class="s">string</span>
          <span class="na">call</span><span class="pi">:</span> <span class="s">pix.cross-reference</span>
</code></pre></div></div>

<p>Because FHIR transactions are already RESTful, the consumes-exposes mapping is one-to-one. PIXm, PDQm, MHD, and IUA all collapse into capabilities of similar size. This is why the FHIR-based IHE profile track at the recent IHE-Europe Connectathon in Brussels is the fastest-growing one.</p>

<h2 id="pattern-4-profile-bundle-as-a-workflow-tool">Pattern 4: Profile Bundle as a Workflow Tool</h2>

<p>For consumers who do not want to wire up four actor capabilities, ship a meta-capability – <code class="language-plaintext highlighter-rouge">xds-affinity-domain-workflow.naftiko.yml</code> – that internally consumes the actor capabilities and exposes a single high-level tool: “share clinical document across affinity domain.” The underlying actors remain the unit of conformance. The bundle is sugar for AI agents that prefer one verb over five.</p>

<p>This is the most AI-friendly shape. An LLM agent can call <code class="language-plaintext highlighter-rouge">share-clinical-document</code> once and the capability orchestrates the IHE-conformant transactions underneath – including the audit emission and identity assertion that a hand-written agent would forget.</p>

<h2 id="pattern-5-auto-generating-capabilities-from-ihe-implementation-guides">Pattern 5: Auto-Generating Capabilities from IHE Implementation Guides</h2>

<p>This is the force-multiplier. IHE publishes its FHIR-based profiles as FHIR Implementation Guides – bundles of <code class="language-plaintext highlighter-rouge">StructureDefinition</code>, <code class="language-plaintext highlighter-rouge">CapabilityStatement</code>, and <code class="language-plaintext highlighter-rouge">OperationDefinition</code> resources. That is the same structured input a Naftiko capability YAML wants to consume.</p>

<p>A Naftiko skill could read an IHE FHIR IG, walk every Actor declared in its CapabilityStatement, and emit one capability YAML per actor with consumes, exposes, parameter schemas, and the <code class="language-plaintext highlighter-rouge">info.ihe</code> metadata block pre-populated. IHE has hundreds of profiles. This is the kind of artifact that one engineer can drive but that no one engineer can hand-write.</p>

<h2 id="pattern-6-connectathon-validated-capabilities">Pattern 6: Connectathon-Validated Capabilities</h2>

<p>A Naftiko capability deployed via the fleet has a network surface. It implements transactions. It can be peer-tested at an IHE Connectathon – the multi-day events where vendors prove interoperability against each other in real time. The North America Connectathon Week is in Arlington, Virginia, in October 2026; the IHE-Europe Connectathon returns to Brussels in spring 2027.</p>

<p>A capability registered as a system-under-test in <a href="https://gazelle.ihe.net/">Gazelle</a>, with its <code class="language-plaintext highlighter-rouge">info.ihe</code> metadata declaring the actor and transactions claimed, becomes Connectathon-eligible. Pass results post on <a href="https://connectathon-results.ihe.net/">connectathon-results.ihe.net</a> – the same enterprise credibility signal that Epic, Cerner, and InterSystems lean on.</p>

<h2 id="pattern-7-ai-agents-calling-ihe-profiles-via-mcp">Pattern 7: AI Agents Calling IHE Profiles via MCP</h2>

<p>This is the pattern that no other IHE actor implementation in the market offers. Every existing implementation – Epic, IBM, InterSystems, b·well, Forcare – exposes profile transactions over HL7 v2, SOAP, or FHIR. None of them expose those transactions as MCP tools.</p>

<p>Naftiko’s <code class="language-plaintext highlighter-rouge">exposes: type: mcp</code> block changes that. A profile-implementing capability automatically becomes an AI-callable tool. An LLM agent can:</p>

<ul>
  <li>Cross-reference a patient identifier (PIXm, ITI-83) before calling a downstream FHIR API.</li>
  <li>Fetch a clinical document set (XDS-I or MHD) as context for a clinical reasoning task.</li>
  <li>Submit a structured audit event (ATNA, ITI-20) when an AI agent acts on a patient’s record.</li>
</ul>

<p>That is the most concrete way to deliver on what the IHE community is increasingly saying out loud: leveraging standards in healthcare will make AI solutions better. Profiles are how standards become usable. Naftiko Capabilities are how profiles become AI-callable.</p>

<h2 id="what-this-means-for-european-and-us-healthcare-programs">What This Means for European and US Healthcare Programs</h2>

<p>These patterns map to the regulatory weather healthcare is operating in right now. The European Health Data Space (EHDS) regulation is in force, with mandatory cross-border patient summary and ePrescription exchange coming online in 2029, and medical images, lab results, and discharge reports following in 2031. Every EU EHR vendor has to certify. Every cross-border exchange needs a working profile implementation.</p>

<p>IHE-Europe runs the practical testing rails for that work, and IHE recently launched the <a href="https://www.ihe.net/prism/">PRISM</a> accelerator – a program that takes a market need (the first cohort is EHDS), runs it through an accelerated profiling cycle, and produces a Connectathon-validated specification. The pattern is the same in the US, where the <a href="https://www.iheusa.org/">North America Connectathon</a> anchors profile testing for TEFCA-aligned exchange.</p>

<p>A Naftiko Capability is not a replacement for this work. It is the AI-callable, governed, declaratively-versioned delivery vehicle for it.</p>

<h2 id="looking-for-healthcare-design-partners">Looking for Healthcare Design Partners</h2>

<p>We are actively looking for healthcare design partners who want to modernize and integrate their existing patient data securely and confidently into AI workflows – and who care about doing it in a way that holds up against IHE profiles, EHDS certification, and cross-border exchange requirements.</p>

<p>The <a href="https://github.com/naftiko/fleet">Naftiko Framework</a> and the Capabilities specification are released under the Apache 2.0 license. If you are working with HL7 v2, FHIR, DICOM, OpenEHR, or any combination of these standards – and you want to explore how governed, AI-callable capabilities can carry that work forward into IHE-conformant integration profiles – we want to hear from you.</p>
