# Task: S3 User Files (Versioning, SSE-KMS)

> **Scope:** implement ONLY this node ("S3 User Files (Versioning, SSE-KMS)"). Work belonging to other nodes appears here solely as interfaces and coordination points — do not implement or re-derive it.
> This document is DERIVED from the NodeSpec model + catalog (fingerprinted, regenerable via generate_task_docs). Node context/export is the model truth; propose model changes through the proposal flow — hand-edits to model facts here do not change the model.

## Component Purpose

**Role:** Object Storage
**Technology:** AWS S3
**Description:** Blob and object storage for files, images, backups, and unstructured data

## Your Deliverable

This is a provider-managed service — no application code implements it.
- **Provisioning configuration (IaC)** — declare the service as config artifacts: existence, sizing, wiring, permissions. The IaC tool is NOT declared on this project's platform container — CONFIRM the tool with the user (Terraform / OpenTofu / Pulumi / provider-native / CDK) before authoring artifacts; do NOT assume one.
- **Connection contracts** for every interface below

## Implementation Tasks

Ordered WORK ORDERS synthesized from the model — this node's deliverable kind, contracts, criterion attribution, configuration, and dependency chain. They guarantee coverage, scope, and traceability; they deliberately do NOT contain the implementation detail — that is your job (see the expansion directive below the list).

- [ ] **T1 — Provision AWS S3 via IaC.**
  Author the provisioning definition as config artifacts bound to this node — existence, wiring, permissions, deployed under AWS Cloud Platform.
  [PLACEHOLDER: config — no user configuration recorded for this node; confirm sizing/domains/settings with the user]
- [ ] **T2 — Declare the wiring to KMS Customer Managed Key (CMK) (aws-kms) per Contract "KMS Data Encryption (At-Rest)" (custom).**
  Build to the contract schema EXACTLY (see Interface Contracts).
  ↳ serves (unverified match): REQ-006 "Server-Side Encryption is enabled on the bucket using AWS KMS (SSE-KMS) with a customer-managed key" — requirement not mapped to that node; verify or reassign before relying on it
  ↳ serves (unverified match): REQ-006 "Object Versioning is enabled on buckets containing user data; deleted objects are soft-deleted via lifecycle policy for 30 days" — requirement not mapped to that node; verify or reassign before relying on it
  ↳ serves (unverified match): REQ-013 "RPO ≤ 1 hour for the primary database; RTO ≤ 4 hours for full application restoration" — requirement not mapped to that node; verify or reassign before relying on it
  ↳ serves (unverified match): REQ-013 "S3 Object Versioning and MFA Delete are enabled on all user-data buckets" — requirement not mapped to that node; verify or reassign before relying on it
- [ ] **T3 — Expose the interface ECS Fargate Backend (Multi-AZ) consumes, per Contract "S3 Pre-signed URLs (File Upload)" (custom).**
  Record the endpoint/identifiers ECS Fargate Backend (Multi-AZ) needs in this node's config artifacts — coordinate with ECS Fargate Backend (Multi-AZ).
  Build to the contract schema EXACTLY (see Interface Contracts).
  ↳ serves (unverified match): REQ-006 "File uploads use S3 pre-signed POST URLs with a maximum expiry of 5 minutes and enforced content-type and size limits" — requirement not mapped to that node; verify or reassign before relying on it
  ↳ serves (unverified match): REQ-006 "Private files are served exclusively through signed CloudFront URLs with a maximum validity of 1 hour" — requirement not mapped to that node; verify or reassign before relying on it
  ↳ serves (unverified match): REQ-006 "Maximum upload file size is enforced at 100 MB per file at the pre-signed URL policy level" — requirement not mapped to that node; verify or reassign before relying on it
- [ ] **T4 — Resolve ownership, then implement: "The S3 bucket has Block Public Access enabled; no bucket policy grants s3:GetObject to *" (REQ-006).**
  [PLACEHOLDER: owner — this node or a sharing node (S3 Static Assets); assign via the requirement mapping, then keep this task here or move it to the owning node's doc]
  ↳ serves: REQ-006 "The S3 bucket has Block Public Access enabled; no bucket policy grants s3:GetObject to *"
- [ ] **T5 — Resolve ownership, then implement: "S3 access logs are enabled and shipped to a separate logging bucket" (REQ-006).**
  [PLACEHOLDER: owner — this node or a sharing node (S3 Static Assets); assign via the requirement mapping, then keep this task here or move it to the owning node's doc]
  ↳ serves: REQ-006 "S3 access logs are enabled and shipped to a separate logging bucket"
- [ ] **T6 — Resolve ownership, then implement: "RDS automated backups run daily with PITR enabled and a 7-day retention window; snapshots are also taken before any schema migration" (REQ-013).**
  [PLACEHOLDER: owner — this node or a sharing node (Aurora PostgreSQL (Multi-AZ, Serverless v2)); assign via the requirement mapping, then keep this task here or move it to the owning node's doc]
  ↳ serves: REQ-013 "RDS automated backups run daily with PITR enabled and a 7-day retention window; snapshots are also taken before any schema migration"
- [ ] **T7 — Resolve ownership, then implement: "AWS Backup is configured to centralise and schedule backup jobs for RDS, EFS (if used), and DynamoDB (if used)" (REQ-013).**
  [PLACEHOLDER: owner — this node or a sharing node (Aurora PostgreSQL (Multi-AZ, Serverless v2)); assign via the requirement mapping, then keep this task here or move it to the owning node's doc]
  ↳ serves: REQ-013 "AWS Backup is configured to centralise and schedule backup jobs for RDS, EFS (if used), and DynamoDB (if used)"
- [ ] **T8 — Resolve ownership, then implement: "A cross-region backup copy of RDS snapshots is maintained in a secondary AWS region" (REQ-013).**
  [PLACEHOLDER: owner — this node or a sharing node (Aurora PostgreSQL (Multi-AZ, Serverless v2)); assign via the requirement mapping, then keep this task here or move it to the owning node's doc]
  ↳ serves: REQ-013 "A cross-region backup copy of RDS snapshots is maintained in a secondary AWS region"
- [ ] **T9 — Resolve ownership, then implement: "Disaster recovery run-books are documented and a full recovery drill is executed at least once per quarter with results logged" (REQ-013).**
  [PLACEHOLDER: owner — this node or a sharing node (Aurora PostgreSQL (Multi-AZ, Serverless v2)); assign via the requirement mapping, then keep this task here or move it to the owning node's doc]
  ↳ serves: REQ-013 "Disaster recovery run-books are documented and a full recovery drill is executed at least once per quarter with results logged"
- [ ] **T10 — Resolve ownership, then implement: "Infrastructure state (Terraform state) is stored in an S3 backend with versioning and DynamoDB locking" (REQ-013).**
  [PLACEHOLDER: owner — this node or a sharing node (Aurora PostgreSQL (Multi-AZ, Serverless v2)); assign via the requirement mapping, then keep this task here or move it to the owning node's doc]
  ↳ serves: REQ-013 "Infrastructure state (Terraform state) is stored in an S3 backend with versioning and DynamoDB locking"
- [ ] **T11 — Verify every acceptance criterion above and tick its box.**
  Completion evidence flows back to the requirement criteria. This node is complete only when every criterion box is ticked and no `[PLACEHOLDER: …]` tag remains open.

**Your first action — expand these work orders.** Each task above guarantees WHAT must be covered, not HOW. Before writing any code or configuration, expand every task with the concrete implementation steps for THIS technology in THIS project — the specific resources, settings, files, schemas, and tests — using the Configuration, Interface Contracts, Technology Guidance, and node context as your references. Record the expanded list in this section via update_artifact (propose_patches) after this doc is accepted, keeping task IDs, criterion citations, and open `[PLACEHOLDER: …]` tags intact. Resolve placeholders with the user through the proposal flow; this node is never complete while one remains open.

### Platform Capability Equivalence

This node is semantically equivalent to a "AWS S3" (aws-s3) platform_capability node. Treat it as the managed AWS service for spec generation, code scaffolding, and architecture decisions.
- **Equivalent Role:** aws-s3 (AWS S3)
- **Provider:** aws

## Requirements — Your Scope

### REQ-006: File & Object Storage
Category: functional | Status: in-progress
_Shared with: S3 Static Assets — their slices live in their own task docs._
User-uploaded files and application-generated assets are stored in Amazon S3. Users never upload directly to the API server; instead, pre-signed S3 URLs are issued per upload. Public assets are served via CloudFront. Private assets require a signed CloudFront URL. Bucket policies deny all direct public access.

**Acceptance criteria — your task boxes:**
- [ ] File uploads use S3 pre-signed POST URLs with a maximum expiry of 5 minutes and enforced content-type and size limits
  → covered by Task T3
- [ ] The S3 bucket has Block Public Access enabled; no bucket policy grants s3:GetObject to *
  → covered by Task T4
- [ ] Private files are served exclusively through signed CloudFront URLs with a maximum validity of 1 hour
  → covered by Task T3
- [ ] Server-Side Encryption is enabled on the bucket using AWS KMS (SSE-KMS) with a customer-managed key
  → covered by Task T2
- [ ] Object Versioning is enabled on buckets containing user data; deleted objects are soft-deleted via lifecycle policy for 30 days
  → covered by Task T2
- [ ] S3 access logs are enabled and shipped to a separate logging bucket
  → covered by Task T5
- [ ] Maximum upload file size is enforced at 100 MB per file at the pre-signed URL policy level
  → covered by Task T3

### REQ-013: Backup & Disaster Recovery
Category: non-functional | Status: in-progress
_Shared with: Aurora PostgreSQL (Multi-AZ, Serverless v2) — their slices live in their own task docs._
The application meets defined RPO (Recovery Point Objective) and RTO (Recovery Time Objective) targets. Automated backups cover the database, object storage versioning, and infrastructure state. Recovery procedures are documented and tested at least quarterly.

**Acceptance criteria — your task boxes:**
- [ ] RPO ≤ 1 hour for the primary database; RTO ≤ 4 hours for full application restoration
  → covered by Task T2
- [ ] RDS automated backups run daily with PITR enabled and a 7-day retention window; snapshots are also taken before any schema migration
  → covered by Task T6
- [ ] AWS Backup is configured to centralise and schedule backup jobs for RDS, EFS (if used), and DynamoDB (if used)
  → covered by Task T7
- [ ] S3 Object Versioning and MFA Delete are enabled on all user-data buckets
  → covered by Task T2
- [ ] A cross-region backup copy of RDS snapshots is maintained in a secondary AWS region
  → covered by Task T8
- [ ] Disaster recovery run-books are documented and a full recovery drill is executed at least once per quarter with results logged
  → covered by Task T9
- [ ] Infrastructure state (Terraform state) is stored in an S3 backend with versioning and DynamoDB locking
  → covered by Task T10

## Interface Contracts

### RECEIVES FROM: ECS Fargate Backend (Multi-AZ) (backend-service)
- **Contract:** S3 Pre-signed URLs (File Upload)
- **Protocol:** custom
- **Their Technology:** aws-fargate

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

## Technology Guidance

_Reference for executing the Implementation Tasks above — apply where relevant. The task list stands even where this guidance is thin._

**Purpose:** AWS's object storage service providing virtually unlimited, highly durable (99.999999999%) storage for any type of data. Use for static asset hosting, file uploads, data lake storage, backups, log archival, and as an origin for CloudFront CDN. S3 supports storage classes (Standard, Intelligent-Tiering, Glacier) for cost optimization across access patterns. Its event notification system integrates with Lambda, SQS, and SNS for event-driven processing. S3 is the default choice for storing files, media, and unstructured data in AWS architectures. Don't use for data requiring POSIX filesystem semantics (use EFS instead). Don't use for structured data that needs querying -- use a database. Avoid S3 Standard for archival data accessed less than once per year -- use S3 Glacier Deep Archive for massive cost savings.

**SDK Initialization:**
```
// Node.js with @aws-sdk/client-s3
import { S3Client, PutObjectCommand, GetObjectCommand } from "@aws-sdk/client-s3";
import { getSignedUrl } from "@aws-sdk/s3-request-presigner";
const s3 = new S3Client({ region: process.env.AWS_REGION });
// [Tailor to project language: Python=boto3.client("s3"), Go=s3.NewFromConfig, Java=S3Client.builder()]
```

**Common API Patterns:**

#### Upload Object
Upload file with server-side encryption
```
await s3.send(new PutObjectCommand({
  Bucket: "my-bucket",
  Key: `uploads/${userId}/${filename}`,
  Body: fileBuffer,
  ContentType: contentType,
  ServerSideEncryption: "AES256"
}));
```

#### Presigned URL
Generate time-limited presigned URL for secure download
```
const url = await getSignedUrl(s3, new GetObjectCommand({
  Bucket: "my-bucket",
  Key: `uploads/${userId}/${filename}`
}), { expiresIn: 3600 });
// Return url to client for temporary download access
```

#### List Objects
List objects by prefix with pagination
```
import { ListObjectsV2Command } from "@aws-sdk/client-s3";
const { Contents = [] } = await s3.send(new ListObjectsV2Command({
  Bucket: "my-bucket",
  Prefix: `uploads/${userId}/`,
  MaxKeys: 100
}));
```

#### Delete Object
Delete a single object
```
import { DeleteObjectCommand } from "@aws-sdk/client-s3";
await s3.send(new DeleteObjectCommand({ Bucket: "my-bucket", Key: objectKey }));
```

**Configuration Template:**
```
resource "aws_s3_bucket" "assets" {
  bucket = "${var.project}-assets"
}
resource "aws_s3_bucket_versioning" "assets" {
  bucket = aws_s3_bucket.assets.id
  versioning_configuration { status = "Enabled" }
}
resource "aws_s3_bucket_server_side_encryption_configuration" "assets" {
  bucket = aws_s3_bucket.assets.id
  rule { apply_server_side_encryption_by_default { sse_algorithm = "AES256" } }
}
resource "aws_s3_bucket_public_access_block" "assets" {
  bucket                  = aws_s3_bucket.assets.id
  block_public_acls       = true
  block_public_policy     = true
  ignore_public_acls      = true
  restrict_public_buckets = true
}
```

**Best Practices:**
- Block all public access by default and use presigned URLs for temporary access
- Enable versioning for buckets storing important data to protect against accidental deletion
- Use S3 Lifecycle rules to transition objects to cheaper storage classes automatically
- Use server-side encryption (SSE-S3 or SSE-KMS) for all buckets
- Use multipart upload for files larger than 100MB
- Enable S3 access logging or CloudTrail for audit trails
- Use intelligent-tiering for unpredictable access patterns to optimize costs automatically

**Anti-Patterns to Avoid:**
- Leaving buckets publicly accessible without a clear, justified reason
- Not enabling versioning for buckets storing user-uploaded content
- Storing all data in S3 Standard when lifecycle policies could save costs
- Using S3 for real-time database operations requiring low-latency queries
- Not encrypting buckets containing any form of user or business data
- Uploading large files without multipart upload causing timeout failures

**Security:** Enable S3 Block Public Access at the account level as a guardrail. Use bucket policies with least-privilege IAM principals. Enable server-side encryption on all buckets. Use VPC endpoints for S3 access from private subnets. Use presigned URLs for temporary access instead of making objects public. Enable MFA Delete for versioned buckets containing critical data.

**Integration Patterns:**
- CloudFront as CDN origin for global static asset delivery
- S3 Event Notifications + Lambda for serverless file processing pipelines
- S3 Lifecycle rules for automated archival to Glacier for long-term storage

**Suggested File Structure:**
- `infra/aws/s3.tf` (config)

## Manual Steps

> The following steps require manual action by a human. AI cannot complete these steps automatically.

**Quick checklist:**
- [ ] Create S3 Bucket *(required)*
- [ ] Configure Bucket Access *(required)*
- [ ] Create IAM User or Role *(required)*
- [ ] Set Environment Variables *(required)*
- [ ] Configure CORS for Browser Uploads *(optional)*

### Required Steps

#### [dashboard_config] Create S3 Bucket

In AWS Console > S3 > Create Bucket. Choose a globally unique name and region. Enable or disable versioning based on your needs.

**Reference:** https://console.aws.amazon.com/s3/

#### [permissions] Configure Bucket Access

Set the bucket to private by default. Configure a bucket policy for least-privilege access. For public assets, use CloudFront distribution instead of making the bucket public.

#### [permissions] Create IAM User or Role

Create an IAM user (for server-side) or IAM role (for AWS services) with minimum S3 permissions: s3:GetObject, s3:PutObject, s3:DeleteObject on the specific bucket ARN.

#### [environment_variable] Set Environment Variables

Add AWS credentials and bucket info to your application environment.

```bash
export AWS_ACCESS_KEY_ID=<from-iam-user>
export AWS_SECRET_ACCESS_KEY=<from-iam-user>
export AWS_S3_BUCKET_NAME=my-app-uploads
export AWS_REGION=us-east-1
```

### Optional Steps

#### [dashboard_config] Configure CORS for Browser Uploads

Under Permissions > CORS, add a CORS configuration allowing your application origin to perform PUT/POST for direct uploads from the browser.

## Dependency Chain

Startup/initialization order based on edge directions and interaction patterns.

**Must be available BEFORE this node starts:**
- KMS Customer Managed Key (CMK) (this node calls/depends on it via KMS Data Encryption (At-Rest) (custom))

**Depends on THIS node being available:**
- ECS Fargate Backend (Multi-AZ) (initiates S3 Pre-signed URLs (File Upload) against this node (custom))

**Parent Container:** AWS Cloud Platform (aws)
