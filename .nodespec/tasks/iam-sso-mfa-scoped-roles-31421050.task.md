# Task: IAM (SSO + MFA, Scoped Roles)

> **Scope:** implement ONLY this node ("IAM (SSO + MFA, Scoped Roles)"). Work belonging to other nodes appears here solely as interfaces and coordination points — do not implement or re-derive it.
> This document is DERIVED from the NodeSpec model + catalog (fingerprinted, regenerable via generate_task_docs). Node context/export is the model truth; propose model changes through the proposal flow — hand-edits to model facts here do not change the model.

## Component Purpose

**Role:** Auth Provider
**Technology:** AWS IAM
**Description:** Authentication and identity management service

## Your Deliverable

This is a provider-managed service — no application code implements it.
- **Provisioning configuration (IaC)** — declare the service as config artifacts: existence, sizing, wiring, permissions. The IaC tool is NOT declared on this project's platform container — CONFIRM the tool with the user (Terraform / OpenTofu / Pulumi / provider-native / CDK) before authoring artifacts; do NOT assume one.
- **Connection contracts** for every interface below

## Implementation Tasks

Ordered WORK ORDERS synthesized from the model — this node's deliverable kind, contracts, criterion attribution, configuration, and dependency chain. They guarantee coverage, scope, and traceability; they deliberately do NOT contain the implementation detail — that is your job (see the expansion directive below the list).

- [ ] **T1 — Provision AWS IAM via IaC.**
  Author the provisioning definition as config artifacts bound to this node — existence, wiring, permissions, deployed under AWS Cloud Platform.
  [PLACEHOLDER: config — no user configuration recorded for this node; confirm sizing/domains/settings with the user]
- [ ] **T2 — Declare the wiring to ECS Fargate Backend (Multi-AZ) (aws-fargate) per Contract "IAM Role Assumption (Task/Execution Role)" (custom).**
  Build to the contract schema EXACTLY (see Interface Contracts).
  ↳ serves (unverified match): REQ-017 "No IAM user has an access key for human/interactive use; all human access is via AWS IAM Identity Center (SSO) with MFA enforced" — requirement not mapped to that node; verify or reassign before relying on it
  ↳ serves (unverified match): REQ-017 "All IAM policies use specific Actions and Resources; no policy grants *, *, or AdministratorAccess outside a break-glass role" — requirement not mapped to that node; verify or reassign before relying on it
  ↳ serves (unverified match): REQ-017 "IAM Access Analyzer is enabled in all regions; any externally accessible resource finding triggers an alert within 1 hour" — requirement not mapped to that node; verify or reassign before relying on it
  ↳ serves (unverified match): REQ-017 "ECS task roles and Lambda execution roles are scoped to the exact S3 buckets, SQS queues, and Secrets Manager ARNs they access" — requirement not mapped to that node; verify or reassign before relying on it
- [ ] **T3 — Declare the wiring to Lambda Email Worker (SQS → SES) (aws-lambda) per Contract "IAM Role Assumption (Task/Execution Role)" (custom).**
  Build to the contract schema EXACTLY (see Interface Contracts).
- [ ] **T4 — Configure the service to satisfy: "AWS Config is enabled with a conformance pack covering CIS AWS Foundations Benchmark Level 1" (REQ-017).**
  No interface contract maps to this criterion — it is this node's internal responsibility.
  ↳ serves: REQ-017 "AWS Config is enabled with a conformance pack covering CIS AWS Foundations Benchmark Level 1"
- [ ] **T5 — Configure the service to satisfy: "Security Hub is enabled with the AWS Foundational Security Best Practices standard; findings are reviewed weekly" (REQ-017).**
  No interface contract maps to this criterion — it is this node's internal responsibility.
  ↳ serves: REQ-017 "Security Hub is enabled with the AWS Foundational Security Best Practices standard; findings are reviewed weekly"
- [ ] **T6 — Configure the service to satisfy: "A root account alert fires (CloudWatch + CloudTrail) whenever the root user signs in" (REQ-017).**
  No interface contract maps to this criterion — it is this node's internal responsibility.
  ↳ serves: REQ-017 "A root account alert fires (CloudWatch + CloudTrail) whenever the root user signs in"
- [ ] **T7 — Verify every acceptance criterion above and tick its box.**
  Completion evidence flows back to the requirement criteria. This node is complete only when every criterion box is ticked and no `[PLACEHOLDER: …]` tag remains open.

**Your first action — expand these work orders.** Each task above guarantees WHAT must be covered, not HOW. Before writing any code or configuration, expand every task with the concrete implementation steps for THIS technology in THIS project — the specific resources, settings, files, schemas, and tests — using the Configuration, Interface Contracts, Technology Guidance, and node context as your references. Record the expanded list in this section via update_artifact (propose_patches) after this doc is accepted, keeping task IDs, criterion citations, and open `[PLACEHOLDER: …]` tags intact. Resolve placeholders with the user through the proposal flow; this node is never complete while one remains open.

## Requirements — Your Scope

### REQ-017: IAM Least Privilege & AWS Account Governance
Category: technical | Status: in-progress
All AWS IAM identities (roles, users, policies) adhere to the principle of least privilege. Human access to AWS is via SSO/Identity Center with MFA; no long-lived IAM user access keys exist for human accounts. Service roles are scoped to the specific actions and resources they require. AWS Config and Security Hub continuously audit posture.

**Acceptance criteria — your task boxes:**
- [ ] No IAM user has an access key for human/interactive use; all human access is via AWS IAM Identity Center (SSO) with MFA enforced
  → covered by Task T2
- [ ] All IAM policies use specific Actions and Resources; no policy grants *, *, or AdministratorAccess outside a break-glass role
  → covered by Task T2
- [ ] IAM Access Analyzer is enabled in all regions; any externally accessible resource finding triggers an alert within 1 hour
  → covered by Task T2
- [ ] AWS Config is enabled with a conformance pack covering CIS AWS Foundations Benchmark Level 1
  → covered by Task T4
- [ ] Security Hub is enabled with the AWS Foundational Security Best Practices standard; findings are reviewed weekly
  → covered by Task T5
- [ ] ECS task roles and Lambda execution roles are scoped to the exact S3 buckets, SQS queues, and Secrets Manager ARNs they access
  → covered by Task T2
- [ ] A root account alert fires (CloudWatch + CloudTrail) whenever the root user signs in
  → covered by Task T6

## Interface Contracts

### SENDS TO: ECS Fargate Backend (Multi-AZ) (backend-service)
- **Contract:** IAM Role Assumption (Task/Execution Role)
- **Protocol:** custom
- **Their Technology:** aws-fargate

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

### SENDS TO: Lambda Email Worker (SQS → SES) (serverless-function)
- **Contract:** IAM Role Assumption (Task/Execution Role)
- **Protocol:** custom
- **Their Technology:** aws-lambda

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

## Technology Guidance

_Reference for executing the Implementation Tasks above — apply where relevant. The task list stands even where this guidance is thin._

**Purpose:** Identity and access management controlling permissions for all AWS resources

**SDK Initialization:**
```
{
  "Version": "2012-10-17",
  "Statement": [{
    "Effect": "Allow",
    "Action": ["s3:GetObject"],
    "Resource": "arn:aws:s3:::my-bucket/*"
  }]
}
```

**Best Practices:**
- Grant least privilege with narrowly scoped policies
- Use roles and temporary credentials instead of long-lived access keys
- Enforce MFA for human users
- Apply permission boundaries when delegating administration
- Audit unused permissions with Access Analyzer
- Use resource tags with condition keys for attribute-based access control

**Anti-Patterns to Avoid:**
- Using the root account for daily operations
- Attaching AdministratorAccess to application roles
- Embedding long-lived access keys in application code
- Sharing IAM users between humans and services

**Suggested File Structure:**
- `infra/aws/iam.tf` (config)

## Dependency Chain

Startup/initialization order based on edge directions and interaction patterns.

**Must be available BEFORE this node starts:**
- ECS Fargate Backend (Multi-AZ) (this node calls/depends on it via IAM Role Assumption (Task/Execution Role) (custom))
- Lambda Email Worker (SQS → SES) (this node calls/depends on it via IAM Role Assumption (Task/Execution Role) (custom))

**Parent Container:** AWS Cloud Platform (aws)
