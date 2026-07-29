# Task: KMS Customer Managed Key (CMK)

> **Scope:** implement ONLY this node ("KMS Customer Managed Key (CMK)"). Work belonging to other nodes appears here solely as interfaces and coordination points — do not implement or re-derive it.
> This document is DERIVED from the NodeSpec model + catalog (fingerprinted, regenerable via generate_task_docs). Node context/export is the model truth; propose model changes through the proposal flow — hand-edits to model facts here do not change the model.

## Component Purpose

**Role:** Auth Provider
**Technology:** AWS KMS
**Description:** Authentication and identity management service

## Your Deliverable

This is a provider-managed service — no application code implements it.
- **Provisioning configuration (IaC)** — declare the service as config artifacts: existence, sizing, wiring, permissions. The IaC tool is NOT declared on this project's platform container — CONFIRM the tool with the user (Terraform / OpenTofu / Pulumi / provider-native / CDK) before authoring artifacts; do NOT assume one.
- **Connection contracts** for every interface below

## Implementation Tasks

Ordered WORK ORDERS synthesized from the model — this node's deliverable kind, contracts, criterion attribution, configuration, and dependency chain. They guarantee coverage, scope, and traceability; they deliberately do NOT contain the implementation detail — that is your job (see the expansion directive below the list).

- [ ] **T1 — Provision AWS KMS via IaC.**
  Author the provisioning definition as config artifacts bound to this node — existence, wiring, permissions, deployed under AWS Cloud Platform.
  [PLACEHOLDER: config — no user configuration recorded for this node; confirm sizing/domains/settings with the user]
- [ ] **T2 — Expose the interface Secrets Manager (Auto-rotation 30d) consumes, per Contract "KMS Data Encryption (At-Rest)" (custom).**
  Record the endpoint/identifiers Secrets Manager (Auto-rotation 30d) needs in this node's config artifacts — coordinate with Secrets Manager (Auto-rotation 30d).
  Build to the contract schema EXACTLY (see Interface Contracts).
  ↳ serves: REQ-011 "All S3 buckets use SSE-KMS with a CMK; bucket-level encryption enforcement policy is applied" — coordinate with Secrets Manager (Auto-rotation 30d)
  ↳ serves: REQ-011 "CMK key rotation is enabled (annual automatic rotation by AWS KMS)" — coordinate with Secrets Manager (Auto-rotation 30d)
- [ ] **T3 — Expose the interface CloudWatch Logs (JSON, 90d hot + Glacier) consumes, per Contract "KMS Data Encryption (At-Rest)" (custom).**
  Record the endpoint/identifiers CloudWatch Logs (JSON, 90d hot + Glacier) needs in this node's config artifacts — coordinate with CloudWatch Logs (JSON, 90d hot + Glacier).
  Build to the contract schema EXACTLY (see Interface Contracts).
- [ ] **T4 — Expose the interface SQS Email Queue (FIFO, DLQ) consumes, per Contract "KMS Data Encryption (At-Rest)" (custom).**
  Record the endpoint/identifiers SQS Email Queue (FIFO, DLQ) needs in this node's config artifacts — coordinate with SQS Email Queue (FIFO, DLQ).
  Build to the contract schema EXACTLY (see Interface Contracts).
- [ ] **T5 — Expose the interface S3 User Files (Versioning, SSE-KMS) consumes, per Contract "KMS Data Encryption (At-Rest)" (custom).**
  Record the endpoint/identifiers S3 User Files (Versioning, SSE-KMS) needs in this node's config artifacts — coordinate with S3 User Files (Versioning, SSE-KMS).
  Build to the contract schema EXACTLY (see Interface Contracts).
- [ ] **T6 — Resolve ownership, then implement: "All public-facing endpoints enforce HTTPS; HTTP requests are redirected to HTTPS with HSTS (max-age ≥ 1 year)" (REQ-011).**
  [PLACEHOLDER: owner — this node or a sharing node (Secrets Manager (Auto-rotation 30d)); assign via the requirement mapping, then keep this task here or move it to the owning node's doc]
  ↳ serves: REQ-011 "All public-facing endpoints enforce HTTPS; HTTP requests are redirected to HTTPS with HSTS (max-age ≥ 1 year)"
- [ ] **T7 — Resolve ownership, then implement: "TLS 1.0 and 1.1 are disabled on CloudFront and the ALB; TLS 1.2+ is required" (REQ-011).**
  [PLACEHOLDER: owner — this node or a sharing node (Secrets Manager (Auto-rotation 30d)); assign via the requirement mapping, then keep this task here or move it to the owning node's doc]
  ↳ serves: REQ-011 "TLS 1.0 and 1.1 are disabled on CloudFront and the ALB; TLS 1.2+ is required"
- [ ] **T8 — Resolve ownership, then implement: "CloudFront uses a TLS certificate issued by ACM; certificate auto-renewal is managed by ACM" (REQ-011).**
  [PLACEHOLDER: owner — this node or a sharing node (Secrets Manager (Auto-rotation 30d)); assign via the requirement mapping, then keep this task here or move it to the owning node's doc]
  ↳ serves: REQ-011 "CloudFront uses a TLS certificate issued by ACM; certificate auto-renewal is managed by ACM"
- [ ] **T9 — Resolve ownership, then implement: "All RDS storage, automated backups, and snapshots are encrypted with a CMK" (REQ-011).**
  [PLACEHOLDER: owner — this node or a sharing node (Secrets Manager (Auto-rotation 30d)); assign via the requirement mapping, then keep this task here or move it to the owning node's doc]
  ↳ serves: REQ-011 "All RDS storage, automated backups, and snapshots are encrypted with a CMK"
- [ ] **T10 — Resolve ownership, then implement: "All SQS queues and SNS topics are encrypted with a CMK" (REQ-011).**
  [PLACEHOLDER: owner — this node or a sharing node (Secrets Manager (Auto-rotation 30d)); assign via the requirement mapping, then keep this task here or move it to the owning node's doc]
  ↳ serves: REQ-011 "All SQS queues and SNS topics are encrypted with a CMK"
- [ ] **T11 — Resolve ownership, then implement: "No secret, token, or password appears in application logs, HTTP query strings, or error messages" (REQ-011).**
  [PLACEHOLDER: owner — this node or a sharing node (Secrets Manager (Auto-rotation 30d)); assign via the requirement mapping, then keep this task here or move it to the owning node's doc]
  ↳ serves: REQ-011 "No secret, token, or password appears in application logs, HTTP query strings, or error messages"
- [ ] **T12 — Verify every acceptance criterion above and tick its box.**
  Completion evidence flows back to the requirement criteria. This node is complete only when every criterion box is ticked and no `[PLACEHOLDER: …]` tag remains open.

**Your first action — expand these work orders.** Each task above guarantees WHAT must be covered, not HOW. Before writing any code or configuration, expand every task with the concrete implementation steps for THIS technology in THIS project — the specific resources, settings, files, schemas, and tests — using the Configuration, Interface Contracts, Technology Guidance, and node context as your references. Record the expanded list in this section via update_artifact (propose_patches) after this doc is accepted, keeping task IDs, criterion citations, and open `[PLACEHOLDER: …]` tags intact. Resolve placeholders with the user through the proposal flow; this node is never complete while one remains open.

## Requirements — Your Scope

### REQ-011: Data Encryption
Category: non-functional | Status: in-progress
_Shared with: Secrets Manager (Auto-rotation 30d) — their slices live in their own task docs._
All data is encrypted in transit and at rest. TLS 1.2 is the minimum enforced at every network boundary. AWS KMS customer-managed keys (CMKs) are used for all AWS-managed encryption (S3, RDS, Secrets Manager, SQS, CloudWatch Logs). Key rotation is automatic. No secrets travel in URLs or logs.

**Acceptance criteria — your task boxes:**
- [ ] All public-facing endpoints enforce HTTPS; HTTP requests are redirected to HTTPS with HSTS (max-age ≥ 1 year)
  → covered by Task T6
- [ ] TLS 1.0 and 1.1 are disabled on CloudFront and the ALB; TLS 1.2+ is required
  → covered by Task T7
- [ ] CloudFront uses a TLS certificate issued by ACM; certificate auto-renewal is managed by ACM
  → covered by Task T8
- [ ] All RDS storage, automated backups, and snapshots are encrypted with a CMK
  → covered by Task T9
- [ ] All S3 buckets use SSE-KMS with a CMK; bucket-level encryption enforcement policy is applied
  → covered by Task T2
- [ ] All SQS queues and SNS topics are encrypted with a CMK
  → covered by Task T10
- [ ] CMK key rotation is enabled (annual automatic rotation by AWS KMS)
  → covered by Task T2
- [ ] No secret, token, or password appears in application logs, HTTP query strings, or error messages
  → covered by Task T11

## Interface Contracts

### RECEIVES FROM: Secrets Manager (Auto-rotation 30d) (secret-manager)
- **Contract:** KMS Data Encryption (At-Rest)
- **Protocol:** custom
- **Their Technology:** aws-secrets-manager

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

### RECEIVES FROM: CloudWatch Logs (JSON, 90d hot + Glacier) (logging)
- **Contract:** KMS Data Encryption (At-Rest)
- **Protocol:** custom
- **Their Technology:** aws-cloudwatch

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

### RECEIVES FROM: SQS Email Queue (FIFO, DLQ) (queue)
- **Contract:** KMS Data Encryption (At-Rest)
- **Protocol:** custom
- **Their Technology:** aws-sqs

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

### RECEIVES FROM: S3 User Files (Versioning, SSE-KMS) (object-storage)
- **Contract:** KMS Data Encryption (At-Rest)
- **Protocol:** custom
- **Their Technology:** aws-s3

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

## Technology Guidance

_Reference for executing the Implementation Tasks above — apply where relevant. The task list stands even where this guidance is thin._

**Purpose:** Managed encryption key service for creating and controlling cryptographic keys

**SDK Initialization:**
```
import { KMSClient, GenerateDataKeyCommand } from "@aws-sdk/client-kms";
const kms = new KMSClient({ region: "us-east-1" });
const { Plaintext, CiphertextBlob } = await kms.send(new GenerateDataKeyCommand({ KeyId: "alias/app-key", KeySpec: "AES_256" }));
```

**Best Practices:**
- Use customer managed keys for auditable, revocable control over data
- Enable automatic annual key rotation on symmetric keys
- Scope key policies to specific services and principals
- Use grants for temporary, revocable access
- Use envelope encryption for payloads larger than 4 KB

**Anti-Patterns to Avoid:**
- Encrypting large blobs directly instead of using envelope encryption
- Using one key for every workload and environment
- Scheduling key deletion without a recovery window plan
- Ignoring CloudTrail logs of key usage

**Suggested File Structure:**
- `infra/aws/kms.tf` (config)

## Dependency Chain

Startup/initialization order based on edge directions and interaction patterns.

**Depends on THIS node being available:**
- Secrets Manager (Auto-rotation 30d) (initiates KMS Data Encryption (At-Rest) against this node (custom))
- CloudWatch Logs (JSON, 90d hot + Glacier) (initiates KMS Data Encryption (At-Rest) against this node (custom))
- SQS Email Queue (FIFO, DLQ) (initiates KMS Data Encryption (At-Rest) against this node (custom))
- S3 User Files (Versioning, SSE-KMS) (initiates KMS Data Encryption (At-Rest) against this node (custom))

**Parent Container:** AWS Cloud Platform (aws)
