# Task: CloudWatch Logs (JSON, 90d hot + Glacier)

> **Scope:** implement ONLY this node ("CloudWatch Logs (JSON, 90d hot + Glacier)"). Work belonging to other nodes appears here solely as interfaces and coordination points — do not implement or re-derive it.
> This document is DERIVED from the NodeSpec model + catalog (fingerprinted, regenerable via generate_task_docs). Node context/export is the model truth; propose model changes through the proposal flow — hand-edits to model facts here do not change the model.

## Component Purpose

**Role:** Logging
**Technology:** AWS CloudWatch
**Description:** Centralized log aggregation and analysis

## Your Deliverable

This is a provider-managed service — no application code implements it.
- **Provisioning configuration (IaC)** — declare the service as config artifacts: existence, sizing, wiring, permissions. The IaC tool is NOT declared on this project's platform container — CONFIRM the tool with the user (Terraform / OpenTofu / Pulumi / provider-native / CDK) before authoring artifacts; do NOT assume one.
- **Connection contracts** for every interface below

## Implementation Tasks

Ordered WORK ORDERS synthesized from the model — this node's deliverable kind, contracts, criterion attribution, configuration, and dependency chain. They guarantee coverage, scope, and traceability; they deliberately do NOT contain the implementation detail — that is your job (see the expansion directive below the list).

- [ ] **T1 — Provision AWS CloudWatch via IaC.**
  Author the provisioning definition as config artifacts bound to this node — existence, wiring, permissions, deployed under AWS Cloud Platform.
  [PLACEHOLDER: config — no user configuration recorded for this node; confirm sizing/domains/settings with the user]
- [ ] **T2 — Declare the wiring to SNS Alarm Topic (→ PagerDuty/Slack) (aws-sns) per Contract "SNS Topic (CloudWatch Alarms → PagerDuty/Slack)" (custom).**
  Build to the contract schema EXACTLY (see Interface Contracts).
  ↳ serves: REQ-012 "CloudWatch Alarms notify the on-call channel (SNS → PagerDuty / Slack) when: error rate > 1% for 5 min, p95 latency > 500 ms for 5 min, any CRITICAL log line appears" — coordinate with SNS Alarm Topic (→ PagerDuty/Slack)
- [ ] **T3 — Declare the wiring to KMS Customer Managed Key (CMK) (aws-kms) per Contract "KMS Data Encryption (At-Rest)" (custom).**
  Build to the contract schema EXACTLY (see Interface Contracts).
- [ ] **T4 — Expose the interface Lambda Email Worker (SQS → SES) consumes, per Contract "CloudWatch Logs (Structured JSON)" (custom).**
  Record the endpoint/identifiers Lambda Email Worker (SQS → SES) needs in this node's config artifacts — coordinate with Lambda Email Worker (SQS → SES).
  Build to the contract schema EXACTLY (see Interface Contracts).
  ↳ serves (unverified match): REQ-012 "All application logs are structured JSON with mandatory fields: timestamp, level, traceId, service, userId (if available), message" — requirement not mapped to that node; verify or reassign before relying on it
  ↳ serves (unverified match): REQ-012 "Logs are streamed to CloudWatch Logs; log groups have a retention policy of ≥ 90 days (hot) and archived to S3 Glacier after 1 year" — requirement not mapped to that node; verify or reassign before relying on it
  ↳ serves (unverified match): REQ-012 "A CloudWatch dashboard displays: request rate, p95/p99 latency, error rate, DB connection pool usage, cache hit ratio, ALB 5xx rate" — requirement not mapped to that node; verify or reassign before relying on it
- [ ] **T5 — Expose the interface ECS Fargate Backend (Multi-AZ) consumes, per Contract "CloudWatch Logs (Structured JSON)" (custom).**
  Record the endpoint/identifiers ECS Fargate Backend (Multi-AZ) needs in this node's config artifacts — coordinate with ECS Fargate Backend (Multi-AZ).
  Build to the contract schema EXACTLY (see Interface Contracts).
- [ ] **T6 — Resolve ownership, then implement: "AWS X-Ray tracing is enabled end-to-end; trace sampling is set to capture 5% of requests plus 100% of errors" (REQ-012).**
  [PLACEHOLDER: owner — this node or a sharing node (AWS X-Ray (5% + 100% errors sampling), SNS Alarm Topic (→ PagerDuty/Slack)); assign via the requirement mapping, then keep this task here or move it to the owning node's doc]
  ↳ serves: REQ-012 "AWS X-Ray tracing is enabled end-to-end; trace sampling is set to capture 5% of requests plus 100% of errors"
- [ ] **T7 — Resolve ownership, then implement: "AWS CloudTrail is enabled in all regions and in all accounts; log file validation is turned on" (REQ-012).**
  [PLACEHOLDER: owner — this node or a sharing node (AWS X-Ray (5% + 100% errors sampling), SNS Alarm Topic (→ PagerDuty/Slack)); assign via the requirement mapping, then keep this task here or move it to the owning node's doc]
  ↳ serves: REQ-012 "AWS CloudTrail is enabled in all regions and in all accounts; log file validation is turned on"
- [ ] **T8 — Resolve ownership, then implement: "Security Hub findings of HIGH or CRITICAL severity trigger an automated alert within 15 minutes" (REQ-012).**
  [PLACEHOLDER: owner — this node or a sharing node (AWS X-Ray (5% + 100% errors sampling), SNS Alarm Topic (→ PagerDuty/Slack)); assign via the requirement mapping, then keep this task here or move it to the owning node's doc]
  ↳ serves: REQ-012 "Security Hub findings of HIGH or CRITICAL severity trigger an automated alert within 15 minutes"
- [ ] **T9 — Verify every acceptance criterion above and tick its box.**
  Completion evidence flows back to the requirement criteria. This node is complete only when every criterion box is ticked and no `[PLACEHOLDER: …]` tag remains open.

**Your first action — expand these work orders.** Each task above guarantees WHAT must be covered, not HOW. Before writing any code or configuration, expand every task with the concrete implementation steps for THIS technology in THIS project — the specific resources, settings, files, schemas, and tests — using the Configuration, Interface Contracts, Technology Guidance, and node context as your references. Record the expanded list in this section via update_artifact (propose_patches) after this doc is accepted, keeping task IDs, criterion citations, and open `[PLACEHOLDER: …]` tags intact. Resolve placeholders with the user through the proposal flow; this node is never complete while one remains open.

## Requirements — Your Scope

### REQ-012: Observability: Logging, Metrics & Alerting
Category: non-functional | Status: in-progress
_Shared with: AWS X-Ray (5% + 100% errors sampling), SNS Alarm Topic (→ PagerDuty/Slack) — their slices live in their own task docs._
The application emits structured JSON logs to CloudWatch Logs, custom metrics to CloudWatch Metrics, and distributed traces via AWS X-Ray. Dashboards surface key health indicators. Alerts fire on SLO violations, error-rate spikes, and security events. Log retention and access follow least-privilege principles.

**Acceptance criteria — your task boxes:**
- [ ] All application logs are structured JSON with mandatory fields: timestamp, level, traceId, service, userId (if available), message
  → covered by Task T4
- [ ] Logs are streamed to CloudWatch Logs; log groups have a retention policy of ≥ 90 days (hot) and archived to S3 Glacier after 1 year
  → covered by Task T4
- [ ] AWS X-Ray tracing is enabled end-to-end; trace sampling is set to capture 5% of requests plus 100% of errors
  → covered by Task T6
- [ ] A CloudWatch dashboard displays: request rate, p95/p99 latency, error rate, DB connection pool usage, cache hit ratio, ALB 5xx rate
  → covered by Task T4
- [ ] CloudWatch Alarms notify the on-call channel (SNS → PagerDuty / Slack) when: error rate > 1% for 5 min, p95 latency > 500 ms for 5 min, any CRITICAL log line appears
  → covered by Task T2
- [ ] AWS CloudTrail is enabled in all regions and in all accounts; log file validation is turned on
  → covered by Task T7
- [ ] Security Hub findings of HIGH or CRITICAL severity trigger an automated alert within 15 minutes
  → covered by Task T8

## Interface Contracts

### SENDS TO: SNS Alarm Topic (→ PagerDuty/Slack) (topic)
- **Contract:** SNS Topic (CloudWatch Alarms → PagerDuty/Slack)
- **Protocol:** custom
- **Their Technology:** aws-sns

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

### RECEIVES FROM: Lambda Email Worker (SQS → SES) (serverless-function)
- **Contract:** CloudWatch Logs (Structured JSON)
- **Protocol:** custom
- **Their Technology:** aws-lambda

**Schema:**
```
{
  "alarms": [
    {
      "name": "error-rate",
      "action": "SNS",
      "threshold": "1% over 5 min"
    },
    {
      "name": "p95-latency",
      "action": "SNS",
      "threshold": "500ms over 5 min"
    },
    {
      "name": "critical-log",
      "action": "SNS",
      "filter": "level=CRITICAL"
    }
  ],
  "format": "structured JSON",
  "logGroup": "/{env}/app/{service}",
  "dashboard": [
    "requestRate",
    "p95Latency",
    "p99Latency",
    "errorRate",
    "dbPoolUsage",
    "cacheHitRatio",
    "alb5xxRate"
  ],
  "retention": {
    "hot": "90 days",
    "archive": "S3 Glacier after 1 year"
  },
  "encryption": "KMS CMK",
  "mandatoryFields": {
    "level": "DEBUG|INFO|WARN|ERROR|CRITICAL",
    "userId": "authenticated user ID or null",
    "message": "string",
    "service": "api|email-worker",
    "traceId": "X-Ray trace ID",
    "timestamp": "ISO 8601"
  }
}
```

### RECEIVES FROM: ECS Fargate Backend (Multi-AZ) (backend-service)
- **Contract:** CloudWatch Logs (Structured JSON)
- **Protocol:** custom
- **Their Technology:** aws-fargate

**Schema:**
```
{
  "alarms": [
    {
      "name": "error-rate",
      "action": "SNS",
      "threshold": "1% over 5 min"
    },
    {
      "name": "p95-latency",
      "action": "SNS",
      "threshold": "500ms over 5 min"
    },
    {
      "name": "critical-log",
      "action": "SNS",
      "filter": "level=CRITICAL"
    }
  ],
  "format": "structured JSON",
  "logGroup": "/{env}/app/{service}",
  "dashboard": [
    "requestRate",
    "p95Latency",
    "p99Latency",
    "errorRate",
    "dbPoolUsage",
    "cacheHitRatio",
    "alb5xxRate"
  ],
  "retention": {
    "hot": "90 days",
    "archive": "S3 Glacier after 1 year"
  },
  "encryption": "KMS CMK",
  "mandatoryFields": {
    "level": "DEBUG|INFO|WARN|ERROR|CRITICAL",
    "userId": "authenticated user ID or null",
    "message": "string",
    "service": "api|email-worker",
    "traceId": "X-Ray trace ID",
    "timestamp": "ISO 8601"
  }
}
```

## Technology Guidance

_Reference for executing the Implementation Tasks above — apply where relevant. The task list stands even where this guidance is thin._

**Purpose:** AWS-native monitoring and observability service providing metrics, logs, alarms, dashboards, and traces. CloudWatch Logs aggregates application and infrastructure logs; CloudWatch Metrics collects time-series data from 70+ AWS services; CloudWatch Alarms triggers notifications and auto-scaling actions. Best for AWS-native monitoring when you want deep integration with all AWS services without managing Prometheus/Grafana infrastructure. Use Datadog or Grafana Cloud for multi-cloud observability.

**Best Practices:**
- Use CloudWatch Logs Insights for structured log querying
- Create composite alarms for multi-metric health checks
- Use metric filters to extract custom metrics from log data
- Enable Container Insights for ECS/EKS monitoring
- Use CloudWatch Synthetics for endpoint monitoring
- Set up cross-account observability for multi-account architectures

**Anti-Patterns to Avoid:**
- Logging at DEBUG level in production causing excessive costs
- Not setting log retention policies (default is indefinite)
- Creating too many high-resolution custom metrics unnecessarily
- Using CloudWatch alone for complex multi-cloud observability
- Not using structured JSON logging format

**Suggested File Structure:**
- `monitoring/cloudwatch-dashboard.json` (config)
- `monitoring/alarms.ts` (config)
- `terraform/cloudwatch.tf` (config)

## Dependency Chain

Startup/initialization order based on edge directions and interaction patterns.

**Must be available BEFORE this node starts:**
- SNS Alarm Topic (→ PagerDuty/Slack) (this node calls/depends on it via SNS Topic (CloudWatch Alarms → PagerDuty/Slack) (custom))
- KMS Customer Managed Key (CMK) (this node calls/depends on it via KMS Data Encryption (At-Rest) (custom))

**Depends on THIS node being available:**
- Lambda Email Worker (SQS → SES) (initiates CloudWatch Logs (Structured JSON) against this node (custom))
- ECS Fargate Backend (Multi-AZ) (initiates CloudWatch Logs (Structured JSON) against this node (custom))

**Parent Container:** AWS Cloud Platform (aws)
