---
title: Harbourline Security
subtitle: Sales, service and access controls in a Salesforce practice org
summary: A Salesforce Developer Edition project covering discount approvals, customer-site records, service cases and admin requests for a fictional security business.
status: completed
status_label: Self-directed practice org · 2026
featured: true
order: 1
card_image: /assets/img/harbourline/flow.webp
card_alt: Harbourline Salesforce record-triggered Flow showing decisions and record creation
card_date: August 2026
card_date_label: Started
card_description: A tested Salesforce practice org for sales approvals, case management, customer sites and admin requests.
tags: [Salesforce Admin, Sales & Service, Flow, Security, Data Quality, Testing]
role: Salesforce Administrator — requirements, configuration, testing and documentation
org: Salesforce Developer Edition · fictional business and test data
period: August 2026 – present
tools: Flow Builder, Schema Builder, Lightning App Builder, Data Loader, dataloader.io
showcase_images:
  - image: /assets/img/harbourline/flow.webp
    alt: Create HQ Building Flow showing Get Records, a decision, and Create Records in Salesforce Flow Builder
    title: Preventing duplicate site records
    caption: Check whether a Headquarters Building exists before creating one
    anchor: "#decisions"
  - image: /assets/img/harbourline/validation.webp
    alt: Salesforce validation error preventing an unapproved discount from advancing to Proposal
    title: Blocking unapproved discounts
    caption: Unapproved discounts cannot move into price stages
    anchor: "#solutions"
  - image: /assets/img/harbourline/kanban.webp
    alt: Harbourline Admin Request tracker Kanban board grouped by status
    title: Tracking admin requests
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
<p class="case-opening">The project applies Salesforce configuration to four business needs: controlled discounts, owned support cases, connected customer-site records and a trackable admin request backlog. Testing covered expected behaviour, exceptions and access restrictions.</p>
<p class="case-context">Harbourline Security is a fictional CCTV and access-control business in North West England. The org was built in Salesforce Developer Edition using fictional users and records. All figures describe practice data and testing, not live business results.</p>

{% include project-showcase.html %}

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
    <div><h3>Linking Accounts to customer sites</h3><p>A custom Building object links each site to an Account, with an optional lookup to the site Contact. External IDs support upsert during migration. Of 500 trade-show lead scans imported, 495 were created and five duplicates were not imported.</p></div>
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

<p>Tests covered actions that should succeed, fail or leave records unchanged. Testing was carried out with two active user licences available. Test users were activated and deactivated in turn to check role-based access and the manager and Sales VP discount approval steps. Login As was also used to investigate record visibility.</p>

<div class="test-grid">
  <div><strong>100 cases</strong><span>Bulk test of the Case alert Flow, including records that should not trigger alerts</span></div>
  <div><strong>500 lead scans</strong><span>495 imported and five duplicates not imported; smaller four-row trials were used before larger data loads</span></div>
  <div><strong>Real email</strong><span>Email-to-Case tested end to end on the primary incoming-email path</span></div>
  <div><strong>Approval levels</strong><span>Manager and Sales VP steps tested using users activated and deactivated in turn</span></div>
</div>

<p><strong>Two instructive fixes:</strong></p>
<ul>
  <li><strong>No Opportunity visibility despite sharing:</strong> the user lacked object-level Read permission. The permission was corrected and access retested.</li>
  <li><strong>Approval failed because Manager was empty:</strong> the submitting user had no Manager assigned. Manager assignment became part of the new-user setup checklist.</li>
</ul>

<p class="case-footnote">The detailed documentation includes other troubleshooting cases, data import decisions, security tables and test scenarios.</p>

<h2 id="outcome">05 / Results, limitations and next steps</h2>

<p>The practice org includes sales approval controls, Email-to-Case, customer-site relationships, security settings and an Admin Request tracker, supported by 13 reports and three dashboards. Test records, configuration decisions and troubleshooting fixes are documented.</p>

<p><strong>Project scope:</strong> This is a fictional, non-production org. No live customer outcomes were measured. Email-to-Case testing did not cover unknown senders or reply threading; Web-to-Case, entitlements and sandbox deployment were not implemented.</p>

<p>Potential next steps include deployment between orgs, Screen Flows and Permission Set Groups, with the same emphasis on design choices and testing.</p>

<div class="case-next-step">
  <div><span class="label">Technical documentation</span><h3>Configuration, diagrams and test evidence</h3><p>The supporting documentation includes field decisions, security settings, test cases and troubleshooting notes.</p></div>
  <div class="case-next-actions">
    <a class="btn primary" href="{{ '/docs/harbourline/' | relative_url }}">Read technical documentation</a>
    <a class="btn" href="{{ '/docs/harbourline/Serap_Ozturk-Kucuk_Salesforce_Admin_Portfolio_v1.1.pdf' | relative_url }}">Download full PDF</a>
  </div>
</div>
