# Task: ECS Fargate Backend (Multi-AZ)

> **Scope:** implement ONLY this node ("ECS Fargate Backend (Multi-AZ)"). Work belonging to other nodes appears here solely as interfaces and coordination points — do not implement or re-derive it.
> This document is DERIVED from the NodeSpec model + catalog (fingerprinted, regenerable via generate_task_docs). Node context/export is the model truth; propose model changes through the proposal flow — hand-edits to model facts here do not change the model.

## Component Purpose

**Role:** Backend Service
**Technology:** AWS Fargate
**Description:** Server-side application or microservice

## Your Deliverable

This is a provider-managed service — no application code implements it.
- **Provisioning configuration (IaC)** — declare the service as config artifacts: existence, sizing, wiring, permissions. The IaC tool is NOT declared on this project's platform container — CONFIRM the tool with the user (Terraform / OpenTofu / Pulumi / provider-native / CDK) before authoring artifacts; do NOT assume one.
- **Connection contracts** for every interface below

## Implementation Tasks

Ordered WORK ORDERS synthesized from the model — this node's deliverable kind, contracts, criterion attribution, configuration, and dependency chain. They guarantee coverage, scope, and traceability; they deliberately do NOT contain the implementation detail — that is your job (see the expansion directive below the list).

- [ ] **T1 — Provision AWS Fargate via IaC.**
  Author the provisioning definition as config artifacts bound to this node — existence, wiring, permissions, deployed under AWS VPC (Private Network).
  [PLACEHOLDER: config — no user configuration recorded for this node; confirm sizing/domains/settings with the user]
- [ ] **T2 — Declare the wiring to ElastiCache Redis (Cluster Mode, Multi-AZ) (aws-elasticache) per Contract "Redis Cache (ElastiCache)" (custom).**
  Build to the contract schema EXACTLY (see Interface Contracts).
  ↳ serves: REQ-009 "Frequently read, rarely written data (e.g., configuration, lookup tables) is cached in ElastiCache Redis with appropriate TTLs" — coordinate with ElastiCache Redis (Cluster Mode, Multi-AZ)
  ↳ serves: REQ-009 "Cache-aside pattern is used; cache miss falls back to the database and populates the cache" — coordinate with ElastiCache Redis (Cluster Mode, Multi-AZ)
  ↳ serves: REQ-009 "CDN cache-hit ratio for static assets is ≥ 95% in steady state" — coordinate with ElastiCache Redis (Cluster Mode, Multi-AZ)
- [ ] **T3 — Declare the wiring to AWS X-Ray (5% + 100% errors sampling) (aws-x-ray) per Contract "AWS X-Ray (Distributed Tracing)" (custom).**
  Build to the contract schema EXACTLY (see Interface Contracts).
  ↳ serves (unverified match): REQ-005 "Database credentials are retrieved at runtime from AWS Secrets Manager and rotated automatically every 30 days" — requirement not mapped to that node; verify or reassign before relying on it
- [ ] **T4 — Declare the wiring to Aurora PostgreSQL (Multi-AZ, Serverless v2) (aws-aurora) per Contract "PostgreSQL Connection (Aurora)" (sql).**
  Build to the contract schema EXACTLY (see Interface Contracts).
  ↳ serves: REQ-005 "The primary datastore is Amazon RDS Aurora PostgreSQL Serverless v2 deployed in Multi-AZ configuration" — coordinate with Aurora PostgreSQL (Multi-AZ, Serverless v2)
  ↳ serves: REQ-005 "The application connects through a connection pooler (e.g., RDS Proxy) with a maximum pool size configured per service instance" — coordinate with Aurora PostgreSQL (Multi-AZ, Serverless v2)
- [ ] **T5 — Declare the wiring to S3 User Files (Versioning, SSE-KMS) (aws-s3) per Contract "S3 Pre-signed URLs (File Upload)" (custom).**
  Build to the contract schema EXACTLY (see Interface Contracts).
- [ ] **T6 — Declare the wiring to Secrets Manager (Auto-rotation 30d) (aws-secrets-manager) per Contract "AWS Secrets Manager (Credentials)" (custom).**
  Build to the contract schema EXACTLY (see Interface Contracts).
- [ ] **T7 — Declare the wiring to CloudWatch Logs (JSON, 90d hot + Glacier) (aws-cloudwatch) per Contract "CloudWatch Logs (Structured JSON)" (custom).**
  Build to the contract schema EXACTLY (see Interface Contracts).
- [ ] **T8 — Declare the wiring to SQS Email Queue (FIFO, DLQ) (aws-sqs) per Contract "SQS Email Queue (Async)" (amqp).**
  Build to the contract schema EXACTLY (see Interface Contracts).
  ↳ serves (unverified match): REQ-005 "PII fields (email, full name) are encrypted at the application layer before storage using AES-256" — requirement not mapped to that node; verify or reassign before relying on it
  ↳ serves (unverified match): REQ-001 "Users can register with email/password and receive a verification email before account activation" — requirement not mapped to that node; verify or reassign before relying on it
  ↳ serves (unverified match): REQ-002 "Users can update display name, avatar, and email (email change triggers re-verification)" — requirement not mapped to that node; verify or reassign before relying on it
  ↳ serves (unverified match): REQ-002 "Admins can invite new users via email link with a configurable expiry (default 48 h)" — requirement not mapped to that node; verify or reassign before relying on it
- [ ] **T9 — Expose the interface IAM (SSO + MFA, Scoped Roles) consumes, per Contract "IAM Role Assumption (Task/Execution Role)" (custom).**
  Record the endpoint/identifiers IAM (SSO + MFA, Scoped Roles) needs in this node's config artifacts — coordinate with IAM (SSO + MFA, Scoped Roles).
  Build to the contract schema EXACTLY (see Interface Contracts).
  ↳ serves (unverified match): REQ-001 "Multi-factor authentication (MFA) via TOTP is available and can be enforced per role" — requirement not mapped to that node; verify or reassign before relying on it
  ↳ serves (unverified match): REQ-001 "RBAC roles (Admin, Member, Viewer) gate API endpoints and UI features" — requirement not mapped to that node; verify or reassign before relying on it
- [ ] **T10 — Expose the interface Application Load Balancer consumes, per Contract "REST API (ALB to Backend)" (rest).**
  Record the endpoint/identifiers Application Load Balancer needs in this node's config artifacts — coordinate with Application Load Balancer.
  Build to the contract schema EXACTLY (see Interface Contracts).
  ↳ serves: REQ-003 "All API routes are prefixed with /api/v1/ and the version is negotiable via Accept header" — coordinate with Application Load Balancer
  ↳ serves: REQ-003 "An OpenAPI 3.1 specification is auto-generated and served at /api/docs" — coordinate with Application Load Balancer
  ↳ serves: REQ-009 "p95 API response time is ≤ 300 ms and p99 ≤ 800 ms under a load of 500 concurrent users" — coordinate with Application Load Balancer
- [ ] **T11 — Configure the service to satisfy: "Users can sign in via OAuth 2.0 / OIDC with at least one federated identity provider (e.g., Google)" (REQ-001).**
  No interface contract maps to this criterion — it is this node's internal responsibility.
  ↳ serves: REQ-001 "Users can sign in via OAuth 2.0 / OIDC with at least one federated identity provider (e.g., Google)"
- [ ] **T12 — Configure the service to satisfy: "Access tokens expire within 15 minutes; refresh tokens rotate on use and expire after 7 days of inactivity" (REQ-001).**
  No interface contract maps to this criterion — it is this node's internal responsibility.
  ↳ serves: REQ-001 "Access tokens expire within 15 minutes; refresh tokens rotate on use and expire after 7 days of inactivity"
- [ ] **T13 — Configure the service to satisfy: "Failed login attempts trigger exponential back-off and account lockout after 10 consecutive failures" (REQ-001).**
  No interface contract maps to this criterion — it is this node's internal responsibility.
  ↳ serves: REQ-001 "Failed login attempts trigger exponential back-off and account lockout after 10 consecutive failures"
- [ ] **T14 — Configure the service to satisfy: "All authentication events (login, logout, MFA challenge, token refresh) are written to an audit log" (REQ-001).**
  No interface contract maps to this criterion — it is this node's internal responsibility.
  ↳ serves: REQ-001 "All authentication events (login, logout, MFA challenge, token refresh) are written to an audit log"
- [ ] **T15 — Configure the service to satisfy: "Password policy enforces minimum 12 characters, complexity, and bcrypt hashing with cost factor ≥ 12" (REQ-001).**
  No interface contract maps to this criterion — it is this node's internal responsibility.
  ↳ serves: REQ-001 "Password policy enforces minimum 12 characters, complexity, and bcrypt hashing with cost factor ≥ 12"
- [ ] **T16 — Resolve ownership, then implement: "All schema changes are applied via versioned migration files (e.g., Flyway or Liquibase) checked into source control" (REQ-005).**
  [PLACEHOLDER: owner — this node or a sharing node (Aurora PostgreSQL (Multi-AZ, Serverless v2)); assign via the requirement mapping, then keep this task here or move it to the owning node's doc]
  ↳ serves: REQ-005 "All schema changes are applied via versioned migration files (e.g., Flyway or Liquibase) checked into source control"
- [ ] **T17 — Resolve ownership, then implement: "No production database credentials are stored in environment variables, source code, or configuration files" (REQ-005).**
  [PLACEHOLDER: owner — this node or a sharing node (Aurora PostgreSQL (Multi-AZ, Serverless v2)); assign via the requirement mapping, then keep this task here or move it to the owning node's doc]
  ↳ serves: REQ-005 "No production database credentials are stored in environment variables, source code, or configuration files"
- [ ] **T18 — Resolve ownership, then implement: "Point-in-time recovery (PITR) is enabled with a minimum 7-day retention window" (REQ-005).**
  [PLACEHOLDER: owner — this node or a sharing node (Aurora PostgreSQL (Multi-AZ, Serverless v2)); assign via the requirement mapping, then keep this task here or move it to the owning node's doc]
  ↳ serves: REQ-005 "Point-in-time recovery (PITR) is enabled with a minimum 7-day retention window"
- [ ] **T19 — Resolve ownership, then implement: "Users can change their password after supplying their current password" (REQ-002).**
  [PLACEHOLDER: owner — this node or a sharing node (Aurora PostgreSQL (Multi-AZ, Serverless v2)); assign via the requirement mapping, then keep this task here or move it to the owning node's doc]
  ↳ serves: REQ-002 "Users can change their password after supplying their current password"
- [ ] **T20 — Resolve ownership, then implement: "Users can enable/disable MFA and view/revoke individual active sessions" (REQ-002).**
  [PLACEHOLDER: owner — this node or a sharing node (Aurora PostgreSQL (Multi-AZ, Serverless v2)); assign via the requirement mapping, then keep this task here or move it to the owning node's doc]
  ↳ serves: REQ-002 "Users can enable/disable MFA and view/revoke individual active sessions"
- [ ] **T21 — Resolve ownership, then implement: "Users can delete their own account; deletion anonymises personal data within 30 days (GDPR)" (REQ-002).**
  [PLACEHOLDER: owner — this node or a sharing node (Aurora PostgreSQL (Multi-AZ, Serverless v2)); assign via the requirement mapping, then keep this task here or move it to the owning node's doc]
  ↳ serves: REQ-002 "Users can delete their own account; deletion anonymises personal data within 30 days (GDPR)"
- [ ] **T22 — Resolve ownership, then implement: "Admins can suspend or permanently delete any user account from the management console" (REQ-002).**
  [PLACEHOLDER: owner — this node or a sharing node (Aurora PostgreSQL (Multi-AZ, Serverless v2)); assign via the requirement mapping, then keep this task here or move it to the owning node's doc]
  ↳ serves: REQ-002 "Admins can suspend or permanently delete any user account from the management console"
- [ ] **T23 — Resolve ownership, then implement: "Account deletion or suspension immediately invalidates all active tokens for that user" (REQ-002).**
  [PLACEHOLDER: owner — this node or a sharing node (Aurora PostgreSQL (Multi-AZ, Serverless v2)); assign via the requirement mapping, then keep this task here or move it to the owning node's doc]
  ↳ serves: REQ-002 "Account deletion or suspension immediately invalidates all active tokens for that user"
- [ ] **T24 — Resolve ownership, then implement: "Unauthenticated requests to protected endpoints return HTTP 401 with a machine-readable error code" (REQ-003).**
  [PLACEHOLDER: owner — this node or a sharing node (Application Load Balancer); assign via the requirement mapping, then keep this task here or move it to the owning node's doc]
  ↳ serves: REQ-003 "Unauthenticated requests to protected endpoints return HTTP 401 with a machine-readable error code"
- [ ] **T25 — Resolve ownership, then implement: "Responses conform to a consistent JSON envelope: { data, meta, errors }" (REQ-003).**
  [PLACEHOLDER: owner — this node or a sharing node (Application Load Balancer); assign via the requirement mapping, then keep this task here or move it to the owning node's doc]
  ↳ serves: REQ-003 "Responses conform to a consistent JSON envelope: { data, meta, errors }"
- [ ] **T26 — Resolve ownership, then implement: "Rate limiting is enforced at 1000 req/min per authenticated user and 60 req/min for unauthenticated public endpoints; violations return HTTP 429 with Retry-After header" (REQ-003).**
  [PLACEHOLDER: owner — this node or a sharing node (Application Load Balancer); assign via the requirement mapping, then keep this task here or move it to the owning node's doc]
  ↳ serves: REQ-003 "Rate limiting is enforced at 1000 req/min per authenticated user and 60 req/min for unauthenticated public endpoints; violations return HTTP 429 with Retry-After header"
- [ ] **T27 — Resolve ownership, then implement: "CORS is configured to allow only explicitly whitelisted origins" (REQ-003).**
  [PLACEHOLDER: owner — this node or a sharing node (Application Load Balancer); assign via the requirement mapping, then keep this task here or move it to the owning node's doc]
  ↳ serves: REQ-003 "CORS is configured to allow only explicitly whitelisted origins"
- [ ] **T28 — Resolve ownership, then implement: "All request/response bodies exceeding 1 MB are rejected with HTTP 413" (REQ-003).**
  [PLACEHOLDER: owner — this node or a sharing node (Application Load Balancer); assign via the requirement mapping, then keep this task here or move it to the owning node's doc]
  ↳ serves: REQ-003 "All request/response bodies exceeding 1 MB are rejected with HTTP 413"
- [ ] **T29 — Resolve ownership, then implement: "The system auto-scales to handle 5× baseline traffic within 3 minutes without manual intervention" (REQ-009).**
  [PLACEHOLDER: owner — this node or a sharing node (ElastiCache Redis (Cluster Mode, Multi-AZ), Application Load Balancer); assign via the requirement mapping, then keep this task here or move it to the owning node's doc]
  ↳ serves: REQ-009 "The system auto-scales to handle 5× baseline traffic within 3 minutes without manual intervention"
- [ ] **T30 — Resolve ownership, then implement: "Database read replicas are used for read-heavy analytical queries to offload the primary writer" (REQ-009).**
  [PLACEHOLDER: owner — this node or a sharing node (ElastiCache Redis (Cluster Mode, Multi-AZ), Application Load Balancer); assign via the requirement mapping, then keep this task here or move it to the owning node's doc]
  ↳ serves: REQ-009 "Database read replicas are used for read-heavy analytical queries to offload the primary writer"
- [ ] **T31 — Resolve ownership, then implement: "A k6 or Locust load test suite is run in CI on every release candidate; builds fail if p95 exceeds the SLO threshold" (REQ-009).**
  [PLACEHOLDER: owner — this node or a sharing node (ElastiCache Redis (Cluster Mode, Multi-AZ), Application Load Balancer); assign via the requirement mapping, then keep this task here or move it to the owning node's doc]
  ↳ serves: REQ-009 "A k6 or Locust load test suite is run in CI on every release candidate; builds fail if p95 exceeds the SLO threshold"
- [ ] **T32 — Verify every acceptance criterion above and tick its box.**
  Completion evidence flows back to the requirement criteria. This node is complete only when every criterion box is ticked and no `[PLACEHOLDER: …]` tag remains open.

**Your first action — expand these work orders.** Each task above guarantees WHAT must be covered, not HOW. Before writing any code or configuration, expand every task with the concrete implementation steps for THIS technology in THIS project — the specific resources, settings, files, schemas, and tests — using the Configuration, Interface Contracts, Technology Guidance, and node context as your references. Record the expanded list in this section via update_artifact (propose_patches) after this doc is accepted, keeping task IDs, criterion citations, and open `[PLACEHOLDER: …]` tags intact. Resolve placeholders with the user through the proposal flow; this node is never complete while one remains open.

## Requirements — Your Scope

### REQ-005: Relational Data Persistence
Category: functional | Status: in-progress
_Shared with: Aurora PostgreSQL (Multi-AZ, Serverless v2) — their slices live in their own task docs._
All application state is persisted in a managed relational database (Amazon RDS Aurora PostgreSQL). The schema is managed via versioned migrations. The application uses connection pooling and never opens unbounded connections. Sensitive fields (PII) are encrypted at the column level where additional protection beyond storage encryption is warranted.

**Acceptance criteria — your task boxes:**
- [ ] The primary datastore is Amazon RDS Aurora PostgreSQL Serverless v2 deployed in Multi-AZ configuration
  → covered by Task T4
- [ ] All schema changes are applied via versioned migration files (e.g., Flyway or Liquibase) checked into source control
  → covered by Task T16
- [ ] The application connects through a connection pooler (e.g., RDS Proxy) with a maximum pool size configured per service instance
  → covered by Task T4
- [ ] PII fields (email, full name) are encrypted at the application layer before storage using AES-256
  → covered by Task T8
- [ ] Database credentials are retrieved at runtime from AWS Secrets Manager and rotated automatically every 30 days
  → covered by Task T3
- [ ] No production database credentials are stored in environment variables, source code, or configuration files
  → covered by Task T17
- [ ] Point-in-time recovery (PITR) is enabled with a minimum 7-day retention window
  → covered by Task T18

### REQ-001: User Authentication & Authorization
Category: functional | Status: in-progress
The application must provide secure user authentication using industry-standard protocols. Users authenticate via email/password or federated identity providers (Google, GitHub). Sessions are managed with short-lived JWTs (15-min access tokens) and rotating refresh tokens stored in HttpOnly cookies. Role-based access control (RBAC) governs what authenticated users can see and do.

**Acceptance criteria — your task boxes:**
- [ ] Users can register with email/password and receive a verification email before account activation
  → covered by Task T8
- [ ] Users can sign in via OAuth 2.0 / OIDC with at least one federated identity provider (e.g., Google)
  → covered by Task T11
- [ ] Multi-factor authentication (MFA) via TOTP is available and can be enforced per role
  → covered by Task T9
- [ ] Access tokens expire within 15 minutes; refresh tokens rotate on use and expire after 7 days of inactivity
  → covered by Task T12
- [ ] Failed login attempts trigger exponential back-off and account lockout after 10 consecutive failures
  → covered by Task T13
- [ ] All authentication events (login, logout, MFA challenge, token refresh) are written to an audit log
  → covered by Task T14
- [ ] RBAC roles (Admin, Member, Viewer) gate API endpoints and UI features
  → covered by Task T9
- [ ] Password policy enforces minimum 12 characters, complexity, and bcrypt hashing with cost factor ≥ 12
  → covered by Task T15

### REQ-002: User Account & Profile Management
Category: functional | Status: in-progress
_Shared with: Aurora PostgreSQL (Multi-AZ, Serverless v2) — their slices live in their own task docs._
Authenticated users can manage their own account settings including profile details, email address, password, connected OAuth providers, MFA configuration, and active session list. Admins can invite, suspend, and delete users. All destructive operations require re-authentication or email confirmation.

**Acceptance criteria — your task boxes:**
- [ ] Users can update display name, avatar, and email (email change triggers re-verification)
  → covered by Task T8
- [ ] Users can change their password after supplying their current password
  → covered by Task T19
- [ ] Users can enable/disable MFA and view/revoke individual active sessions
  → covered by Task T20
- [ ] Users can delete their own account; deletion anonymises personal data within 30 days (GDPR)
  → covered by Task T21
- [ ] Admins can invite new users via email link with a configurable expiry (default 48 h)
  → covered by Task T8
- [ ] Admins can suspend or permanently delete any user account from the management console
  → covered by Task T22
- [ ] Account deletion or suspension immediately invalidates all active tokens for that user
  → covered by Task T23

### REQ-003: RESTful API Layer
Category: functional | Status: in-progress
_Shared with: Application Load Balancer — their slices live in their own task docs._
The application exposes a versioned RESTful API (v1) as the single integration surface for the frontend and any future third-party clients. All endpoints require authentication unless explicitly marked public. The API follows JSON:API or OpenAPI 3.1 conventions, returns consistent error envelopes, and enforces per-user rate limiting.

**Acceptance criteria — your task boxes:**
- [ ] All API routes are prefixed with /api/v1/ and the version is negotiable via Accept header
  → covered by Task T10
- [ ] Unauthenticated requests to protected endpoints return HTTP 401 with a machine-readable error code
  → covered by Task T24
- [ ] Responses conform to a consistent JSON envelope: { data, meta, errors }
  → covered by Task T25
- [ ] Rate limiting is enforced at 1000 req/min per authenticated user and 60 req/min for unauthenticated public endpoints; violations return HTTP 429 with Retry-After header
  → covered by Task T26
- [ ] An OpenAPI 3.1 specification is auto-generated and served at /api/docs
  → covered by Task T10
- [ ] CORS is configured to allow only explicitly whitelisted origins
  → covered by Task T27
- [ ] All request/response bodies exceeding 1 MB are rejected with HTTP 413
  → covered by Task T28

### REQ-009: Performance & Scalability
Category: non-functional | Status: in-progress
_Shared with: ElastiCache Redis (Cluster Mode, Multi-AZ), Application Load Balancer — their slices live in their own task docs._
The application must handle baseline traffic with headroom to scale elastically under load. API response times must remain within SLO thresholds at both normal and peak load. A caching layer (ElastiCache Redis) reduces database pressure for hot read paths. Performance budgets are enforced in CI.

**Acceptance criteria — your task boxes:**
- [ ] p95 API response time is ≤ 300 ms and p99 ≤ 800 ms under a load of 500 concurrent users
  → covered by Task T10
- [ ] The system auto-scales to handle 5× baseline traffic within 3 minutes without manual intervention
  → covered by Task T29
- [ ] Frequently read, rarely written data (e.g., configuration, lookup tables) is cached in ElastiCache Redis with appropriate TTLs
  → covered by Task T2
- [ ] Cache-aside pattern is used; cache miss falls back to the database and populates the cache
  → covered by Task T2
- [ ] Database read replicas are used for read-heavy analytical queries to offload the primary writer
  → covered by Task T30
- [ ] A k6 or Locust load test suite is run in CI on every release candidate; builds fail if p95 exceeds the SLO threshold
  → covered by Task T31
- [ ] CDN cache-hit ratio for static assets is ≥ 95% in steady state
  → covered by Task T2

## Interface Contracts

### SENDS TO: ElastiCache Redis (Cluster Mode, Multi-AZ) (cache)
- **Contract:** Redis Cache (ElastiCache)
- **Protocol:** custom
- **Their Technology:** aws-elasticache

**Schema:**
```
{
  "tls": true,
  "auth": "token from Secrets Manager",
  "mode": "cluster-mode, Multi-AZ, auto-failover",
  "port": 6379,
  "ttls": {
    "session": "900s",
    "lookupTable": "3600s",
    "userProfile": "300s"
  },
  "engine": "Redis 7+ on ElastiCache",
  "pattern": "cache-aside",
  "eviction": "allkeys-lru",
  "keyFormat": "app:{entity}:{id}"
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

### RECEIVES FROM: Application Load Balancer (load-balancer)
- **Contract:** REST API (ALB to Backend)
- **Protocol:** rest
- **Spec Format:** openapi
- **Their Technology:** aws-elb

**Schema:**
```
{
  "cors": "whitelist-only origins",
  "openapi": "3.1.0",
  "version": "v1",
  "basePath": "/api/v1",
  "envelope": "{data, meta, errors}",
  "security": "JWT bearer (15-min access token)",
  "bodyLimit": "1MB max (413 on exceed)",
  "endpoints": [
    "/auth/register",
    "/auth/login",
    "/auth/refresh",
    "/auth/mfa/setup",
    "/users/me",
    "/users/me/sessions",
    "/files/upload-url",
    "/health (public)"
  ],
  "rateLimits": {
    "authenticated": "1000 req/min per user",
    "unauthenticated": "60 req/min per IP"
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

### SENDS TO: Aurora PostgreSQL (Multi-AZ, Serverless v2) (database)
- **Contract:** PostgreSQL Connection (Aurora)
- **Protocol:** sql
- **Their Technology:** aws-aurora

**Schema:**
```
{
  "ssl": "require TLS 1.2+",
  "port": 5432,
  "backup": {
    "pitr": "7-day retention",
    "crossRegionCopy": true,
    "preDeploySnapshot": true
  },
  "engine": "PostgreSQL 15+ on Aurora Serverless v2",
  "pooling": "RDS Proxy or app-level, max 50/instance",
  "deployment": "Multi-AZ",
  "migrations": "Flyway, db/migrations/V{ver}__{desc}.sql",
  "credentials": "Secrets Manager at startup",
  "piiEncryption": "AES-256 app-layer (email, full_name)"
}
```

### SENDS TO: S3 User Files (Versioning, SSE-KMS) (object-storage)
- **Contract:** S3 Pre-signed URLs (File Upload)
- **Protocol:** custom
- **Their Technology:** aws-s3

**Schema:**
```
{
  "bucket": "user-uploads-{env}",
  "expiry": "300s",
  "method": "POST pre-signed URL",
  "encryption": "SSE-KMS with CMK",
  "versioning": true,
  "maxFileSize": "100MB",
  "accessLogging": "s3-access-logs-{env}",
  "blockPublicAccess": true,
  "softDeleteRetention": "30 days",
  "contentTypeWhitelist": [
    "image/*",
    "application/pdf"
  ]
}
```

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

### SENDS TO: SQS Email Queue (FIFO, DLQ) (queue)
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

## Technology Guidance

_Reference for executing the Implementation Tasks above — apply where relevant. The task list stands even where this guidance is thin._

**Purpose:** AWS Fargate is AWS's serverless compute engine for containers that runs Amazon ECS tasks and Amazon EKS pods without managing EC2 instances or cluster nodes. Use it for containerized services, background workers, batch-style tasks, and event-driven containers when you want right-sized compute and reduced infrastructure operations. Choose AWS Fargate over App Runner when you need deeper ECS or EKS integration and over EC2-backed containers when you do not want to manage host capacity. Do not use it when you need full host-level control, specialized daemon patterns, or cluster economics that favor self-managed nodes at very large scale.

**Best Practices:**
- Use Fargate for stateless containers and externalize durable state to managed backing services
- Size CPU and memory explicitly for each task or pod instead of relying on broad defaults
- Store images in trusted registries and pin production deployments by immutable image references
- Use task roles or pod execution identities instead of embedding credentials in container environments
- Separate service workloads from job-style workloads so scaling and retry behavior stay clear
- Monitor startup time, compute cost, memory pressure, and network behavior continuously
- Choose Fargate intentionally for operational simplicity rather than by default for every container workload

**Anti-Patterns to Avoid:**
- Treating Fargate tasks as long-lived pets with host-level assumptions
- Using local ephemeral storage as the system of record for business data
- Packing many unrelated services into one task definition or pod without lifecycle alignment
- Choosing Fargate when workloads require privileged host access or custom daemon behavior
- Ignoring task sizing and overpaying for container resources that do not match demand
- Assuming serverless containers eliminate the need for observability, deployment discipline, or network design

**Suggested File Structure:**
- `ecs/task-definition.json` (config)
- `infra/fargate-service.yaml` (config)
- `src/containers/entrypoint.sh` (source)

## Dependency Chain

Startup/initialization order based on edge directions and interaction patterns.

**Must be available BEFORE this node starts:**
- ElastiCache Redis (Cluster Mode, Multi-AZ) (this node calls/depends on it via Redis Cache (ElastiCache) (custom))
- AWS X-Ray (5% + 100% errors sampling) (this node calls/depends on it via AWS X-Ray (Distributed Tracing) (custom))
- Aurora PostgreSQL (Multi-AZ, Serverless v2) (this node calls/depends on it via PostgreSQL Connection (Aurora) (sql))
- S3 User Files (Versioning, SSE-KMS) (this node calls/depends on it via S3 Pre-signed URLs (File Upload) (custom))
- Secrets Manager (Auto-rotation 30d) (this node calls/depends on it via AWS Secrets Manager (Credentials) (custom))
- CloudWatch Logs (JSON, 90d hot + Glacier) (this node calls/depends on it via CloudWatch Logs (Structured JSON) (custom))

**Depends on THIS node being available:**
- IAM (SSO + MFA, Scoped Roles) (initiates IAM Role Assumption (Task/Execution Role) against this node (custom))
- Application Load Balancer (calls this node via REST API (ALB to Backend) (rest))
- SQS Email Queue (FIFO, DLQ) (consumes this node's output via SQS Email Queue (Async) (amqp))

## Error Handling Contracts

**Errors this node MUST emit to consumers:**
- HTTP error responses to Application Load Balancer ("REST API (ALB to Backend)"): return proper 4xx for validation errors, 401/403 for auth failures, 5xx for internal errors with correlation IDs

**Errors this node MUST handle from dependencies:**
- Database errors from Aurora PostgreSQL (Multi-AZ, Serverless v2) ("PostgreSQL Connection (Aurora)"): handle connection pool exhaustion, query timeout, constraint violations, and deadlocks
- Queue acknowledgment failures for SQS Email Queue (FIFO, DLQ) ("SQS Email Queue (Async)"): implement retry semantics with max-retry cap and DLQ

**Parent Container:** AWS VPC (Private Network) (vpc)
