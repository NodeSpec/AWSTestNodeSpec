# Task: Amazon SES (Transactional Email)

> **Scope:** implement ONLY this node ("Amazon SES (Transactional Email)"). Work belonging to other nodes appears here solely as interfaces and coordination points — do not implement or re-derive it.
> This document is DERIVED from the NodeSpec model + catalog (fingerprinted, regenerable via generate_task_docs). Node context/export is the model truth; propose model changes through the proposal flow — hand-edits to model facts here do not change the model.

## Component Purpose

**Role:** External Service
**Technology:** AWS SES
**Description:** Third-party API or SaaS integration

## Your Deliverable

This component is an engine that owns its own internals (AWS SES). Never decompose its internals into architecture nodes, and never reimplement its functionality as application code.
- **Connection contracts** for every interface below (triggers, payloads, endpoints)
- **Provisioning configuration (IaC)** — declare the service as config artifacts: existence, sizing, wiring, permissions. The IaC tool is NOT declared on this project's platform container — CONFIRM the tool with the user (Terraform / OpenTofu / Pulumi / provider-native / CDK) before authoring artifacts; do NOT assume one.

## Implementation Tasks

Ordered WORK ORDERS synthesized from the model — this node's deliverable kind, contracts, criterion attribution, configuration, and dependency chain. They guarantee coverage, scope, and traceability; they deliberately do NOT contain the implementation detail — that is your job (see the expansion directive below the list).

- [ ] **T1 — Provision AWS SES via IaC.**
  Author the provisioning definition as config artifacts bound to this node — existence, wiring, permissions, deployed under AWS Cloud Platform.
  [PLACEHOLDER: config — no user configuration recorded for this node; confirm sizing/domains/settings with the user]
- [ ] **T2 — Declare the wiring to SNS Alarm Topic (→ PagerDuty/Slack) (aws-sns) per Contract "SES Bounce/Complaint Notifications" (custom).**
  Build to the contract schema EXACTLY (see Interface Contracts).
  ↳ serves (unverified match): REQ-007 "SES is configured with a verified sending domain, DKIM, SPF, and DMARC records" — requirement not mapped to that node; verify or reassign before relying on it
  ↳ serves (unverified match): REQ-007 "Bounce and complaint notifications are handled: bounced addresses are suppressed; complaint rates above 0.1% trigger an alert" — requirement not mapped to that node; verify or reassign before relying on it
  ↳ serves (unverified match): REQ-007 "Email delivery status (sent, bounced, complained) is logged per user for audit purposes" — requirement not mapped to that node; verify or reassign before relying on it
- [ ] **T3 — Expose the interface Lambda Email Worker (SQS → SES) consumes, per Contract "Amazon SES (Email Sending)" (custom).**
  Record the endpoint/identifiers Lambda Email Worker (SQS → SES) needs in this node's config artifacts — coordinate with Lambda Email Worker (SQS → SES).
  Build to the contract schema EXACTLY (see Interface Contracts).
- [ ] **T4 — Resolve ownership, then implement: "Transactional emails are queued via SQS and processed by a background worker; sending is never synchronous in the request path" (REQ-007).**
  [PLACEHOLDER: owner — this node or a sharing node (SQS Email Queue (FIFO, DLQ), Lambda Email Worker (SQS → SES)); assign via the requirement mapping, then keep this task here or move it to the owning node's doc]
  ↳ serves: REQ-007 "Transactional emails are queued via SQS and processed by a background worker; sending is never synchronous in the request path"
- [ ] **T5 — Resolve ownership, then implement: "All email templates are stored as version-controlled HTML/text files with variable substitution" (REQ-007).**
  [PLACEHOLDER: owner — this node or a sharing node (SQS Email Queue (FIFO, DLQ), Lambda Email Worker (SQS → SES)); assign via the requirement mapping, then keep this task here or move it to the owning node's doc]
  ↳ serves: REQ-007 "All email templates are stored as version-controlled HTML/text files with variable substitution"
- [ ] **T6 — Resolve ownership, then implement: "Unsubscribe links are included in all marketing-adjacent emails per CAN-SPAM / GDPR" (REQ-007).**
  [PLACEHOLDER: owner — this node or a sharing node (SQS Email Queue (FIFO, DLQ), Lambda Email Worker (SQS → SES)); assign via the requirement mapping, then keep this task here or move it to the owning node's doc]
  ↳ serves: REQ-007 "Unsubscribe links are included in all marketing-adjacent emails per CAN-SPAM / GDPR"
- [ ] **T7 — Verify every acceptance criterion above and tick its box.**
  Completion evidence flows back to the requirement criteria. This node is complete only when every criterion box is ticked and no `[PLACEHOLDER: …]` tag remains open.

**Your first action — expand these work orders.** Each task above guarantees WHAT must be covered, not HOW. Before writing any code or configuration, expand every task with the concrete implementation steps for THIS technology in THIS project — the specific resources, settings, files, schemas, and tests — using the Configuration, Interface Contracts, Technology Guidance, and node context as your references. Record the expanded list in this section via update_artifact (propose_patches) after this doc is accepted, keeping task IDs, criterion citations, and open `[PLACEHOLDER: …]` tags intact. Resolve placeholders with the user through the proposal flow; this node is never complete while one remains open.

## Requirements — Your Scope

### REQ-007: Email Notification Service
Category: functional | Status: in-progress
_Shared with: SQS Email Queue (FIFO, DLQ), Lambda Email Worker (SQS → SES) — their slices live in their own task docs._
The application sends transactional emails (verification, password reset, invitations, alerts) via Amazon SES. Email sending is decoupled from the request path using an async queue (SQS). Templates are version-controlled. Delivery, bounce, and complaint events are tracked and acted upon.

**Acceptance criteria — your task boxes:**
- [ ] Transactional emails are queued via SQS and processed by a background worker; sending is never synchronous in the request path
  → covered by Task T4
- [ ] SES is configured with a verified sending domain, DKIM, SPF, and DMARC records
  → covered by Task T2
- [ ] Bounce and complaint notifications are handled: bounced addresses are suppressed; complaint rates above 0.1% trigger an alert
  → covered by Task T2
- [ ] All email templates are stored as version-controlled HTML/text files with variable substitution
  → covered by Task T5
- [ ] Unsubscribe links are included in all marketing-adjacent emails per CAN-SPAM / GDPR
  → covered by Task T6
- [ ] Email delivery status (sent, bounced, complained) is logged per user for audit purposes
  → covered by Task T2

## Interface Contracts

### SENDS TO: SNS Alarm Topic (→ PagerDuty/Slack) (topic)
- **Contract:** SES Bounce/Complaint Notifications
- **Protocol:** custom
- **Their Technology:** aws-sns

**Schema:**
```
{
  "source": "SES → SNS",
  "actions": {
    "complaint": "suppress + alert if rate > 0.1%",
    "permanentBounce": "add to suppression list",
    "transientBounce": "retry 3x then suppress"
  },
  "bounceMessage": {
    "timestamp": "ISO 8601",
    "bounceType": "Permanent|Transient",
    "feedbackId": "string",
    "bouncedRecipients": [
      {
        "status": "string",
        "emailAddress": "string",
        "diagnosticCode": "string"
      }
    ]
  },
  "complaintMessage": {
    "timestamp": "ISO 8601",
    "feedbackId": "string",
    "complainedRecipients": [
      {
        "emailAddress": "string"
      }
    ],
    "complaintFeedbackType": "abuse|auth-failure|fraud|not-spam|other|virus"
  },
  "notificationTypes": [
    "Bounce",
    "Complaint"
  ]
}
```

### RECEIVES FROM: Lambda Email Worker (SQS → SES) (serverless-function)
- **Contract:** Amazon SES (Email Sending)
- **Protocol:** custom
- **Their Technology:** aws-lambda

**Schema:**
```
{
  "service": "SESv2",
  "templates": "emails/templates/ (HTML + text, Handlebars)",
  "compliance": {
    "gdpr": true,
    "canSpam": true,
    "unsubscribeLink": "required on marketing emails"
  },
  "suppressionList": "account-level auto-suppress on bounce",
  "configurationSet": "app-email-{env}",
  "eventDestination": "SNS topic for bounce/complaint",
  "domainVerification": "DKIM + SPF + DMARC (p=quarantine)"
}
```

## Technology Guidance

_Reference for executing the Implementation Tasks above — apply where relevant. The task list stands even where this guidance is thin._

**Purpose:** Managed email service for transactional and bulk sending at high deliverability

**SDK Initialization:**
```
import { SESv2Client, SendEmailCommand } from "@aws-sdk/client-sesv2";
const ses = new SESv2Client({ region: "us-east-1" });
await ses.send(new SendEmailCommand({ FromEmailAddress: "noreply@example.com", Destination: { ToAddresses: ["user@example.com"] }, Content: { Simple: { Subject: { Data: "Welcome" }, Body: { Text: { Data: "Hello" } } } } }));
```

**Best Practices:**
- Verify sending domains with DKIM, SPF, and DMARC before production traffic
- Use configuration sets to track bounces, complaints, and deliveries
- Route bounce and complaint notifications to SNS and suppress bad addresses
- Request production access early to exit the sandbox
- Warm up sending volume gradually on new domains
- Use SES templates for personalized transactional email

**Anti-Patterns to Avoid:**
- Sending from an unverified domain in production
- Ignoring bounce and complaint feedback loops until reputation drops
- Staying in sandbox mode for production workloads
- Hardcoding SMTP credentials instead of using IAM roles

**Suggested File Structure:**
- `infra/aws/ses.tf` (config)
- `emails/templates/` (source)

## Dependency Chain

Startup/initialization order based on edge directions and interaction patterns.

**Must be available BEFORE this node starts:**
- SNS Alarm Topic (→ PagerDuty/Slack) (this node calls/depends on it via SES Bounce/Complaint Notifications (custom))

**Depends on THIS node being available:**
- Lambda Email Worker (SQS → SES) (initiates Amazon SES (Email Sending) against this node (custom))

**Parent Container:** AWS Cloud Platform (aws)
