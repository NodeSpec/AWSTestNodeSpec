# Task: SQS Email Queue (FIFO, DLQ)

> **Scope:** implement ONLY this node ("SQS Email Queue (FIFO, DLQ)"). Work belonging to other nodes appears here solely as interfaces and coordination points — do not implement or re-derive it.
> This document is DERIVED from the NodeSpec model + catalog (fingerprinted, regenerable via generate_task_docs). Node context/export is the model truth; propose model changes through the proposal flow — hand-edits to model facts here do not change the model.

## Component Purpose

**Role:** Message Queue
**Technology:** AWS SQS
**Description:** Point-to-point message queue for asynchronous task dispatch and work distribution

## Your Deliverable

This is a provider-managed service — no application code implements it.
- **Provisioning configuration (IaC)** — declare the service as config artifacts: existence, sizing, wiring, permissions. The IaC tool is NOT declared on this project's platform container — CONFIRM the tool with the user (Terraform / OpenTofu / Pulumi / provider-native / CDK) before authoring artifacts; do NOT assume one.
- **Connection contracts** for every interface below

## Implementation Tasks

Ordered WORK ORDERS synthesized from the model — this node's deliverable kind, contracts, criterion attribution, configuration, and dependency chain. They guarantee coverage, scope, and traceability; they deliberately do NOT contain the implementation detail — that is your job (see the expansion directive below the list).

- [ ] **T1 — Provision AWS SQS via IaC.**
  Author the provisioning definition as config artifacts bound to this node — existence, wiring, permissions, deployed under AWS Cloud Platform.
  [PLACEHOLDER: config — no user configuration recorded for this node; confirm sizing/domains/settings with the user]
- [ ] **T2 — Declare the wiring to KMS Customer Managed Key (CMK) (aws-kms) per Contract "KMS Data Encryption (At-Rest)" (custom).**
  Build to the contract schema EXACTLY (see Interface Contracts).
- [ ] **T3 — Declare the wiring to Lambda Email Worker (SQS → SES) (aws-lambda) per Contract "SQS Email Queue (Async)" (amqp).**
  Build to the contract schema EXACTLY (see Interface Contracts).
  ↳ serves (unverified match): REQ-007 "Transactional emails are queued via SQS and processed by a background worker; sending is never synchronous in the request path" — requirement not mapped to that node; verify or reassign before relying on it
  ↳ serves (unverified match): REQ-007 "All email templates are stored as version-controlled HTML/text files with variable substitution" — requirement not mapped to that node; verify or reassign before relying on it
  ↳ serves (unverified match): REQ-007 "Unsubscribe links are included in all marketing-adjacent emails per CAN-SPAM / GDPR" — requirement not mapped to that node; verify or reassign before relying on it
  ↳ serves (unverified match): REQ-007 "Email delivery status (sent, bounced, complained) is logged per user for audit purposes" — requirement not mapped to that node; verify or reassign before relying on it
- [ ] **T4 — Expose the interface ECS Fargate Backend (Multi-AZ) consumes, per Contract "SQS Email Queue (Async)" (amqp).**
  Record the endpoint/identifiers ECS Fargate Backend (Multi-AZ) needs in this node's config artifacts — coordinate with ECS Fargate Backend (Multi-AZ).
  Build to the contract schema EXACTLY (see Interface Contracts).
- [ ] **T5 — Resolve ownership, then implement: "SES is configured with a verified sending domain, DKIM, SPF, and DMARC records" (REQ-007).**
  [PLACEHOLDER: owner — this node or a sharing node (Lambda Email Worker (SQS → SES), Amazon SES (Transactional Email)); assign via the requirement mapping, then keep this task here or move it to the owning node's doc]
  ↳ serves: REQ-007 "SES is configured with a verified sending domain, DKIM, SPF, and DMARC records"
- [ ] **T6 — Resolve ownership, then implement: "Bounce and complaint notifications are handled: bounced addresses are suppressed; complaint rates above 0.1% trigger an alert" (REQ-007).**
  [PLACEHOLDER: owner — this node or a sharing node (Lambda Email Worker (SQS → SES), Amazon SES (Transactional Email)); assign via the requirement mapping, then keep this task here or move it to the owning node's doc]
  ↳ serves: REQ-007 "Bounce and complaint notifications are handled: bounced addresses are suppressed; complaint rates above 0.1% trigger an alert"
- [ ] **T7 — Verify every acceptance criterion above and tick its box.**
  Completion evidence flows back to the requirement criteria. This node is complete only when every criterion box is ticked and no `[PLACEHOLDER: …]` tag remains open.

**Your first action — expand these work orders.** Each task above guarantees WHAT must be covered, not HOW. Before writing any code or configuration, expand every task with the concrete implementation steps for THIS technology in THIS project — the specific resources, settings, files, schemas, and tests — using the Configuration, Interface Contracts, Technology Guidance, and node context as your references. Record the expanded list in this section via update_artifact (propose_patches) after this doc is accepted, keeping task IDs, criterion citations, and open `[PLACEHOLDER: …]` tags intact. Resolve placeholders with the user through the proposal flow; this node is never complete while one remains open.

### Platform Capability Equivalence

This node is semantically equivalent to a "AWS SQS" (aws-sqs) platform_capability node. Treat it as the managed AWS service for spec generation, code scaffolding, and architecture decisions.
- **Equivalent Role:** aws-sqs (AWS SQS)
- **Provider:** aws

## Requirements — Your Scope

### REQ-007: Email Notification Service
Category: functional | Status: in-progress
_Shared with: Lambda Email Worker (SQS → SES), Amazon SES (Transactional Email) — their slices live in their own task docs._
The application sends transactional emails (verification, password reset, invitations, alerts) via Amazon SES. Email sending is decoupled from the request path using an async queue (SQS). Templates are version-controlled. Delivery, bounce, and complaint events are tracked and acted upon.

**Acceptance criteria — your task boxes:**
- [ ] Transactional emails are queued via SQS and processed by a background worker; sending is never synchronous in the request path
  → covered by Task T3
- [ ] SES is configured with a verified sending domain, DKIM, SPF, and DMARC records
  → covered by Task T5
- [ ] Bounce and complaint notifications are handled: bounced addresses are suppressed; complaint rates above 0.1% trigger an alert
  → covered by Task T6
- [ ] All email templates are stored as version-controlled HTML/text files with variable substitution
  → covered by Task T3
- [ ] Unsubscribe links are included in all marketing-adjacent emails per CAN-SPAM / GDPR
  → covered by Task T3
- [ ] Email delivery status (sent, bounced, complained) is logged per user for audit purposes
  → covered by Task T3

## Interface Contracts

### SENDS TO: KMS Customer Managed Key (CMK) (auth-provider)
- **Contract:** KMS Data Encryption (At-Rest)
- **Protocol:** custom
- **Their Technology:** aws-kms

**Schema:**
```
{
  "audit": "CloudTrail logs all Encrypt/Decrypt/GenerateDataKey",
  "keyType": "symmetric AES-256",
  "rotation": {
    "automatic": true,
    "intervalDays": 365
  },
  "keyPolicy": {
    "services": [
      "S3 (SSE-KMS)",
      "RDS (storage encryption)",
      "Secrets Manager",
      "SQS",
      "SNS",
      "CloudWatch Logs"
    ],
    "administrators": "IAM Identity Center admins"
  },
  "deletionProtection": "30-day scheduled deletion minimum",
  "envelopeEncryption": "GenerateDataKey per object, encrypt locally, store encrypted key with ciphertext"
}
```

### RECEIVES FROM: ECS Fargate Backend (Multi-AZ) (backend-service)
- **Contract:** SQS Email Queue (Async)
- **Protocol:** amqp
- **Their Technology:** aws-fargate

**Schema:**
```
{
  "dlq": {
    "maxReceiveCount": 3
  },
  "queueType": "FIFO",
  "encryption": "SSE-KMS",
  "messageBody": {
    "to": "email",
    "subject": "string",
    "metadata": {
      "userId": "string",
      "eventType": "verification|password_reset|invitation|alert",
      "timestamp": "ISO 8601"
    },
    "messageId": "uuid",
    "variables": "object",
    "templateId": "string"
  },
  "deduplication": "content-based",
  "messageRetention": "4 days",
  "visibilityTimeout": 60
}
```

### SENDS TO: Lambda Email Worker (SQS → SES) (serverless-function)
- **Contract:** SQS Email Queue (Async)
- **Protocol:** amqp
- **Their Technology:** aws-lambda

**Schema:**
```
{
  "dlq": {
    "maxReceiveCount": 3
  },
  "queueType": "FIFO",
  "encryption": "SSE-KMS",
  "messageBody": {
    "to": "email",
    "subject": "string",
    "metadata": {
      "userId": "string",
      "eventType": "verification|password_reset|invitation|alert",
      "timestamp": "ISO 8601"
    },
    "messageId": "uuid",
    "variables": "object",
    "templateId": "string"
  },
  "deduplication": "content-based",
  "messageRetention": "4 days",
  "visibilityTimeout": 60
}
```

## Technology Guidance

_Reference for executing the Implementation Tasks above — apply where relevant. The task list stands even where this guidance is thin._

**Purpose:** Fully managed message queuing service for decoupling distributed systems. Supports standard (best-effort ordering) and FIFO (exactly-once processing) queue types.

**Best Practices:**
- Use FIFO queues when message ordering matters
- Configure dead-letter queues for failed messages
- Set appropriate visibility timeout based on processing time
- Use long polling to reduce empty receives and cost
- Batch send/receive/delete operations for throughput

**Anti-Patterns to Avoid:**
- Using SQS for real-time communication
- Setting visibility timeout shorter than processing time
- Not handling duplicate messages in standard queues
- Ignoring dead-letter queue messages

**Suggested File Structure:**
- `infra/aws/sqs.tf` (config)

## Manual Steps

> The following steps require manual action by a human. AI cannot complete these steps automatically.

**Quick checklist:**
- [ ] Create SQS Queue *(required)*
- [ ] Configure Dead-Letter Queue *(required)*
- [ ] Set IAM Permissions *(required)*
- [ ] Set Environment Variables *(required)*

### Required Steps

#### [dashboard_config] Create SQS Queue

In AWS Console > SQS > Create Queue. Choose Standard (high throughput, at-least-once) or FIFO (exactly-once, ordered). Set visibility timeout to match your expected processing time.

**Reference:** https://console.aws.amazon.com/sqs/

#### [dashboard_config] Configure Dead-Letter Queue

Create a separate DLQ and configure the main queue to send failed messages (after N receive attempts) to the DLQ. Monitor the DLQ for stuck messages.

#### [permissions] Set IAM Permissions

Create an IAM policy granting sqs:SendMessage, sqs:ReceiveMessage, sqs:DeleteMessage, and sqs:GetQueueAttributes on the specific queue ARN. Attach to your application role or user.

#### [environment_variable] Set Environment Variables

Add SQS queue URL and AWS credentials to your application environment.

```bash
export AWS_SQS_QUEUE_URL=https://sqs.us-east-1.amazonaws.com/123456789/my-queue
export AWS_REGION=us-east-1
export AWS_ACCESS_KEY_ID=<from-iam>
export AWS_SECRET_ACCESS_KEY=<from-iam>
```

## Dependency Chain

Startup/initialization order based on edge directions and interaction patterns.

**Must be available BEFORE this node starts:**
- KMS Customer Managed Key (CMK) (this node calls/depends on it via KMS Data Encryption (At-Rest) (custom))

**Depends on THIS node being available:**
- Lambda Email Worker (SQS → SES) (consumes this node's output via SQS Email Queue (Async) (amqp))

## Error Handling Contracts

**Errors this node MUST emit to consumers:**
- Job failure signals to ECS Fargate Backend (Multi-AZ) ("SQS Email Queue (Async)"): emit failure status with error details, support idempotent retry

**Errors this node MUST handle from dependencies:**
- Queue acknowledgment failures for Lambda Email Worker (SQS → SES) ("SQS Email Queue (Async)"): implement retry semantics with max-retry cap and DLQ

**Parent Container:** AWS Cloud Platform (aws)
