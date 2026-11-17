---
title: Harbourline Security
subtitle: Sales, Service and Access Controls in a Salesforce Practice Org
summary: A Salesforce Developer Edition project covering discount approvals, customer-site records, service cases and admin requests for a security business.
status: completed
status_label: Self-Directed Practice Org · 2026
featured: true
order: 1
card_image: /assets/img/harbourline/flow.webp
card_alt: Harbourline Salesforce record-triggered Flow showing decisions and record creation
card_date: August 2026
card_date_label: Started
card_description: A tested Salesforce practice org for sales approvals, case management, customer sites and admin requests.
tags: [Salesforce Admin, Sales & Service, Flow, Security, Data Quality, Testing]
role: "Salesforce Administrator: requirements, configuration, testing and documentation"
org: Salesforce Developer Edition · practice org and test data
period: August 2026 – present
tools: Flow Builder, Schema Builder, Lightning App Builder, Data Loader, dataloader.io
links:
  - label: Technical Documentation
    url: /docs/harbourline/
  - label: Download PDF
    url: /docs/harbourline/Serap_Ozturk-Kucuk_Salesforce_Admin_Portfolio_v1.1.pdf
stats:
  - value: "28"
    label: Accounts
  - value: "65"
    label: Customer Sites
  - value: "40"
    label: Opportunities
  - value: "4"
    label: Record-Triggered Flows
---

<p class="case-opening">Salesforce configuration addresses controlled discounts, support case ownership, customer-site records, admin requests and access to reporting. Testing covers expected behaviour, exceptions and permissions.</p>
<p class="case-context">Harbourline Security is a fictional CCTV and access-control business in North West England. The Salesforce Developer Edition org uses test users and records. The figures describe practice data, not live business results.</p>

<!-- PROJECT SECTION -->

<p>Discounts were agreed by email, support messages lacked clear ownership, customer-site records were scattered, and admin requests had no shared priority or status. Sales managers also needed visibility without giving every sales rep access to every Opportunity.</p>

<!-- PROJECT SECTION -->

<div class="solution-list">
  <div class="solution-item">
    <h3>Sales Approvals With a Clear Stopping Point</h3>
    <p>Two Opportunity sales processes support different types of deals. Discounts above 15% require manager approval; discounts above 40% require a second approval step. A validation rule blocks unapproved discounts from progressing into price stages.</p>
    {% include project-showcase.html src="/assets/img/harbourline/validation.webp" width="1500" height="204" loading="eager" alt="Salesforce validation rule error preventing an unapproved Opportunity discount" caption="Validation rule blocks a discount until the required approval is complete." %}
  </div>
  <div class="solution-item">
    <h3>Support Cases Routed to the Right Place</h3>
    <p>Email-to-Case creates cases from incoming messages. A Support Queue, assignment rules and auto-response rules establish ownership and acknowledgement; an escalation rule notifies the service manager about cases still open after four hours. A record-triggered Flow alerts the service manager about Key Account cases.</p>
    {% include project-showcase.html src="/assets/img/harbourline/service.webp" width="1500" height="712" alt="Harbourline Service Home with Support Queue and open Cases" caption="Service Home shows the Support Queue and the open case workload." %}
  </div>
  <div class="solution-item">
    <h3>Linking Accounts to Customer Sites</h3>
    <p>A custom Building object links sites to Accounts, with an optional lookup to a site Contact. External IDs support upsert during data migration.</p>
  </div>
  <div class="solution-item">
    <h3>Admin Requests People Can Track</h3>
    <p>A custom Admin Request object provides a Kanban view, P1–P4 priority based on Impact and Urgency, status validation and notification Flows. A shared backlog replaces informal requests.</p>
    {% include project-showcase.html src="/assets/img/harbourline/kanban.webp" width="1500" height="260" alt="Admin Request Kanban board grouped by status and priority" caption="Kanban view tracks the status and priority of admin requests." %}
  </div>
  <div class="solution-item">
    <h3>Access and Reporting</h3>
    <p>Private Opportunities opened through the role hierarchy, a sharing rule and an account team, with 13 reports on three dashboards.</p>
  </div>
</div>

<!-- PROJECT SECTION -->

<p>Declarative options were compared before adding automation. Formulas and roll-ups calculate values, validation rules enforce requirements, approval processes manage sign-off, and record-triggered Flows handle actions requiring additional logic.</p>

<div class="case-decision">
  <h3>Why Building Is a Child of Account</h3>
  <p>The Building-to-Account relationship uses master-detail, allowing site counts to roll up to Account. Site Contact uses a lookup because a site may have no named contact.</p>
  {% include project-showcase.html src="/assets/img/harbourline/schema.webp" width="895" height="817" alt="Schema Builder showing Account, Building, Contact and Training Session relationships" caption="Schema Builder shows Account, Building and the Training Session junction." %}
</div>

<div class="case-decision">
  <h3>Why the HQ Building Flow Checks Before Creating</h3>
  <p>An after-save Flow checks for an existing Headquarters Building when an Opportunity is won. Tests covered both the creation path and the existing-building path to prevent duplicate records.</p>
  {% include project-showcase.html src="/assets/img/harbourline/flow.webp" width="1400" height="1249" alt="Record-triggered Create HQ Building Flow with Get Records, Decision and Create Records" caption="The Flow checks for an existing Headquarters Building before creating a new record." %}
</div>

<div class="case-decision">
  <h3>Why Sharing Rules Were Not Enough on Their Own</h3>
  <p>Opportunities are Private, with access opened through the role hierarchy and specific sharing. One test user could not view Opportunities because the profile lacked object-level Read permission. Lesson: sharing opens records, but the profile must first give access to the object.</p>
</div>

<!-- PROJECT SECTION -->

<p>Tests covered actions that should succeed, fail or leave records unchanged. With two user licences, test users were activated in turn, and Login As was used to check each user’s access and both discount approval steps.</p>

<div class="test-grid">
  <div><strong>100 Cases</strong><span>Bulk-tested the Case alert Flow, including records that should not trigger alerts</span></div>
  <div><strong>500 Lead Scans</strong><span>495 imported, five duplicates not imported; small four-row trials preceded larger loads</span></div>
  <div><strong>Real Email</strong><span>Email-to-Case tested end to end on the primary incoming-email path</span></div>
  <div><strong>Approval Levels</strong><span>Manager and Sales VP steps tested by activating users in turn</span></div>
</div>

<h3>Key Fixes</h3>
<ul>
  <li><strong>Missing Opportunity visibility:</strong> Object-level Read permission was added, then record access was retested.</li>
  <li><strong>Missing approval manager:</strong> The submitter's Manager field was empty. Manager assignment was added to the user setup checklist.</li>
</ul>

<!-- PROJECT SECTION -->

<p>The practice org includes discount approvals, Email-to-Case, customer-site relationships, access controls and an Admin Request tracker, with 13 reports and three dashboards. Configuration decisions, test outcomes and troubleshooting fixes are documented.</p>
{% include project-showcase.html src="/assets/img/harbourline/sales.webp" width="785" height="896" alt="Harbourline Sales dashboard showing discount approvals, pipeline by stage and won revenue by customer tier" caption="Sales dashboard showing pending discount approvals, pipeline stages and customer tiers." %}

<p><strong>Project Scope:</strong> No live customer outcomes were measured. Email-to-Case testing did not cover unknown senders or reply threading; Web-to-Case, entitlements and sandbox deployment were not implemented.</p>
<p><strong>Next Steps:</strong> Deployment between orgs with GitHub version control, a fuller service process (entitlements, Web-to-Case, Omni-Channel), and an Agentforce agent.</p>
