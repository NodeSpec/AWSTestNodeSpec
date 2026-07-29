# Task: SNS Alarm Topic (→ PagerDuty/Slack)

> **Scope:** implement ONLY this node ("SNS Alarm Topic (→ PagerDuty/Slack)"). Work belonging to other nodes appears here solely as interfaces and coordination points — do not implement or re-derive it.
> This document is DERIVED from the NodeSpec model + catalog (fingerprinted, regenerable via generate_task_docs). Node context/export is the model truth; propose model changes through the proposal flow — hand-edits to model facts here do not change the model.

## Component Purpose

**Role:** Pub/Sub Topic
**Technology:** AWS SNS
**Description:** Publish-subscribe topic for fan-out event distribution to multiple consumers

## Your Deliverable

This is a provider-managed service — no application code implements it.
- **Provisioning configuration (IaC)** — declare the service as config artifacts: existence, sizing, wiring, permissions. The IaC tool is NOT declared on this project's platform container — CONFIRM the tool with the user (Terraform / OpenTofu / Pulumi / provider-native / CDK) before authoring artifacts; do NOT assume one.
- **Connection contracts** for every interface below

## Implementation Tasks

Ordered WORK ORDERS synthesized from the model — this node's deliverable kind, contracts, criterion attribution, configuration, and dependency chain. They guarantee coverage, scope, and traceability; they deliberately do NOT contain the implementation detail — that is your job (see the expansion directive below the list).

- [ ] **T1 — Provision AWS SNS via IaC.**
  Author the provisioning definition as config artifacts bound to this node — existence, wiring, permissions, deployed under AWS Cloud Platform.
  [PLACEHOLDER: config — no user configuration recorded for this node; confirm sizing/domains/settings with the user]
- [ ] **T2 — Expose the interface CloudWatch Logs (JSON, 90d hot + Glacier) consumes, per Contract "SNS Topic (CloudWatch Alarms → PagerDuty/Slack)" (custom).**
  Record the endpoint/identifiers CloudWatch Logs (JSON, 90d hot + Glacier) needs in this node's config artifacts — coordinate with CloudWatch Logs (JSON, 90d hot + Glacier).
  Build to the contract schema EXACTLY (see Interface Contracts).
  ↳ serves: REQ-012 "CloudWatch Alarms notify the on-call channel (SNS → PagerDuty / Slack) when: error rate > 1% for 5 min, p95 latency > 500 ms for 5 min, any CRITICAL log line appears" — coordinate with CloudWatch Logs (JSON, 90d hot + Glacier)
- [ ] **T3 — Expose the interface Amazon SES (Transactional Email) consumes, per Contract "SES Bounce/Complaint Notifications" (custom).**
  Record the endpoint/identifiers Amazon SES (Transactional Email) needs in this node's config artifacts — coordinate with Amazon SES (Transactional Email).
  Build to the contract schema EXACTLY (see Interface Contracts).
- [ ] **T4 — Resolve ownership, then implement: "All application logs are structured JSON with mandatory fields: timestamp, level, traceId, service, userId (if available), message" (REQ-012).**
  [PLACEHOLDER: owner — this node or a sharing node (CloudWatch Logs (JSON, 90d hot + Glacier), AWS X-Ray (5% + 100% errors sampling)); assign via the requirement mapping, then keep this task here or move it to the owning node's doc]
  ↳ serves: REQ-012 "All application logs are structured JSON with mandatory fields: timestamp, level, traceId, service, userId (if available), message"
- [ ] **T5 — Resolve ownership, then implement: "Logs are streamed to CloudWatch Logs; log groups have a retention policy of ≥ 90 days (hot) and archived to S3 Glacier after 1 year" (REQ-012).**
  [PLACEHOLDER: owner — this node or a sharing node (CloudWatch Logs (JSON, 90d hot + Glacier), AWS X-Ray (5% + 100% errors sampling)); assign via the requirement mapping, then keep this task here or move it to the owning node's doc]
  ↳ serves: REQ-012 "Logs are streamed to CloudWatch Logs; log groups have a retention policy of ≥ 90 days (hot) and archived to S3 Glacier after 1 year"
- [ ] **T6 — Resolve ownership, then implement: "AWS X-Ray tracing is enabled end-to-end; trace sampling is set to capture 5% of requests plus 100% of errors" (REQ-012).**
  [PLACEHOLDER: owner — this node or a sharing node (CloudWatch Logs (JSON, 90d hot + Glacier), AWS X-Ray (5% + 100% errors sampling)); assign via the requirement mapping, then keep this task here or move it to the owning node's doc]
  ↳ serves: REQ-012 "AWS X-Ray tracing is enabled end-to-end; trace sampling is set to capture 5% of requests plus 100% of errors"
- [ ] **T7 — Resolve ownership, then implement: "A CloudWatch dashboard displays: request rate, p95/p99 latency, error rate, DB connection pool usage, cache hit ratio, ALB 5xx rate" (REQ-012).**
  [PLACEHOLDER: owner — this node or a sharing node (CloudWatch Logs (JSON, 90d hot + Glacier), AWS X-Ray (5% + 100% errors sampling)); assign via the requirement mapping, then keep this task here or move it to the owning node's doc]
  ↳ serves: REQ-012 "A CloudWatch dashboard displays: request rate, p95/p99 latency, error rate, DB connection pool usage, cache hit ratio, ALB 5xx rate"
- [ ] **T8 — Resolve ownership, then implement: "AWS CloudTrail is enabled in all regions and in all accounts; log file validation is turned on" (REQ-012).**
  [PLACEHOLDER: owner — this node or a sharing node (CloudWatch Logs (JSON, 90d hot + Glacier), AWS X-Ray (5% + 100% errors sampling)); assign via the requirement mapping, then keep this task here or move it to the owning node's doc]
  ↳ serves: REQ-012 "AWS CloudTrail is enabled in all regions and in all accounts; log file validation is turned on"
- [ ] **T9 — Resolve ownership, then implement: "Security Hub findings of HIGH or CRITICAL severity trigger an automated alert within 15 minutes" (REQ-012).**
  [PLACEHOLDER: owner — this node or a sharing node (CloudWatch Logs (JSON, 90d hot + Glacier), AWS X-Ray (5% + 100% errors sampling)); assign via the requirement mapping, then keep this task here or move it to the owning node's doc]
  ↳ serves: REQ-012 "Security Hub findings of HIGH or CRITICAL severity trigger an automated alert within 15 minutes"
- [ ] **T10 — Verify every acceptance criterion above and tick its box.**
  Completion evidence flows back to the requirement criteria. This node is complete only when every criterion box is ticked and no `[PLACEHOLDER: …]` tag remains open.

**Your first action — expand these work orders.** Each task above guarantees WHAT must be covered, not HOW. Before writing any code or configuration, expand every task with the concrete implementation steps for THIS technology in THIS project — the specific resources, settings, files, schemas, and tests — using the Configuration, Interface Contracts, Technology Guidance, and node context as your references. Record the expanded list in this section via update_artifact (propose_patches) after this doc is accepted, keeping task IDs, criterion citations, and open `[PLACEHOLDER: …]` tags intact. Resolve placeholders with the user through the proposal flow; this node is never complete while one remains open.

## Requirements — Your Scope

### REQ-012: Observability: Logging, Metrics & Alerting
Category: non-functional | Status: in-progress
_Shared with: CloudWatch Logs (JSON, 90d hot + Glacier), AWS X-Ray (5% + 100% errors sampling) — their slices live in their own task docs._
The application emits structured JSON logs to CloudWatch Logs, custom metrics to CloudWatch Metrics, and distributed traces via AWS X-Ray. Dashboards surface key health indicators. Alerts fire on SLO violations, error-rate spikes, and security events. Log retention and access follow least-privilege principles.

**Acceptance criteria — your task boxes:**
- [ ] All application logs are structured JSON with mandatory fields: timestamp, level, traceId, service, userId (if available), message
  → covered by Task T4
- [ ] Logs are streamed to CloudWatch Logs; log groups have a retention policy of ≥ 90 days (hot) and archived to S3 Glacier after 1 year
  → covered by Task T5
- [ ] AWS X-Ray tracing is enabled end-to-end; trace sampling is set to capture 5% of requests plus 100% of errors
  → covered by Task T6
- [ ] A CloudWatch dashboard displays: request rate, p95/p99 latency, error rate, DB connection pool usage, cache hit ratio, ALB 5xx rate
  → covered by Task T7
- [ ] CloudWatch Alarms notify the on-call channel (SNS → PagerDuty / Slack) when: error rate > 1% for 5 min, p95 latency > 500 ms for 5 min, any CRITICAL log line appears
  → covered by Task T2
- [ ] AWS CloudTrail is enabled in all regions and in all accounts; log file validation is turned on
  → covered by Task T8
- [ ] Security Hub findings of HIGH or CRITICAL severity trigger an automated alert within 15 minutes
  → covered by Task T9

## Interface Contracts

### RECEIVES FROM: CloudWatch Logs (JSON, 90d hot + Glacier) (logging)
- **Contract:** SNS Topic (CloudWatch Alarms → PagerDuty/Slack)
- **Protocol:** custom
- **Their Technology:** aws-cloudwatch

**Schema:**
```
{
  "topicType": "Standard",
  "encryption": "KMS CMK",
  "accessPolicy": "publish restricted to CloudWatch, SES, Security Hub",
  "messageFormat": {
    "state": "ALARM|OK",
    "metric": "string",
    "reason": "string",
    "alarmName": "string",
    "threshold": "number",
    "timestamp": "ISO 8601"
  },
  "subscriptions": [
    {
      "endpoint": "PagerDuty Events API v2",
      "protocol": "HTTPS"
    },
    {
      "endpoint": "Slack Incoming Webhook",
      "protocol": "HTTPS"
    }
  ]
}
```

### RECEIVES FROM: Amazon SES (Transactional Email) (external-service)
- **Contract:** SES Bounce/Complaint Notifications
- **Protocol:** custom
- **Their Technology:** aws-ses

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

## Technology Guidance

_Reference for executing the Implementation Tasks above — apply where relevant. The task list stands even where this guidance is thin._

**Purpose:** AWS-native pub/sub messaging service for fan-out notifications, A2A and A2P messaging. Best for decoupling microservices via topic-based fan-out, sending push notifications, and triggering multiple SQS queues or Lambda functions from a single event. Use SNS+SQS fan-out pattern for reliable multi-subscriber delivery. Not suited for ordered message processing (use SQS FIFO) or event replay (use Kinesis).

**Best Practices:**
- Use SNS+SQS fan-out pattern for reliable multi-subscriber delivery
- Enable message filtering policies to reduce unnecessary processing
- Use FIFO topics when message ordering and deduplication matter
- Set up dead-letter queues on subscriptions for failed deliveries
- Use message attributes for metadata instead of embedding in body

**Anti-Patterns to Avoid:**
- Using SNS for ordered processing without FIFO topics
- Relying on SNS for durable event replay (use Kinesis or EventBridge)
- Sending large payloads directly (use S3 references for >256KB)
- Not setting up DLQs on subscriptions
- Using SNS when simple point-to-point queuing suffices (use SQS directly)

**Suggested File Structure:**
- `src/messaging/sns-client.ts` (source)
- `src/messaging/sns-topics.ts` (config)
- `terraform/sns.tf` (config)

## Dependency Chain

Startup/initialization order based on edge directions and interaction patterns.

**Depends on THIS node being available:**
- CloudWatch Logs (JSON, 90d hot + Glacier) (initiates SNS Topic (CloudWatch Alarms → PagerDuty/Slack) against this node (custom))
- Amazon SES (Transactional Email) (initiates SES Bounce/Complaint Notifications against this node (custom))

**Parent Container:** AWS Cloud Platform (aws)
