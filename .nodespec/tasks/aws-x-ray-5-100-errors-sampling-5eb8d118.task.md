# Task: AWS X-Ray (5% + 100% errors sampling)

> **Scope:** implement ONLY this node ("AWS X-Ray (5% + 100% errors sampling)"). Work belonging to other nodes appears here solely as interfaces and coordination points — do not implement or re-derive it.
> This document is DERIVED from the NodeSpec model + catalog (fingerprinted, regenerable via generate_task_docs). Node context/export is the model truth; propose model changes through the proposal flow — hand-edits to model facts here do not change the model.

## Component Purpose

**Role:** Monitoring
**Technology:** AWS X-Ray
**Description:** Application and infrastructure monitoring

## Your Deliverable

This is a provider-managed service — no application code implements it.
- **Provisioning configuration (IaC)** — declare the service as config artifacts: existence, sizing, wiring, permissions. The IaC tool is NOT declared on this project's platform container — CONFIRM the tool with the user (Terraform / OpenTofu / Pulumi / provider-native / CDK) before authoring artifacts; do NOT assume one.
- **Connection contracts** for every interface below

## Implementation Tasks

Ordered WORK ORDERS synthesized from the model — this node's deliverable kind, contracts, criterion attribution, configuration, and dependency chain. They guarantee coverage, scope, and traceability; they deliberately do NOT contain the implementation detail — that is your job (see the expansion directive below the list).

- [ ] **T1 — Provision AWS X-Ray via IaC.**
  Author the provisioning definition as config artifacts bound to this node — existence, wiring, permissions, deployed under AWS Cloud Platform.
  [PLACEHOLDER: config — no user configuration recorded for this node; confirm sizing/domains/settings with the user]
- [ ] **T2 — Expose the interface ECS Fargate Backend (Multi-AZ) consumes, per Contract "AWS X-Ray (Distributed Tracing)" (custom).**
  Record the endpoint/identifiers ECS Fargate Backend (Multi-AZ) needs in this node's config artifacts — coordinate with ECS Fargate Backend (Multi-AZ).
  Build to the contract schema EXACTLY (see Interface Contracts).
  ↳ serves (unverified match): REQ-012 "AWS X-Ray tracing is enabled end-to-end; trace sampling is set to capture 5% of requests plus 100% of errors" — requirement not mapped to that node; verify or reassign before relying on it
  ↳ serves (unverified match): REQ-012 "AWS CloudTrail is enabled in all regions and in all accounts; log file validation is turned on" — requirement not mapped to that node; verify or reassign before relying on it
- [ ] **T3 — Expose the interface Lambda Email Worker (SQS → SES) consumes, per Contract "AWS X-Ray (Distributed Tracing)" (custom).**
  Record the endpoint/identifiers Lambda Email Worker (SQS → SES) needs in this node's config artifacts — coordinate with Lambda Email Worker (SQS → SES).
  Build to the contract schema EXACTLY (see Interface Contracts).
- [ ] **T4 — Resolve ownership, then implement: "All application logs are structured JSON with mandatory fields: timestamp, level, traceId, service, userId (if available), message" (REQ-012).**
  [PLACEHOLDER: owner — this node or a sharing node (CloudWatch Logs (JSON, 90d hot + Glacier), SNS Alarm Topic (→ PagerDuty/Slack)); assign via the requirement mapping, then keep this task here or move it to the owning node's doc]
  ↳ serves: REQ-012 "All application logs are structured JSON with mandatory fields: timestamp, level, traceId, service, userId (if available), message"
- [ ] **T5 — Resolve ownership, then implement: "Logs are streamed to CloudWatch Logs; log groups have a retention policy of ≥ 90 days (hot) and archived to S3 Glacier after 1 year" (REQ-012).**
  [PLACEHOLDER: owner — this node or a sharing node (CloudWatch Logs (JSON, 90d hot + Glacier), SNS Alarm Topic (→ PagerDuty/Slack)); assign via the requirement mapping, then keep this task here or move it to the owning node's doc]
  ↳ serves: REQ-012 "Logs are streamed to CloudWatch Logs; log groups have a retention policy of ≥ 90 days (hot) and archived to S3 Glacier after 1 year"
- [ ] **T6 — Resolve ownership, then implement: "A CloudWatch dashboard displays: request rate, p95/p99 latency, error rate, DB connection pool usage, cache hit ratio, ALB 5xx rate" (REQ-012).**
  [PLACEHOLDER: owner — this node or a sharing node (CloudWatch Logs (JSON, 90d hot + Glacier), SNS Alarm Topic (→ PagerDuty/Slack)); assign via the requirement mapping, then keep this task here or move it to the owning node's doc]
  ↳ serves: REQ-012 "A CloudWatch dashboard displays: request rate, p95/p99 latency, error rate, DB connection pool usage, cache hit ratio, ALB 5xx rate"
- [ ] **T7 — Resolve ownership, then implement: "CloudWatch Alarms notify the on-call channel (SNS → PagerDuty / Slack) when: error rate > 1% for 5 min, p95 latency > 500 ms for 5 min, any CRITICAL log line appears" (REQ-012).**
  [PLACEHOLDER: owner — this node or a sharing node (CloudWatch Logs (JSON, 90d hot + Glacier), SNS Alarm Topic (→ PagerDuty/Slack)); assign via the requirement mapping, then keep this task here or move it to the owning node's doc]
  ↳ serves: REQ-012 "CloudWatch Alarms notify the on-call channel (SNS → PagerDuty / Slack) when: error rate > 1% for 5 min, p95 latency > 500 ms for 5 min, any CRITICAL log line appears"
- [ ] **T8 — Resolve ownership, then implement: "Security Hub findings of HIGH or CRITICAL severity trigger an automated alert within 15 minutes" (REQ-012).**
  [PLACEHOLDER: owner — this node or a sharing node (CloudWatch Logs (JSON, 90d hot + Glacier), SNS Alarm Topic (→ PagerDuty/Slack)); assign via the requirement mapping, then keep this task here or move it to the owning node's doc]
  ↳ serves: REQ-012 "Security Hub findings of HIGH or CRITICAL severity trigger an automated alert within 15 minutes"
- [ ] **T9 — Verify every acceptance criterion above and tick its box.**
  Completion evidence flows back to the requirement criteria. This node is complete only when every criterion box is ticked and no `[PLACEHOLDER: …]` tag remains open.

**Your first action — expand these work orders.** Each task above guarantees WHAT must be covered, not HOW. Before writing any code or configuration, expand every task with the concrete implementation steps for THIS technology in THIS project — the specific resources, settings, files, schemas, and tests — using the Configuration, Interface Contracts, Technology Guidance, and node context as your references. Record the expanded list in this section via update_artifact (propose_patches) after this doc is accepted, keeping task IDs, criterion citations, and open `[PLACEHOLDER: …]` tags intact. Resolve placeholders with the user through the proposal flow; this node is never complete while one remains open.

## Requirements — Your Scope

### REQ-012: Observability: Logging, Metrics & Alerting
Category: non-functional | Status: in-progress
_Shared with: CloudWatch Logs (JSON, 90d hot + Glacier), SNS Alarm Topic (→ PagerDuty/Slack) — their slices live in their own task docs._
The application emits structured JSON logs to CloudWatch Logs, custom metrics to CloudWatch Metrics, and distributed traces via AWS X-Ray. Dashboards surface key health indicators. Alerts fire on SLO violations, error-rate spikes, and security events. Log retention and access follow least-privilege principles.

**Acceptance criteria — your task boxes:**
- [ ] All application logs are structured JSON with mandatory fields: timestamp, level, traceId, service, userId (if available), message
  → covered by Task T4
- [ ] Logs are streamed to CloudWatch Logs; log groups have a retention policy of ≥ 90 days (hot) and archived to S3 Glacier after 1 year
  → covered by Task T5
- [ ] AWS X-Ray tracing is enabled end-to-end; trace sampling is set to capture 5% of requests plus 100% of errors
  → covered by Task T2
- [ ] A CloudWatch dashboard displays: request rate, p95/p99 latency, error rate, DB connection pool usage, cache hit ratio, ALB 5xx rate
  → covered by Task T6
- [ ] CloudWatch Alarms notify the on-call channel (SNS → PagerDuty / Slack) when: error rate > 1% for 5 min, p95 latency > 500 ms for 5 min, any CRITICAL log line appears
  → covered by Task T7
- [ ] AWS CloudTrail is enabled in all regions and in all accounts; log file validation is turned on
  → covered by Task T2
- [ ] Security Hub findings of HIGH or CRITICAL severity trigger an automated alert within 15 minutes
  → covered by Task T8

## Interface Contracts

### RECEIVES FROM: ECS Fargate Backend (Multi-AZ) (backend-service)
- **Contract:** AWS X-Ray (Distributed Tracing)
- **Protocol:** custom
- **Their Technology:** aws-fargate

**Schema:**
```
{
  "sdk": "aws-xray-sdk",
  "daemon": {
    "port": 2000,
    "protocol": "UDP"
  },
  "metadata": {
    "cacheHit": "boolean",
    "responseSize": "number"
  },
  "sampling": {
    "errors": "100% of 5xx responses",
    "nominal": "5% fixed rate, reservoir 1/s"
  },
  "segments": [
    "api-gateway",
    "backend-service",
    "database",
    "cache",
    "s3",
    "sqs",
    "ses"
  ],
  "annotations": {
    "userId": "string",
    "statusCode": "number",
    "requestPath": "string"
  }
}
```

### RECEIVES FROM: Lambda Email Worker (SQS → SES) (serverless-function)
- **Contract:** AWS X-Ray (Distributed Tracing)
- **Protocol:** custom
- **Their Technology:** aws-lambda

**Schema:**
```
{
  "sdk": "aws-xray-sdk",
  "daemon": {
    "port": 2000,
    "protocol": "UDP"
  },
  "metadata": {
    "cacheHit": "boolean",
    "responseSize": "number"
  },
  "sampling": {
    "errors": "100% of 5xx responses",
    "nominal": "5% fixed rate, reservoir 1/s"
  },
  "segments": [
    "api-gateway",
    "backend-service",
    "database",
    "cache",
    "s3",
    "sqs",
    "ses"
  ],
  "annotations": {
    "userId": "string",
    "statusCode": "number",
    "requestPath": "string"
  }
}
```

## Technology Guidance

_Reference for executing the Implementation Tasks above — apply where relevant. The task list stands even where this guidance is thin._

**Purpose:** Distributed tracing service mapping request paths across microservices and AWS resources

**SDK Initialization:**
```
import { NodeSDK } from "@opentelemetry/sdk-node";
import { OTLPTraceExporter } from "@opentelemetry/exporter-trace-otlp-grpc";
new NodeSDK({ traceExporter: new OTLPTraceExporter() }).start();
```

**Best Practices:**
- Instrument services with OpenTelemetry and the ADOT collector
- Enable active tracing on Lambda and API Gateway stages
- Propagate trace context across queue and event boundaries
- Use sampling rules to keep high-volume tracing affordable
- Annotate segments with business keys for trace filtering

**Anti-Patterns to Avoid:**
- Tracing 100 percent of requests on high-volume production services
- Dropping trace context at async boundaries like SQS
- Relying on logs alone to debug cross-service latency

**Suggested File Structure:**
- `infra/aws/xray.tf` (config)

## Dependency Chain

Startup/initialization order based on edge directions and interaction patterns.

**Depends on THIS node being available:**
- ECS Fargate Backend (Multi-AZ) (initiates AWS X-Ray (Distributed Tracing) against this node (custom))
- Lambda Email Worker (SQS → SES) (initiates AWS X-Ray (Distributed Tracing) against this node (custom))

**Parent Container:** AWS Cloud Platform (aws)
