---
title: Harbourline Security
subtitle: Sales, Service and Access Controls in a Salesforce Practice Org
summary: A Trailhead Playground (Developer Edition) project covering discount approvals, customer-site records, service cases and admin requests for a security business.
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
org: Trailhead Playground (Developer Edition) · practice org and test data
period: August 2026 – present
tools: Flow Builder, Schema Builder, Lightning App Builder, Data Import Wizard, dataloader.io
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
  - value: "11"
    label: Automations & Rules
---

<p class="case-context">Harbourline Security is a fictional CCTV and access-control business in North West England. The Trailhead Playground (Developer Edition) org uses test users and records. The figures describe practice data, not live business results.</p>
<p class="case-opening">Salesforce configuration addresses controlled discounts, support case ownership, customer-site records, admin requests and access to reporting. Testing covers expected behaviour, exceptions and permissions.</p>

<!-- PROJECT SECTION -->

<p>Discounts were agreed by email, so margin was given away with no approval or record. Support messages lacked clear ownership, so slow or missed replies put customer relationships at risk. Customer-site records were scattered, and engineers lost time searching for the right information. Admin requests had no shared priority or status, so effort went to whoever asked loudest. Sales managers also needed visibility without giving every sales rep access to every Opportunity.</p>

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
    <p>A custom Building object links each site to its Account through master-detail, so the number of sites rolls up to the Account. An optional lookup links a site Contact, and External IDs support upsert during data migration.</p>
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

<p>Three choices shaped the build: how customer sites relate to Accounts, how the HQ Building Flow avoids duplicate records, and how discount approvals are enforced rather than left optional. Each choice was made after comparing the alternatives.</p>

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
  <h3>Why the Approval Process Needs a Validation Rule</h3>
  <p>An approval process on its own is optional: a sales rep can simply not submit. A validation rule blocks the price stages until the discount is approved, so the approval cannot be skipped.</p>
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
<div class="case-fixes">
  <div class="case-fix">
    <h3>No Opportunity Access Despite a Sharing Rule</h3>
    <p>The service manager could not see any Opportunities, although a sharing rule shared won deals with her role. Her profile had no Read permission on Opportunity. Read access was added and the test repeated.</p>
    <p class="case-lesson"><strong>Lesson:</strong> sharing opens records, but the profile must first give access to the object.</p>
  </div>
  <div class="case-fix">
    <h3>Approval Failed: Manager Field Empty</h3>
    <p>The first discount submission failed because the submitter had no Manager on the user record. Managers were set for every user.</p>
    <p class="case-lesson"><strong>Lesson:</strong> check every approver path before activating an approval process.</p>
  </div>
  <div class="case-fix">
    <h3>Lead Conversion Blocked</h3>
    <p>A required Legacy ID field stopped every Lead from converting. Legacy IDs became optional, and required fields were set on the page layout instead.</p>
    <p class="case-lesson"><strong>Lesson:</strong> fields added for a data migration must not block records created after go-live.</p>
  </div>
  <div class="case-fix">
    <h3>Duplicate Accounts After Import</h3>
    <p>The wrong import option created a copy of each Account. Records were cleaned up, and the data was reloaded with matching on email after a 4-row test.</p>
    <p class="case-lesson"><strong>Lesson:</strong> test every import on a small file and check the results before the full load.</p>
  </div>
</div>

<!-- PROJECT SECTION -->

<p>The practice org includes discount approvals, Email-to-Case, customer-site relationships, access controls and an Admin Request tracker, with 13 reports and three dashboards. Configuration decisions, test outcomes and troubleshooting fixes are documented.</p>
{% include project-showcase.html src="/assets/img/harbourline/sales.webp" width="785" height="896" alt="Harbourline Sales dashboard showing discount approvals, pipeline by stage and won revenue by customer tier" caption="Sales dashboard showing pending discount approvals, pipeline stages and customer tiers." %}

<p><strong>Limitations:</strong> No live customer outcomes were measured. Email-to-Case testing did not cover unknown senders or reply threading; Web-to-Case, entitlements and sandbox deployment were not implemented.</p>
<p><strong>Next Steps:</strong> deploying changes between two orgs as a sandbox-to-production release, a fuller service process (entitlements, Web-to-Case, Omni-Channel), and an Agentforce agent.</p>
