# Task: Secrets Manager (Auto-rotation 30d)

> **Scope:** implement ONLY this node ("Secrets Manager (Auto-rotation 30d)"). Work belonging to other nodes appears here solely as interfaces and coordination points — do not implement or re-derive it.
> This document is DERIVED from the NodeSpec model + catalog (fingerprinted, regenerable via generate_task_docs). Node context/export is the model truth; propose model changes through the proposal flow — hand-edits to model facts here do not change the model.

## Component Purpose

**Role:** Secret Manager
**Technology:** AWS Secrets Manager
**Description:** Centralized secrets and key management

## Your Deliverable

This is a provider-managed service — no application code implements it.
- **Provisioning configuration (IaC)** — declare the service as config artifacts: existence, sizing, wiring, permissions. The IaC tool is NOT declared on this project's platform container — CONFIRM the tool with the user (Terraform / OpenTofu / Pulumi / provider-native / CDK) before authoring artifacts; do NOT assume one.
- **Connection contracts** for every interface below

## Implementation Tasks

Ordered WORK ORDERS synthesized from the model — this node's deliverable kind, contracts, criterion attribution, configuration, and dependency chain. They guarantee coverage, scope, and traceability; they deliberately do NOT contain the implementation detail — that is your job (see the expansion directive below the list).

- [ ] **T1 — Provision AWS Secrets Manager via IaC.**
  Author the provisioning definition as config artifacts bound to this node — existence, wiring, permissions, deployed under AWS Cloud Platform.
  [PLACEHOLDER: config — no user configuration recorded for this node; confirm sizing/domains/settings with the user]
- [ ] **T2 — Declare the wiring to KMS Customer Managed Key (CMK) (aws-kms) per Contract "KMS Data Encryption (At-Rest)" (custom).**
  Build to the contract schema EXACTLY (see Interface Contracts).
  ↳ serves: REQ-011 "All S3 buckets use SSE-KMS with a CMK; bucket-level encryption enforcement policy is applied" — coordinate with KMS Customer Managed Key (CMK)
  ↳ serves: REQ-011 "CMK key rotation is enabled (annual automatic rotation by AWS KMS)" — coordinate with KMS Customer Managed Key (CMK)
  ↳ serves (unverified match): REQ-016 "Automatic rotation is enabled for database credentials via a Lambda rotation function; rotation interval is ≤ 30 days" — requirement not mapped to that node; verify or reassign before relying on it
- [ ] **T3 — Expose the interface Lambda Email Worker (SQS → SES) consumes, per Contract "AWS Secrets Manager (Credentials)" (custom).**
  Record the endpoint/identifiers Lambda Email Worker (SQS → SES) needs in this node's config artifacts — coordinate with Lambda Email Worker (SQS → SES).
  Build to the contract schema EXACTLY (see Interface Contracts).
  ↳ serves (unverified match): REQ-016 "All secrets (DB passwords, API keys, signing keys) are stored in AWS Secrets Manager and fetched at application startup or via the AWS Secrets Manager SDK" — requirement not mapped to that node; verify or reassign before relying on it
  ↳ serves (unverified match): REQ-016 "Container images are scanned to confirm no secrets are baked into image layers (Dockerfile secrets detection in CI)" — requirement not mapped to that node; verify or reassign before relying on it
  ↳ serves (unverified match): REQ-016 "IAM task roles grant ECS/EKS tasks the minimum Secrets Manager GetSecretValue permissions required (scoped to specific secret ARNs)" — requirement not mapped to that node; verify or reassign before relying on it
  ↳ serves (unverified match): REQ-016 "A pre-commit hook (e.g., git-secrets, truffleHog) is installed and run in CI to prevent accidental secret commits" — requirement not mapped to that node; verify or reassign before relying on it
- [ ] **T4 — Expose the interface ECS Fargate Backend (Multi-AZ) consumes, per Contract "AWS Secrets Manager (Credentials)" (custom).**
  Record the endpoint/identifiers ECS Fargate Backend (Multi-AZ) needs in this node's config artifacts — coordinate with ECS Fargate Backend (Multi-AZ).
  Build to the contract schema EXACTLY (see Interface Contracts).
- [ ] **T5 — Configure the service to satisfy: "Non-sensitive configuration (feature flags, service URLs, timeouts) is stored in SSM Parameter Store as SecureString or String parameters" (REQ-016).**
  No interface contract maps to this criterion — it is this node's internal responsibility.
  ↳ serves: REQ-016 "Non-sensitive configuration (feature flags, service URLs, timeouts) is stored in SSM Parameter Store as SecureString or String parameters"
- [ ] **T6 — Configure the service to satisfy: "Secret access events are logged in CloudTrail and an alert fires on any GetSecretValue call from an unexpected principal" (REQ-016).**
  No interface contract maps to this criterion — it is this node's internal responsibility.
  ↳ serves: REQ-016 "Secret access events are logged in CloudTrail and an alert fires on any GetSecretValue call from an unexpected principal"
- [ ] **T7 — Resolve ownership, then implement: "All public-facing endpoints enforce HTTPS; HTTP requests are redirected to HTTPS with HSTS (max-age ≥ 1 year)" (REQ-011).**
  [PLACEHOLDER: owner — this node or a sharing node (KMS Customer Managed Key (CMK)); assign via the requirement mapping, then keep this task here or move it to the owning node's doc]
  ↳ serves: REQ-011 "All public-facing endpoints enforce HTTPS; HTTP requests are redirected to HTTPS with HSTS (max-age ≥ 1 year)"
- [ ] **T8 — Resolve ownership, then implement: "TLS 1.0 and 1.1 are disabled on CloudFront and the ALB; TLS 1.2+ is required" (REQ-011).**
  [PLACEHOLDER: owner — this node or a sharing node (KMS Customer Managed Key (CMK)); assign via the requirement mapping, then keep this task here or move it to the owning node's doc]
  ↳ serves: REQ-011 "TLS 1.0 and 1.1 are disabled on CloudFront and the ALB; TLS 1.2+ is required"
- [ ] **T9 — Resolve ownership, then implement: "CloudFront uses a TLS certificate issued by ACM; certificate auto-renewal is managed by ACM" (REQ-011).**
  [PLACEHOLDER: owner — this node or a sharing node (KMS Customer Managed Key (CMK)); assign via the requirement mapping, then keep this task here or move it to the owning node's doc]
  ↳ serves: REQ-011 "CloudFront uses a TLS certificate issued by ACM; certificate auto-renewal is managed by ACM"
- [ ] **T10 — Resolve ownership, then implement: "All RDS storage, automated backups, and snapshots are encrypted with a CMK" (REQ-011).**
  [PLACEHOLDER: owner — this node or a sharing node (KMS Customer Managed Key (CMK)); assign via the requirement mapping, then keep this task here or move it to the owning node's doc]
  ↳ serves: REQ-011 "All RDS storage, automated backups, and snapshots are encrypted with a CMK"
- [ ] **T11 — Resolve ownership, then implement: "All SQS queues and SNS topics are encrypted with a CMK" (REQ-011).**
  [PLACEHOLDER: owner — this node or a sharing node (KMS Customer Managed Key (CMK)); assign via the requirement mapping, then keep this task here or move it to the owning node's doc]
  ↳ serves: REQ-011 "All SQS queues and SNS topics are encrypted with a CMK"
- [ ] **T12 — Resolve ownership, then implement: "No secret, token, or password appears in application logs, HTTP query strings, or error messages" (REQ-011).**
  [PLACEHOLDER: owner — this node or a sharing node (KMS Customer Managed Key (CMK)); assign via the requirement mapping, then keep this task here or move it to the owning node's doc]
  ↳ serves: REQ-011 "No secret, token, or password appears in application logs, HTTP query strings, or error messages"
- [ ] **T13 — Verify every acceptance criterion above and tick its box.**
  Completion evidence flows back to the requirement criteria. This node is complete only when every criterion box is ticked and no `[PLACEHOLDER: …]` tag remains open.

**Your first action — expand these work orders.** Each task above guarantees WHAT must be covered, not HOW. Before writing any code or configuration, expand every task with the concrete implementation steps for THIS technology in THIS project — the specific resources, settings, files, schemas, and tests — using the Configuration, Interface Contracts, Technology Guidance, and node context as your references. Record the expanded list in this section via update_artifact (propose_patches) after this doc is accepted, keeping task IDs, criterion citations, and open `[PLACEHOLDER: …]` tags intact. Resolve placeholders with the user through the proposal flow; this node is never complete while one remains open.

## Requirements — Your Scope

### REQ-011: Data Encryption
Category: non-functional | Status: in-progress
_Shared with: KMS Customer Managed Key (CMK) — their slices live in their own task docs._
All data is encrypted in transit and at rest. TLS 1.2 is the minimum enforced at every network boundary. AWS KMS customer-managed keys (CMKs) are used for all AWS-managed encryption (S3, RDS, Secrets Manager, SQS, CloudWatch Logs). Key rotation is automatic. No secrets travel in URLs or logs.

**Acceptance criteria — your task boxes:**
- [ ] All public-facing endpoints enforce HTTPS; HTTP requests are redirected to HTTPS with HSTS (max-age ≥ 1 year)
  → covered by Task T7
- [ ] TLS 1.0 and 1.1 are disabled on CloudFront and the ALB; TLS 1.2+ is required
  → covered by Task T8
- [ ] CloudFront uses a TLS certificate issued by ACM; certificate auto-renewal is managed by ACM
  → covered by Task T9
- [ ] All RDS storage, automated backups, and snapshots are encrypted with a CMK
  → covered by Task T10
- [ ] All S3 buckets use SSE-KMS with a CMK; bucket-level encryption enforcement policy is applied
  → covered by Task T2
- [ ] All SQS queues and SNS topics are encrypted with a CMK
  → covered by Task T11
- [ ] CMK key rotation is enabled (annual automatic rotation by AWS KMS)
  → covered by Task T2
- [ ] No secret, token, or password appears in application logs, HTTP query strings, or error messages
  → covered by Task T12

### REQ-016: Secrets & Configuration Management
Category: technical | Status: in-progress
Application secrets (DB credentials, API keys, OAuth client secrets) are stored in AWS Secrets Manager with automatic rotation. Non-sensitive environment-specific configuration is stored in AWS Systems Manager Parameter Store. No secret ever resides in source code, .env files, environment variables at build time, or container images.

**Acceptance criteria — your task boxes:**
- [ ] All secrets (DB passwords, API keys, signing keys) are stored in AWS Secrets Manager and fetched at application startup or via the AWS Secrets Manager SDK
  → covered by Task T3
- [ ] Automatic rotation is enabled for database credentials via a Lambda rotation function; rotation interval is ≤ 30 days
  → covered by Task T2
- [ ] Non-sensitive configuration (feature flags, service URLs, timeouts) is stored in SSM Parameter Store as SecureString or String parameters
  → covered by Task T5
- [ ] Container images are scanned to confirm no secrets are baked into image layers (Dockerfile secrets detection in CI)
  → covered by Task T3
- [ ] IAM task roles grant ECS/EKS tasks the minimum Secrets Manager GetSecretValue permissions required (scoped to specific secret ARNs)
  → covered by Task T3
- [ ] A pre-commit hook (e.g., git-secrets, truffleHog) is installed and run in CI to prevent accidental secret commits
  → covered by Task T3
- [ ] Secret access events are logged in CloudTrail and an alert fires on any GetSecretValue call from an unexpected principal
  → covered by Task T6

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

### RECEIVES FROM: Lambda Email Worker (SQS → SES) (serverless-function)
- **Contract:** AWS Secrets Manager (Credentials)
- **Protocol:** custom
- **Their Technology:** aws-lambda

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

### RECEIVES FROM: ECS Fargate Backend (Multi-AZ) (backend-service)
- **Contract:** AWS Secrets Manager (Credentials)
- **Protocol:** custom
- **Their Technology:** aws-fargate

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

## Technology Guidance

_Reference for executing the Implementation Tasks above — apply where relevant. The task list stands even where this guidance is thin._

**Purpose:** Centralized secrets management service for rotating, managing, and retrieving database credentials, API keys, and other secrets. Supports automatic rotation with Lambda functions, fine-grained IAM access policies, and cross-account secret sharing. Integrates natively with RDS, Redshift, and DocumentDB for automatic credential rotation.

**Best Practices:**
- Enable automatic rotation for database credentials
- Use resource-based policies for cross-account access
- Reference secrets by ARN in application config rather than embedding values
- Use VPC endpoints to access Secrets Manager without traversing the internet
- Tag secrets for cost allocation and access control
- Use staging labels to manage secret versions during rotation

**Anti-Patterns to Avoid:**
- Storing secrets in environment variables, config files, or source code instead
- Not enabling automatic rotation for database credentials
- Granting overly broad IAM permissions to retrieve all secrets
- Not using VPC endpoints when accessing from private subnets

**Suggested File Structure:**
- `terraform/secrets-manager.tf` (config)
- `src/config/secrets.ts` (source)

## Dependency Chain

Startup/initialization order based on edge directions and interaction patterns.

**Must be available BEFORE this node starts:**
- KMS Customer Managed Key (CMK) (this node calls/depends on it via KMS Data Encryption (At-Rest) (custom))

**Depends on THIS node being available:**
- Lambda Email Worker (SQS → SES) (initiates AWS Secrets Manager (Credentials) against this node (custom))
- ECS Fargate Backend (Multi-AZ) (initiates AWS Secrets Manager (Credentials) against this node (custom))

**Parent Container:** AWS Cloud Platform (aws)
