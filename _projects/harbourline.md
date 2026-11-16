---
title: Harbourline Security
subtitle: Salesforce CRM implementation in a practice org
summary: A self-directed Salesforce Developer Edition practice org for a fictional company that sells, installs and maintains CCTV and access-control systems, covering sales, service and admin processes, configured and tested by me.
status: completed
status_label: Practice org · 2026
featured: true
order: 1
card_image: /assets/img/harbourline/service.png
card_alt: Harbourline Service Home page in Salesforce, with the Support Queue
tags: [Sales Cloud, Service Cloud, Flow, Security, Data Management, Reports]
role: Salesforce Administrator. I designed, configured, tested and documented everything.
org: Salesforce Developer Edition, with two full user licences
period: August 2026 – present
tools: Setup, Flow Builder, Schema Builder, Lightning App Builder, Data Import Wizard, dataloader.io
links:
  - label: Full documentation
    url: /docs/harbourline/
  - label: Download as PDF
    url: /docs/harbourline/Serap_Ozturk-Kucuk_Salesforce_Admin_Portfolio_v1.1.pdf
stats:
  - value: "28"
    label: customer Accounts
  - value: "65"
    label: customer sites
  - value: "40"
    label: deals migrated
  - value: "4"
    label: record-triggered flows
contents:
  - id: scenario
    title: Business scenario
  - id: requirements
    title: Requirements
  - id: data-model
    title: Data model
  - id: configuration
    title: Configuration
  - id: automation
    title: Automation and Flow
  - id: security
    title: Security and access
  - id: testing
    title: Testing and troubleshooting
  - id: results
    title: Results and lessons
---

<h2 id="scenario">1. Business scenario and challenge</h2>

Harbourline Security is a fictional company in the North West of England. It sells, installs and maintains CCTV and access-control systems for businesses such as logistics firms, care homes and colleges. A sales team wins new customers and sells more to existing ones, and a service team handles customer problems.

Before Salesforce, the scenario was:

- **Discounts were agreed by email**, with no record of who approved what.
- **Support emails arrived in a shared inbox**, where they could be missed.
- **Customer site details were scattered**, so engineers had to ask around.
- **Requests to the admin were made in passing**, with no list and no priorities.

<h2 id="requirements">2. Requirements and objectives</h2>

| The business needs | How I met it |
|---|---|
| Sales reps see only their own deals; managers see their team's | Private org-wide default for Opportunities, plus a role hierarchy |
| Discounts over 15% need the manager, over 40% the Sales VP too | Approval process with two steps, and a validation rule so it can't be skipped |
| Every support email becomes a case with an owner | Email-to-Case, a support queue, an assignment rule and an automatic reply |
| The service manager hears at once about problems at the biggest customers | Record-triggered flow that emails the manager for Key Account cases |
| Old CRM data moves over without duplicates | Legacy IDs as External IDs, and upserts tested on 4 rows first |
| Managers see the numbers each week | 13 reports on three dashboards |
| One place for all requests to the admin | A custom Admin Request object with a Kanban board |

<h2 id="data-model">3. Solution architecture and data model</h2>

- **Building** (custom): a customer's site. Master-detail to Account, because a site can't exist without its customer. A roll-up summary on Account counts the sites.
- **Site Contact**: a lookup from Building to Contact, because a site doesn't always have a named person.
- **Training Session** and **Session Attendee**: customers' staff attend many sessions, and each session has many people, so a junction object links them.
- **Admin Request** (custom): tracks requests to the admin, with an auto-number, a priority formula and a status lifecycle.
- **Legacy IDs** on Account, Contact and Opportunity: External IDs, so imports can match records without Salesforce IDs.

<figure class="shot narrow"><img src="{{ '/assets/img/harbourline/schema.png' | relative_url }}" alt="Schema Builder showing Account, Contact, Building, Session Attendee and Training Session" loading="lazy"><figcaption>Schema Builder: Building linked to Account and Contact, and the Session Attendee junction between Contact and Training Session.</figcaption></figure>

<h2 id="configuration">4. Salesforce configuration</h2>

**Sales**
- Two record types and sales processes: External Sale (new customers, with a Site Survey stage) and Internal Sale (existing customers), each with its own Path.
- Three products in the standard price book, and a discount approval process with a validation rule that blocks the price stages until the discount is approved.

<figure class="shot"><img src="{{ '/assets/img/harbourline/validation.png' | relative_url }}" alt="Validation rule error stopping a deal with an unapproved discount from moving to Proposal" loading="lazy"><figcaption>The validation rule at work: a deal with an unapproved discount can't move to Proposal/Price Quote.</figcaption></figure>

**Service**
- Email-to-Case, a Support Queue, an assignment rule, an auto-response, and an escalation rule after four hours.

<figure class="shot"><img src="{{ '/assets/img/harbourline/service.png' | relative_url }}" alt="Service Home page with the Support Queue and open cases" loading="lazy"><figcaption>Service Home as the service manager. The red flag on case 00001026 is the escalation rule.</figcaption></figure>

**For every team**
- A Lightning app per team with only the tabs they use, Home pages by app and profile, and compact layouts with the key fields at the top.
- 13 reports on Sales, Service and Admin Work dashboards, each answering one question a manager asks.

<figure class="shot narrow"><img src="{{ '/assets/img/harbourline/sales.png' | relative_url }}" alt="Harbourline Sales dashboard with discounts awaiting approval and open pipeline by stage" loading="lazy"><figcaption>The Sales dashboard: discounts waiting for approval, open pipeline by stage, and won revenue by customer tier.</figcaption></figure>

**Admin Request tracker**
- Anyone raises a request from the + menu. Priority (P1 to P4) is worked out from Impact and Urgency, so nobody picks their own. A rejection needs a reason, and requesters are emailed when the status changes.

<figure class="shot"><img src="{{ '/assets/img/harbourline/kanban.png' | relative_url }}" alt="Kanban board of admin requests grouped by status" loading="lazy"><figcaption>All open requests on a Kanban board. Everyone can see it; only the Admin Team can move cards.</figcaption></figure>

<h2 id="automation">5. Automation and Flow design</h2>

I use Flow only when a simpler tool can't do the job. Formulas, roll-ups, validation rules and approval processes handle the rest.

| Flow | Type | What it does |
|---|---|---|
| Create HQ Building | After save, Opportunity | When a deal is won, creates the customer's Headquarters Building, unless one already exists |
| Key Account case alert | After save, Case | Emails the service manager when a Key Account customer opens a case |
| Admin Request owner and dates | Before save, Admin Request | Sets the queue as owner and fills in the requester; stamps Date Closed, and clears it if the request is reopened |
| Admin Request status email | After save, Admin Request | Emails the requester when the status changes |

<figure class="shot narrow"><img src="{{ '/assets/img/harbourline/flow.png' | relative_url }}" alt="Flow Builder showing the Create HQ Building flow with Get Records, a decision and Create Records" loading="lazy"><figcaption>The Create HQ Building flow: it looks for the building first, and creates it only on the "No HQ" path.</figcaption></figure>

<h2 id="security">6. Security and access management</h2>

| Object | Org-wide default | Opened up by |
|---|---|---|
| Opportunity | Private | Role hierarchy, a criteria-based sharing rule, an account team |
| Account | Public Read Only | Owners and managers edit |
| Contact | Controlled by Parent | Follows the Account |
| Case | Public Read Only | Service team edits |
| Admin Request | Public Read Only | Only the Admin Team queue edits |

- Two custom profiles (Sales, Service), with permission sets for single extras, such as deleting Opportunities.
- A sharing rule gives the service team read access to won deals. An account team gives two people access to one Key Account.

<h2 id="testing">7. Testing and troubleshooting</h2>

**How I tested**
- I checked every user's view with **Login As**. With only two licences, I moved the spare licence from user to user.
- I tested every flow on the path where it should act and on the path where it shouldn't, including a bulk test with 100 cases.
- I tested every import with a 4-row file first, and counted records before and after.
- I tested Email-to-Case end to end with a real email.

**Problems I hit, and how I fixed them**

| Problem | Cause and fix |
|---|---|
| Approval failed: "Manager field is empty" | The submitter had no Manager. Now on my new-user checklist |
| Lead conversion failed on a Legacy ID | The field was required. Legacy IDs are now optional, and required fields are set on the page layout |
| A user saw no deals despite a sharing rule | Her profile had no Read on Opportunity. Sharing opens records; the profile opens the object |
| A rep's Home page chart was empty | Report charts run as the viewer. A dashboard that runs as a fixed user shows team totals |
| The Contacts import duplicated Accounts | The wrong import option. I cleaned up, tested 4 rows, then reloaded with matching on email |

<h2 id="results">8. Results, limitations and lessons learned</h2>

**Results.** Each business need in section 2 is now a defined process in Salesforce, configured and tested. These results come from a practice org. In a live business I would measure the before and after, such as approval times and cases escalated, before claiming results.

**Limitations**
- A Developer Edition org with two licences, so I tested users one at a time.
- Email-to-Case is tested on the main path only. Emails from unknown senders and reply threading are not tested yet.
- Web-to-Case, entitlements and sandboxes are not part of this build.

**What I'd do next:** a sandbox-style deployment between two orgs, Screen Flows, and Permission Set Groups.
