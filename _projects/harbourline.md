---
title: Harbourline Security
subtitle: A Salesforce CRM built around real-world business problems
summary: A self-directed Salesforce practice project bringing sales approvals, customer-site data, service cases and admin requests into one tested CRM solution for a fictional security business.
status: completed
status_label: Self-directed practice org · 2026
featured: true
order: 1
card_image: /assets/img/harbourline/flow.webp
card_alt: Harbourline Salesforce record-triggered Flow showing decisions and record creation
card_date: August 2026
card_date_label: Started
card_description: Salesforce CRM practice build connecting sales approvals, service cases, customer sites and admin requests.
tags: [Salesforce Admin, Sales & Service, Flow, Security, Data Quality, Testing]
role: Salesforce Administrator — requirements, configuration, testing and documentation
org: Salesforce Developer Edition · fictional business and test data
period: August 2026 – present
tools: Flow Builder, Schema Builder, Lightning App Builder, Data Loader, dataloader.io
showcase_images:
  - image: /assets/img/harbourline/flow.webp
    alt: Create HQ Building Flow showing Get Records, a decision, and Create Records in Salesforce Flow Builder
    title: Designing for the exception
    caption: Check whether a Headquarters Building exists before creating one
    anchor: "#decisions"
  - image: /assets/img/harbourline/validation.webp
    alt: Salesforce validation error preventing an unapproved discount from advancing to Proposal
    title: A control that actually stops the record
    caption: Unapproved discounts cannot move into price stages
    anchor: "#solutions"
  - image: /assets/img/harbourline/kanban.webp
    alt: Harbourline Admin Request tracker Kanban board grouped by status
    title: Turning requests into a visible queue
    caption: Admin work tracked by priority and status
    anchor: "#solutions"
links:
  - label: Explore technical evidence
    url: /docs/harbourline/
  - label: Download full portfolio PDF
    url: /docs/harbourline/Serap_Ozturk-Kucuk_Salesforce_Admin_Portfolio_v1.1.pdf
stats:
  - value: "28"
    label: customer Accounts
  - value: "65"
    label: customer sites
  - value: "40"
    label: Opportunities migrated
  - value: "4"
    label: record-triggered Flows
contents:
  - id: challenge
    title: The challenge
  - id: solutions
    title: Solution overview
  - id: decisions
    title: Design decisions
  - id: testing
    title: Testing and fixes
  - id: outcome
    title: Results and next steps
---

<p class="case-kicker">The project in brief</p>
<p class="case-opening">The aim was to turn business requirements into working Salesforce processes, then test both the expected outcomes and the exceptions. The project documents not only what was configured, but why each approach was chosen and where problems were found.</p>
<p class="case-context">Harbourline Security represents a fictional CCTV and access-control business in North West England. The sales, service and admin processes were configured and tested in Salesforce Developer Edition using fictional users and test data. The figures above describe the practice dataset, not live business results.</p>

<h2 id="challenge">01 / The challenge</h2>

<p>Four problems shaped the build: discounts were agreed by email, support messages had no reliable ownership, information about customer sites was scattered, and requests to the Salesforce admin had no shared priority or status. Sales managers also needed visibility without giving every rep access to every deal.</p>

<h2 id="solutions">02 / Solution overview</h2>

<div class="solution-list">
  <div class="solution-item">
    <span class="solution-index">01</span>
    <div><h3>Sales approvals with a clear stopping point</h3><p>Two Opportunity sales processes support different types of deals. Discounts above 15% require manager approval; those above 40% require a second approval step. A validation rule prevents a deal with an unapproved discount from progressing into price stages.</p></div>
  </div>
  <div class="solution-item">
    <span class="solution-index">02</span>
    <div><h3>Support cases routed to the right place</h3><p>Email-to-Case creates cases from incoming messages. A Support Queue, assignment and auto-response rules establish ownership and acknowledgement; an escalation rule addresses cases after four hours. A Flow alerts the service manager to cases from Key Accounts.</p></div>
  </div>
  <div class="solution-item">
    <span class="solution-index">03</span>
    <div><h3>Customer data that connects to the real world</h3><p>A custom Building object links sites to Accounts, with a lookup to the site Contact. External IDs support matching during imports, and reports and dashboards help sales, service and admin teams see their work.</p></div>
  </div>
  <div class="solution-item">
    <span class="solution-index">04</span>
    <div><h3>Admin requests people can track</h3><p>A custom Admin Request object provides a Kanban view, P1–P4 priority based on Impact and Urgency, status validation and notification Flows. The result is a visible backlog instead of informal requests.</p></div>
  </div>
</div>

<h2 id="decisions">03 / Decisions behind the build</h2>

<p>Declarative options were compared before adding automation. Formulas and roll-ups calculate values, validation rules enforce requirements, approval processes manage sign-off, and record-triggered Flows handle actions that need additional logic.</p>

<div class="case-decision">
  <h3>Why Building is a child of Account</h3>
  <p>A Building record depends on its customer, so the relationship to Account uses master-detail. This also allows the site count to roll up to Account. Site Contact uses a lookup because a building does not always have a named contact.</p>
  <figure class="shot">
    <img src="{{ '/assets/img/harbourline/schema.webp' | relative_url }}" alt="Salesforce Schema Builder showing Account, Building, Contact and Training Session object relationships" loading="lazy" width="1000" height="650">
    <figcaption>Actual Schema Builder view from the practice org, including the Building relationship and Session Attendee junction.</figcaption>
  </figure>
</div>

<div class="case-decision">
  <h3>Why the HQ Building Flow checks before creating</h3>
  <p>When an Opportunity is won, an after-save Flow checks for an existing Headquarters Building before creating a new record. Testing covered both the creation path and the existing-building path to check that duplicates were not created.</p>
</div>

<div class="case-decision">
  <h3>Why sharing rules were not enough on their own</h3>
  <p>Opportunities are Private, with access opened through the role hierarchy and specific sharing. During testing, one user still saw no deals because her profile lacked Read permission on Opportunity. Fixing the object permission made the distinction between object access and record sharing very clear.</p>
</div>

<h2 id="testing">04 / Testing and troubleshooting</h2>

<p>Tests covered both expected behaviour and cases where a feature should not act or should block a change. With two full Developer Edition licences, the spare licence was reassigned between test users, with access checked through Login As.</p>

<div class="test-grid">
  <div><strong>100 cases</strong><span>Bulk test of the Case alert Flow, including records that should not trigger alerts</span></div>
  <div><strong>4-row trials</strong><span>Small imports before larger loads, followed by record-count checks</span></div>
  <div><strong>Real email</strong><span>Email-to-Case tested end to end on the primary incoming-email path</span></div>
  <div><strong>Multiple roles</strong><span>Record visibility checked with Login As and role-based access tests</span></div>
</div>

<p><strong>Two instructive fixes:</strong></p>
<ul>
  <li><strong>No Opportunity visibility despite sharing:</strong> the user lacked object-level Read permission. The permission was corrected and access retested.</li>
  <li><strong>Approval failed because Manager was empty:</strong> the submitting user had no Manager assigned. Manager assignment became part of the new-user setup checklist.</li>
</ul>

<p class="case-footnote">The detailed documentation includes other troubleshooting cases, data import decisions, security tables and test scenarios.</p>

<h2 id="outcome">05 / Results, limitations and next steps</h2>

<p>The finished practice org includes tested sales approvals, customer service processes, customer-site records, access controls and an Admin Request tracker, supported by 13 reports across three dashboards. The documentation records the configuration, failed tests, causes and fixes.</p>

<p><strong>What this does not prove:</strong> Harbourline is a practice project, not a live deployment. Real customer outcomes, such as reduced approval time or faster case resolution, have not been measured. Email-to-Case testing did not yet cover unknown senders or reply threading. Web-to-Case, entitlements and sandbox deployment were not implemented.</p>

<p>Potential next steps include deployment between orgs, Screen Flows and Permission Set Groups, with the same emphasis on design choices and testing.</p>

<div class="case-next-step">
  <div><span class="label">Want the technical detail?</span><h3>See the diagrams, field decisions and test evidence</h3><p>The complete documentation keeps the deeper configuration steps without slowing down this case study.</p></div>
  <div class="case-next-actions">
    <a class="btn primary" href="{{ '/docs/harbourline/' | relative_url }}">Read technical documentation</a>
    <a class="btn" href="{{ '/docs/harbourline/Serap_Ozturk-Kucuk_Salesforce_Admin_Portfolio_v1.1.pdf' | relative_url }}">Download full PDF</a>
  </div>
</div>
