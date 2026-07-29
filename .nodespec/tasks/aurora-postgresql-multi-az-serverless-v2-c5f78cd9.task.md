# Task: Aurora PostgreSQL (Multi-AZ, Serverless v2)

> **Scope:** implement ONLY this node ("Aurora PostgreSQL (Multi-AZ, Serverless v2)"). Work belonging to other nodes appears here solely as interfaces and coordination points — do not implement or re-derive it.
> This document is DERIVED from the NodeSpec model + catalog (fingerprinted, regenerable via generate_task_docs). Node context/export is the model truth; propose model changes through the proposal flow — hand-edits to model facts here do not change the model.

## Component Purpose

**Role:** Database
**Technology:** AWS Aurora
**Description:** Persistent data storage (relational or document)

## Your Deliverable

This is a provider-managed service — no application code implements it.
- **Provisioning configuration (IaC)** — declare the service as config artifacts: existence, sizing, wiring, permissions. The IaC tool is NOT declared on this project's platform container — CONFIRM the tool with the user (Terraform / OpenTofu / Pulumi / provider-native / CDK) before authoring artifacts; do NOT assume one.
- **Connection contracts** for every interface below

## Implementation Tasks

Ordered WORK ORDERS synthesized from the model — this node's deliverable kind, contracts, criterion attribution, configuration, and dependency chain. They guarantee coverage, scope, and traceability; they deliberately do NOT contain the implementation detail — that is your job (see the expansion directive below the list).

- [ ] **T1 — Provision AWS Aurora via IaC.**
  Author the provisioning definition as config artifacts bound to this node — existence, wiring, permissions, deployed under AWS VPC (Private Network).
  [PLACEHOLDER: config — no user configuration recorded for this node; confirm sizing/domains/settings with the user]
- [ ] **T2 — Expose the interface ECS Fargate Backend (Multi-AZ) consumes, per Contract "PostgreSQL Connection (Aurora)" (sql).**
  Record the endpoint/identifiers ECS Fargate Backend (Multi-AZ) needs in this node's config artifacts — coordinate with ECS Fargate Backend (Multi-AZ).
  Build to the contract schema EXACTLY (see Interface Contracts).
  ↳ serves: REQ-005 "The primary datastore is Amazon RDS Aurora PostgreSQL Serverless v2 deployed in Multi-AZ configuration" — coordinate with ECS Fargate Backend (Multi-AZ)
  ↳ serves: REQ-005 "The application connects through a connection pooler (e.g., RDS Proxy) with a maximum pool size configured per service instance" — coordinate with ECS Fargate Backend (Multi-AZ)
- [ ] **T3 — Resolve ownership, then implement: "All schema changes are applied via versioned migration files (e.g., Flyway or Liquibase) checked into source control" (REQ-005).**
  [PLACEHOLDER: owner — this node or a sharing node (ECS Fargate Backend (Multi-AZ)); assign via the requirement mapping, then keep this task here or move it to the owning node's doc]
  ↳ serves: REQ-005 "All schema changes are applied via versioned migration files (e.g., Flyway or Liquibase) checked into source control"
- [ ] **T4 — Resolve ownership, then implement: "PII fields (email, full name) are encrypted at the application layer before storage using AES-256" (REQ-005).**
  [PLACEHOLDER: owner — this node or a sharing node (ECS Fargate Backend (Multi-AZ)); assign via the requirement mapping, then keep this task here or move it to the owning node's doc]
  ↳ serves: REQ-005 "PII fields (email, full name) are encrypted at the application layer before storage using AES-256"
- [ ] **T5 — Resolve ownership, then implement: "Database credentials are retrieved at runtime from AWS Secrets Manager and rotated automatically every 30 days" (REQ-005).**
  [PLACEHOLDER: owner — this node or a sharing node (ECS Fargate Backend (Multi-AZ)); assign via the requirement mapping, then keep this task here or move it to the owning node's doc]
  ↳ serves: REQ-005 "Database credentials are retrieved at runtime from AWS Secrets Manager and rotated automatically every 30 days"
- [ ] **T6 — Resolve ownership, then implement: "No production database credentials are stored in environment variables, source code, or configuration files" (REQ-005).**
  [PLACEHOLDER: owner — this node or a sharing node (ECS Fargate Backend (Multi-AZ)); assign via the requirement mapping, then keep this task here or move it to the owning node's doc]
  ↳ serves: REQ-005 "No production database credentials are stored in environment variables, source code, or configuration files"
- [ ] **T7 — Resolve ownership, then implement: "Point-in-time recovery (PITR) is enabled with a minimum 7-day retention window" (REQ-005).**
  [PLACEHOLDER: owner — this node or a sharing node (ECS Fargate Backend (Multi-AZ)); assign via the requirement mapping, then keep this task here or move it to the owning node's doc]
  ↳ serves: REQ-005 "Point-in-time recovery (PITR) is enabled with a minimum 7-day retention window"
- [ ] **T8 — Resolve ownership, then implement: "Users can update display name, avatar, and email (email change triggers re-verification)" (REQ-002).**
  [PLACEHOLDER: owner — this node or a sharing node (ECS Fargate Backend (Multi-AZ)); assign via the requirement mapping, then keep this task here or move it to the owning node's doc]
  ↳ serves: REQ-002 "Users can update display name, avatar, and email (email change triggers re-verification)"
- [ ] **T9 — Resolve ownership, then implement: "Users can change their password after supplying their current password" (REQ-002).**
  [PLACEHOLDER: owner — this node or a sharing node (ECS Fargate Backend (Multi-AZ)); assign via the requirement mapping, then keep this task here or move it to the owning node's doc]
  ↳ serves: REQ-002 "Users can change their password after supplying their current password"
- [ ] **T10 — Resolve ownership, then implement: "Users can enable/disable MFA and view/revoke individual active sessions" (REQ-002).**
  [PLACEHOLDER: owner — this node or a sharing node (ECS Fargate Backend (Multi-AZ)); assign via the requirement mapping, then keep this task here or move it to the owning node's doc]
  ↳ serves: REQ-002 "Users can enable/disable MFA and view/revoke individual active sessions"
- [ ] **T11 — Resolve ownership, then implement: "Users can delete their own account; deletion anonymises personal data within 30 days (GDPR)" (REQ-002).**
  [PLACEHOLDER: owner — this node or a sharing node (ECS Fargate Backend (Multi-AZ)); assign via the requirement mapping, then keep this task here or move it to the owning node's doc]
  ↳ serves: REQ-002 "Users can delete their own account; deletion anonymises personal data within 30 days (GDPR)"
- [ ] **T12 — Resolve ownership, then implement: "Admins can invite new users via email link with a configurable expiry (default 48 h)" (REQ-002).**
  [PLACEHOLDER: owner — this node or a sharing node (ECS Fargate Backend (Multi-AZ)); assign via the requirement mapping, then keep this task here or move it to the owning node's doc]
  ↳ serves: REQ-002 "Admins can invite new users via email link with a configurable expiry (default 48 h)"
- [ ] **T13 — Resolve ownership, then implement: "Admins can suspend or permanently delete any user account from the management console" (REQ-002).**
  [PLACEHOLDER: owner — this node or a sharing node (ECS Fargate Backend (Multi-AZ)); assign via the requirement mapping, then keep this task here or move it to the owning node's doc]
  ↳ serves: REQ-002 "Admins can suspend or permanently delete any user account from the management console"
- [ ] **T14 — Resolve ownership, then implement: "Account deletion or suspension immediately invalidates all active tokens for that user" (REQ-002).**
  [PLACEHOLDER: owner — this node or a sharing node (ECS Fargate Backend (Multi-AZ)); assign via the requirement mapping, then keep this task here or move it to the owning node's doc]
  ↳ serves: REQ-002 "Account deletion or suspension immediately invalidates all active tokens for that user"
- [ ] **T15 — Resolve ownership, then implement: "RPO ≤ 1 hour for the primary database; RTO ≤ 4 hours for full application restoration" (REQ-013).**
  [PLACEHOLDER: owner — this node or a sharing node (S3 User Files (Versioning, SSE-KMS)); assign via the requirement mapping, then keep this task here or move it to the owning node's doc]
  ↳ serves: REQ-013 "RPO ≤ 1 hour for the primary database; RTO ≤ 4 hours for full application restoration"
- [ ] **T16 — Resolve ownership, then implement: "RDS automated backups run daily with PITR enabled and a 7-day retention window; snapshots are also taken before any schema migration" (REQ-013).**
  [PLACEHOLDER: owner — this node or a sharing node (S3 User Files (Versioning, SSE-KMS)); assign via the requirement mapping, then keep this task here or move it to the owning node's doc]
  ↳ serves: REQ-013 "RDS automated backups run daily with PITR enabled and a 7-day retention window; snapshots are also taken before any schema migration"
- [ ] **T17 — Resolve ownership, then implement: "AWS Backup is configured to centralise and schedule backup jobs for RDS, EFS (if used), and DynamoDB (if used)" (REQ-013).**
  [PLACEHOLDER: owner — this node or a sharing node (S3 User Files (Versioning, SSE-KMS)); assign via the requirement mapping, then keep this task here or move it to the owning node's doc]
  ↳ serves: REQ-013 "AWS Backup is configured to centralise and schedule backup jobs for RDS, EFS (if used), and DynamoDB (if used)"
- [ ] **T18 — Resolve ownership, then implement: "S3 Object Versioning and MFA Delete are enabled on all user-data buckets" (REQ-013).**
  [PLACEHOLDER: owner — this node or a sharing node (S3 User Files (Versioning, SSE-KMS)); assign via the requirement mapping, then keep this task here or move it to the owning node's doc]
  ↳ serves: REQ-013 "S3 Object Versioning and MFA Delete are enabled on all user-data buckets"
- [ ] **T19 — Resolve ownership, then implement: "A cross-region backup copy of RDS snapshots is maintained in a secondary AWS region" (REQ-013).**
  [PLACEHOLDER: owner — this node or a sharing node (S3 User Files (Versioning, SSE-KMS)); assign via the requirement mapping, then keep this task here or move it to the owning node's doc]
  ↳ serves: REQ-013 "A cross-region backup copy of RDS snapshots is maintained in a secondary AWS region"
- [ ] **T20 — Resolve ownership, then implement: "Disaster recovery run-books are documented and a full recovery drill is executed at least once per quarter with results logged" (REQ-013).**
  [PLACEHOLDER: owner — this node or a sharing node (S3 User Files (Versioning, SSE-KMS)); assign via the requirement mapping, then keep this task here or move it to the owning node's doc]
  ↳ serves: REQ-013 "Disaster recovery run-books are documented and a full recovery drill is executed at least once per quarter with results logged"
- [ ] **T21 — Resolve ownership, then implement: "Infrastructure state (Terraform state) is stored in an S3 backend with versioning and DynamoDB locking" (REQ-013).**
  [PLACEHOLDER: owner — this node or a sharing node (S3 User Files (Versioning, SSE-KMS)); assign via the requirement mapping, then keep this task here or move it to the owning node's doc]
  ↳ serves: REQ-013 "Infrastructure state (Terraform state) is stored in an S3 backend with versioning and DynamoDB locking"
- [ ] **T22 — Verify every acceptance criterion above and tick its box.**
  Completion evidence flows back to the requirement criteria. This node is complete only when every criterion box is ticked and no `[PLACEHOLDER: …]` tag remains open.

**Your first action — expand these work orders.** Each task above guarantees WHAT must be covered, not HOW. Before writing any code or configuration, expand every task with the concrete implementation steps for THIS technology in THIS project — the specific resources, settings, files, schemas, and tests — using the Configuration, Interface Contracts, Technology Guidance, and node context as your references. Record the expanded list in this section via update_artifact (propose_patches) after this doc is accepted, keeping task IDs, criterion citations, and open `[PLACEHOLDER: …]` tags intact. Resolve placeholders with the user through the proposal flow; this node is never complete while one remains open.

## Requirements — Your Scope

### REQ-005: Relational Data Persistence
Category: functional | Status: in-progress
_Shared with: ECS Fargate Backend (Multi-AZ) — their slices live in their own task docs._
All application state is persisted in a managed relational database (Amazon RDS Aurora PostgreSQL). The schema is managed via versioned migrations. The application uses connection pooling and never opens unbounded connections. Sensitive fields (PII) are encrypted at the column level where additional protection beyond storage encryption is warranted.

**Acceptance criteria — your task boxes:**
- [ ] The primary datastore is Amazon RDS Aurora PostgreSQL Serverless v2 deployed in Multi-AZ configuration
  → covered by Task T2
- [ ] All schema changes are applied via versioned migration files (e.g., Flyway or Liquibase) checked into source control
  → covered by Task T3
- [ ] The application connects through a connection pooler (e.g., RDS Proxy) with a maximum pool size configured per service instance
  → covered by Task T2
- [ ] PII fields (email, full name) are encrypted at the application layer before storage using AES-256
  → covered by Task T4
- [ ] Database credentials are retrieved at runtime from AWS Secrets Manager and rotated automatically every 30 days
  → covered by Task T5
- [ ] No production database credentials are stored in environment variables, source code, or configuration files
  → covered by Task T6
- [ ] Point-in-time recovery (PITR) is enabled with a minimum 7-day retention window
  → covered by Task T7

### REQ-002: User Account & Profile Management
Category: functional | Status: in-progress
_Shared with: ECS Fargate Backend (Multi-AZ) — their slices live in their own task docs._
Authenticated users can manage their own account settings including profile details, email address, password, connected OAuth providers, MFA configuration, and active session list. Admins can invite, suspend, and delete users. All destructive operations require re-authentication or email confirmation.

**Acceptance criteria — your task boxes:**
- [ ] Users can update display name, avatar, and email (email change triggers re-verification)
  → covered by Task T8
- [ ] Users can change their password after supplying their current password
  → covered by Task T9
- [ ] Users can enable/disable MFA and view/revoke individual active sessions
  → covered by Task T10
- [ ] Users can delete their own account; deletion anonymises personal data within 30 days (GDPR)
  → covered by Task T11
- [ ] Admins can invite new users via email link with a configurable expiry (default 48 h)
  → covered by Task T12
- [ ] Admins can suspend or permanently delete any user account from the management console
  → covered by Task T13
- [ ] Account deletion or suspension immediately invalidates all active tokens for that user
  → covered by Task T14

### REQ-013: Backup & Disaster Recovery
Category: non-functional | Status: in-progress
_Shared with: S3 User Files (Versioning, SSE-KMS) — their slices live in their own task docs._
The application meets defined RPO (Recovery Point Objective) and RTO (Recovery Time Objective) targets. Automated backups cover the database, object storage versioning, and infrastructure state. Recovery procedures are documented and tested at least quarterly.

**Acceptance criteria — your task boxes:**
- [ ] RPO ≤ 1 hour for the primary database; RTO ≤ 4 hours for full application restoration
  → covered by Task T15
- [ ] RDS automated backups run daily with PITR enabled and a 7-day retention window; snapshots are also taken before any schema migration
  → covered by Task T16
- [ ] AWS Backup is configured to centralise and schedule backup jobs for RDS, EFS (if used), and DynamoDB (if used)
  → covered by Task T17
- [ ] S3 Object Versioning and MFA Delete are enabled on all user-data buckets
  → covered by Task T18
- [ ] A cross-region backup copy of RDS snapshots is maintained in a secondary AWS region
  → covered by Task T19
- [ ] Disaster recovery run-books are documented and a full recovery drill is executed at least once per quarter with results logged
  → covered by Task T20
- [ ] Infrastructure state (Terraform state) is stored in an S3 backend with versioning and DynamoDB locking
  → covered by Task T21

## Interface Contracts

### RECEIVES FROM: ECS Fargate Backend (Multi-AZ) (backend-service)
- **Contract:** PostgreSQL Connection (Aurora)
- **Protocol:** sql
- **Their Technology:** aws-fargate

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

## Technology Guidance

_Reference for executing the Implementation Tasks above — apply where relevant. The task list stands even where this guidance is thin._

**Purpose:** AWS cloud-native relational database compatible with MySQL and PostgreSQL with up to 5x MySQL / 3x PostgreSQL throughput and automatic storage scaling to 128TB. Best for AWS-centric architectures needing higher performance than standard RDS. Aurora Serverless v2 enables auto-scaling for variable workloads. Use Aurora over standard RDS when performance, auto-scaling, or global distribution matters.

**Best Practices:**
- Use Aurora Serverless v2 for variable workloads
- Route read traffic through reader endpoints
- Enable Global Database for cross-region DR
- Use Performance Insights and Enhanced Monitoring
- Use RDS Proxy for connection pooling with serverless apps
- Enable backtrack for point-in-time recovery

**Anti-Patterns to Avoid:**
- Not using reader endpoint, routing all traffic to writer
- Ignoring Performance Insights
- Choosing Aurora for simple low-traffic apps where standard RDS is cheaper

**Suggested File Structure:**
- `infra/aws/aurora.tf` (config)
- `db/migrations/` (source)

## Dependency Chain

Startup/initialization order based on edge directions and interaction patterns.

**Depends on THIS node being available:**
- ECS Fargate Backend (Multi-AZ) (initiates PostgreSQL Connection (Aurora) against this node (sql))

**Parent Container:** AWS VPC (Private Network) (vpc)
