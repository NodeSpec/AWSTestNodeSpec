# Task: Lambda Email Worker (SQS → SES)

> **Scope:** implement ONLY this node ("Lambda Email Worker (SQS → SES)"). Work belonging to other nodes appears here solely as interfaces and coordination points — do not implement or re-derive it.
> This document is DERIVED from the NodeSpec model + catalog (fingerprinted, regenerable via generate_task_docs). Node context/export is the model truth; propose model changes through the proposal flow — hand-edits to model facts here do not change the model.

## Component Purpose

**Role:** Serverless Function
**Technology:** AWS Lambda
**Description:** Serverless or edge-deployed function (AWS Lambda, Cloudflare Workers, Deno Deploy, etc.)

## Your Deliverable

**Working code for this component**, honoring the contracts and criteria below, plus its configuration artifacts and tests.

## Implementation Tasks

Ordered WORK ORDERS synthesized from the model — this node's deliverable kind, contracts, criterion attribution, configuration, and dependency chain. They guarantee coverage, scope, and traceability; they deliberately do NOT contain the implementation detail — that is your job (see the expansion directive below the list).

- [ ] **T1 — Scaffold the AWS Lambda component.**
  Create the source layout, build files, and test harness this node's working code lives in.
  Start from the catalog's suggested structure: `src/handlers/index.ts`, `infra/aws/lambda.tf`.
- [ ] **T2 — Implement the integration with Secrets Manager (Auto-rotation 30d) (aws-secrets-manager) per Contract "AWS Secrets Manager (Credentials)" (custom).**
  Build to the contract schema EXACTLY (see Interface Contracts).
- [ ] **T3 — Implement the integration with CloudWatch Logs (JSON, 90d hot + Glacier) (aws-cloudwatch) per Contract "CloudWatch Logs (Structured JSON)" (custom).**
  Build to the contract schema EXACTLY (see Interface Contracts).
- [ ] **T4 — Implement the integration with AWS X-Ray (5% + 100% errors sampling) (aws-x-ray) per Contract "AWS X-Ray (Distributed Tracing)" (custom).**
  Build to the contract schema EXACTLY (see Interface Contracts).
- [ ] **T5 — Implement the integration with Amazon SES (Transactional Email) (aws-ses) per Contract "Amazon SES (Email Sending)" (custom).**
  Build to the contract schema EXACTLY (see Interface Contracts).
  ↳ serves: REQ-007 "SES is configured with a verified sending domain, DKIM, SPF, and DMARC records" — coordinate with Amazon SES (Transactional Email)
  ↳ serves: REQ-007 "Bounce and complaint notifications are handled: bounced addresses are suppressed; complaint rates above 0.1% trigger an alert" — coordinate with Amazon SES (Transactional Email)
  ↳ serves: REQ-007 "Email delivery status (sent, bounced, complained) is logged per user for audit purposes" — coordinate with Amazon SES (Transactional Email)
- [ ] **T6 — Expose the interface IAM (SSO + MFA, Scoped Roles) consumes, per Contract "IAM Role Assumption (Task/Execution Role)" (custom).**
  Record the endpoint/identifiers IAM (SSO + MFA, Scoped Roles) needs in this node's config artifacts — coordinate with IAM (SSO + MFA, Scoped Roles).
  Build to the contract schema EXACTLY (see Interface Contracts).
- [ ] **T7 — Expose the interface SQS Email Queue (FIFO, DLQ) consumes, per Contract "SQS Email Queue (Async)" (amqp).**
  Record the endpoint/identifiers SQS Email Queue (FIFO, DLQ) needs in this node's config artifacts — coordinate with SQS Email Queue (FIFO, DLQ).
  Build to the contract schema EXACTLY (see Interface Contracts).
  ↳ serves: REQ-007 "Transactional emails are queued via SQS and processed by a background worker; sending is never synchronous in the request path" — coordinate with SQS Email Queue (FIFO, DLQ)
  ↳ serves: REQ-007 "All email templates are stored as version-controlled HTML/text files with variable substitution" — coordinate with SQS Email Queue (FIFO, DLQ)
  ↳ serves: REQ-007 "Unsubscribe links are included in all marketing-adjacent emails per CAN-SPAM / GDPR" — coordinate with SQS Email Queue (FIFO, DLQ)
- [ ] **T8 — Verify every acceptance criterion above and tick its box.**
  Completion evidence flows back to the requirement criteria. This node is complete only when every criterion box is ticked and no `[PLACEHOLDER: …]` tag remains open.

**Your first action — expand these work orders.** Each task above guarantees WHAT must be covered, not HOW. Before writing any code or configuration, expand every task with the concrete implementation steps for THIS technology in THIS project — the specific resources, settings, files, schemas, and tests — using the Configuration, Interface Contracts, Technology Guidance, and node context as your references. Record the expanded list in this section via update_artifact (propose_patches) after this doc is accepted, keeping task IDs, criterion citations, and open `[PLACEHOLDER: …]` tags intact. Resolve placeholders with the user through the proposal flow; this node is never complete while one remains open.

## Requirements — Your Scope

### REQ-007: Email Notification Service
Category: functional | Status: in-progress
_Shared with: SQS Email Queue (FIFO, DLQ), Amazon SES (Transactional Email) — their slices live in their own task docs._
The application sends transactional emails (verification, password reset, invitations, alerts) via Amazon SES. Email sending is decoupled from the request path using an async queue (SQS). Templates are version-controlled. Delivery, bounce, and complaint events are tracked and acted upon.

**Acceptance criteria — your task boxes:**
- [ ] Transactional emails are queued via SQS and processed by a background worker; sending is never synchronous in the request path
  → covered by Task T7
- [ ] SES is configured with a verified sending domain, DKIM, SPF, and DMARC records
  → covered by Task T5
- [ ] Bounce and complaint notifications are handled: bounced addresses are suppressed; complaint rates above 0.1% trigger an alert
  → covered by Task T5
- [ ] All email templates are stored as version-controlled HTML/text files with variable substitution
  → covered by Task T7
- [ ] Unsubscribe links are included in all marketing-adjacent emails per CAN-SPAM / GDPR
  → covered by Task T7
- [ ] Email delivery status (sent, bounced, complained) is logged per user for audit purposes
  → covered by Task T5

## Interface Contracts

### SENDS TO: Secrets Manager (Auto-rotation 30d) (secret-manager)
- **Contract:** AWS Secrets Manager (Credentials)
- **Protocol:** custom
- **Their Technology:** aws-secrets-manager

**Schema:**
```
{
  "api": "GetSecretValue",
  "audit": "CloudTrail + alarm on unexpected principal",
  "access": "IAM policy scoped to specific secret ARNs",
  "secrets": [
    {
      "name": "app/db-credentials/{env}",
      "rotation": {
        "enabled": true,
        "mechanism": "AWS-managed Lambda",
        "intervalDays": 30
      }
    },
    {
      "name": "app/jwt-signing-key/{env}",
      "rotation": false
    },
    {
      "name": "app/oauth-client/{env}",
      "rotation": false
    }
  ],
  "encryption": "KMS CMK"
}
```

### SENDS TO: CloudWatch Logs (JSON, 90d hot + Glacier) (logging)
- **Contract:** CloudWatch Logs (Structured JSON)
- **Protocol:** custom
- **Their Technology:** aws-cloudwatch

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

### SENDS TO: AWS X-Ray (5% + 100% errors sampling) (monitoring)
- **Contract:** AWS X-Ray (Distributed Tracing)
- **Protocol:** custom
- **Their Technology:** aws-x-ray

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

### RECEIVES FROM: IAM (SSO + MFA, Scoped Roles) (auth-provider)
- **Contract:** IAM Role Assumption (Task/Execution Role)
- **Protocol:** custom
- **Their Technology:** aws-iam

**Schema:**
```
{
  "roles": [
    {
      "name": "ecs-task-role-{env}",
      "trust": "ecs-tasks.amazonaws.com",
      "permissions": [
        "secretsmanager:GetSecretValue (app/* ARNs)",
        "s3:PutObject,GetObject (user-uploads bucket)",
        "sqs:SendMessage (email queue ARN)",
        "xray:PutTraceSegments,PutTelemetryRecords",
        "logs:CreateLogStream,PutLogEvents"
      ]
    },
    {
      "name": "lambda-email-role-{env}",
      "trust": "lambda.amazonaws.com",
      "permissions": [
        "ses:SendEmail,SendTemplatedEmail",
        "secretsmanager:GetSecretValue (ses-credentials ARN)",
        "sqs:ReceiveMessage,DeleteMessage,GetQueueAttributes",
        "xray:PutTraceSegments",
        "logs:CreateLogStream,PutLogEvents"
      ]
    }
  ],
  "governance": {
    "rootAlert": "CloudWatch + CloudTrail on root login",
    "accessAnalyzer": "enabled all regions"
  },
  "humanAccess": {
    "mfa": "required",
    "provider": "IAM Identity Center (SSO)",
    "breakGlass": "single IAM user with MFA, alerts on use"
  }
}
```

### SENDS TO: Amazon SES (Transactional Email) (external-service)
- **Contract:** Amazon SES (Email Sending)
- **Protocol:** custom
- **Their Technology:** aws-ses

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

### RECEIVES FROM: SQS Email Queue (FIFO, DLQ) (queue)
- **Contract:** SQS Email Queue (Async)
- **Protocol:** amqp
- **Their Technology:** aws-sqs

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

**Purpose:** Serverless compute service that runs code in response to events and automatically manages the underlying compute resources. Supports Node.js, Python, Java, Go, .NET, Ruby, and custom runtimes.

**Best Practices:**
- Keep functions small and single-purpose
- Use environment variables for configuration
- Implement proper error handling and dead letter queues
- Use Lambda Layers for shared dependencies
- Set appropriate memory allocation for cost/performance balance
- Use provisioned concurrency for latency-sensitive workloads
- Use Step Functions for orchestrating multiple Lambda invocations
- Monitor with CloudWatch and X-Ray tracing

**Anti-Patterns to Avoid:**
- Monolithic Lambda functions doing too many things
- Not setting appropriate memory and timeout limits
- Storing state inside the function
- Not handling cold starts
- Synchronous invocation chains across multiple Lambdas
- Ignoring concurrency limits

**Suggested File Structure:**
- `src/handlers/index.ts` (source)
- `infra/aws/lambda.tf` (config)

## Dependency Chain

Startup/initialization order based on edge directions and interaction patterns.

**Must be available BEFORE this node starts:**
- Secrets Manager (Auto-rotation 30d) (this node calls/depends on it via AWS Secrets Manager (Credentials) (custom))
- CloudWatch Logs (JSON, 90d hot + Glacier) (this node calls/depends on it via CloudWatch Logs (Structured JSON) (custom))
- AWS X-Ray (5% + 100% errors sampling) (this node calls/depends on it via AWS X-Ray (Distributed Tracing) (custom))
- Amazon SES (Transactional Email) (this node calls/depends on it via Amazon SES (Email Sending) (custom))

**Depends on THIS node being available:**
- IAM (SSO + MFA, Scoped Roles) (initiates IAM Role Assumption (Task/Execution Role) against this node (custom))

## Error Handling Contracts

**Errors this node MUST emit to consumers:**
- Job failure signals to SQS Email Queue (FIFO, DLQ) ("SQS Email Queue (Async)"): emit failure status with error details, support idempotent retry

**Parent Container:** AWS VPC (Private Network) (vpc)
