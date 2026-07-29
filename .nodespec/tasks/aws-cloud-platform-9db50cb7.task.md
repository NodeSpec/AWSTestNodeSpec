# Task: AWS Cloud Platform

> **Scope:** implement ONLY this node ("AWS Cloud Platform"). Work belonging to other nodes appears here solely as interfaces and coordination points — do not implement or re-derive it.
> This document is DERIVED from the NodeSpec model + catalog (fingerprinted, regenerable via generate_task_docs). Node context/export is the model truth; propose model changes through the proposal flow — hand-edits to model facts here do not change the model.

## Component Purpose

**Role:** AWS
**Technology:** Amazon Web Services
**Description:** Amazon Web Services cloud platform providing compute, storage, messaging, and database services

## Your Deliverable

This container provisions the runtime context for the components inside it — no application code implements the container itself.
- **Provisioning configuration (IaC)** — declare the service as config artifacts: existence, sizing, wiring, permissions. The IaC tool is NOT declared on this project's platform container — CONFIRM the tool with the user (Terraform / OpenTofu / Pulumi / provider-native / CDK) before authoring artifacts; do NOT assume one.
- **Connection contracts** for every interface below

## Implementation Tasks

Ordered WORK ORDERS synthesized from the model — this node's deliverable kind, contracts, criterion attribution, configuration, and dependency chain. They guarantee coverage, scope, and traceability; they deliberately do NOT contain the implementation detail — that is your job (see the expansion directive below the list).

- [ ] **T1 — Provision Amazon Web Services via IaC.**
  Author the provisioning definition as config artifacts bound to this node — existence, wiring, permissions.
  [PLACEHOLDER: config — no user configuration recorded for this node; confirm sizing/domains/settings with the user]
- [ ] **T2 — Account for every hosted component in this container's definition.**
  Hosted here: AWS WAF (CloudFront + ALB), Secrets Manager (Auto-rotation 30d), IAM (SSO + MFA, Scoped Roles), Amazon SES (Transactional Email), CloudWatch Logs (JSON, 90d hot + Glacier), AWS X-Ray (5% + 100% errors sampling), S3 User Files (Versioning, SSE-KMS), AWS VPC (Private Network), KMS Customer Managed Key (CMK), CloudFront CDN, S3 Static Assets, SNS Alarm Topic (→ PagerDuty/Slack), SQS Email Queue (FIFO, DLQ).
  Each hosted component must be represented in the provisioning definition (compose service entry / subnet placement / deployment target, as appropriate for this container).
- [ ] **T3 — Verify every acceptance criterion above and tick its box.**
  Completion evidence flows back to the requirement criteria. This node is complete only when every criterion box is ticked and no `[PLACEHOLDER: …]` tag remains open.

**Your first action — expand these work orders.** Each task above guarantees WHAT must be covered, not HOW. Before writing any code or configuration, expand every task with the concrete implementation steps for THIS technology in THIS project — the specific resources, settings, files, schemas, and tests — using the Configuration, Interface Contracts, Technology Guidance, and node context as your references. Record the expanded list in this section via update_artifact (propose_patches) after this doc is accepted, keeping task IDs, criterion citations, and open `[PLACEHOLDER: …]` tags intact. Resolve placeholders with the user through the proposal flow; this node is never complete while one remains open.

## Technology Guidance

_Reference for executing the Implementation Tasks above — apply where relevant. The task list stands even where this guidance is thin._

**Purpose:** AWS cloud provider identifier for infrastructure container nodes. Use this as the technology for cloud-project, vpc, and subnet roles when the deployment target is AWS. Individual AWS managed services (Aurora, SQS, Lambda, EC2, etc.) have their own technology entries and should be assigned to their respective leaf-role nodes, NOT to infrastructure containers.

**SDK Initialization:**
```
aws configure --profile my-project && export AWS_PROFILE=my-project && aws sts get-caller-identity
```

**Common API Patterns:**

#### Create Organization
Set up AWS Organizations for multi-account strategy
```
aws organizations create-organization --feature-set ALL && aws organizations create-account --email prod@company.com --account-name production
```

#### CloudFormation Stack
Deploy infrastructure via CloudFormation
```
aws cloudformation deploy --template-file infra.yml --stack-name my-stack --capabilities CAPABILITY_NAMED_IAM --parameter-overrides Env=prod
```

#### Cost Explorer
Query cost and usage data grouped by tag
```
aws ce get-cost-and-usage --time-period Start=2024-01-01,End=2024-02-01 --granularity MONTHLY --metrics BlendedCost --group-by Type=TAG,Key=Environment
```

**Configuration Template:**
```
# AWS Landing Zone pattern (Terraform)
provider "aws" {
  region = var.primary_region
  default_tags {
    tags = {
      Environment = var.environment
      ManagedBy   = "terraform"
    }
  }
}
resource "aws_organizations_organization" "root" {
  feature_set = "ALL"
  enabled_policy_types = ["SERVICE_CONTROL_POLICY"]
}
```

**Best Practices:**
- Use VPCs with private subnets for workloads
- Enable CloudTrail for audit logging
- Use AWS Organizations for multi-account strategy
- Tag all resources for cost allocation

**Anti-Patterns to Avoid:**
- Using a single AWS account for all environments
- Assigning IAM policies directly to users instead of roles/groups
- Running workloads in default VPCs with public IPs
- Ignoring resource tagging making cost attribution impossible
- Using long-lived access keys instead of IAM roles and temporary credentials

**Security:** Enable AWS CloudTrail in all regions for audit logging. Use AWS Organizations with SCPs to enforce guardrails. Never use root account credentials for day-to-day operations. Enable GuardDuty for threat detection. Use AWS Config rules for compliance monitoring. Enforce MFA on all IAM users. Use AWS SSO for centralized access management.

**Integration Patterns:**
- AWS Organizations + Control Tower for multi-account governance
- CloudTrail + CloudWatch for centralized logging and monitoring
- IAM Identity Center (SSO) for federated access across accounts
- AWS Config for continuous compliance evaluation
- AWS Cost Explorer + Budgets for cost governance

**Contains:**
- AWS WAF (CloudFront + ALB) [aws-waf] (waf)
- Secrets Manager (Auto-rotation 30d) [aws-secrets-manager] (secret-manager)
- IAM (SSO + MFA, Scoped Roles) [aws-iam] (auth-provider)
- Amazon SES (Transactional Email) [aws-ses] (external-service)
- CloudWatch Logs (JSON, 90d hot + Glacier) [aws-cloudwatch] (logging)
- AWS X-Ray (5% + 100% errors sampling) [aws-x-ray] (monitoring)
- S3 User Files (Versioning, SSE-KMS) [aws-s3] (object-storage)
- AWS VPC (Private Network) [aws-vpc] (vpc)
- KMS Customer Managed Key (CMK) [aws-kms] (auth-provider)
- CloudFront CDN [aws-cloudfront] (cdn)
- S3 Static Assets [aws-s3] (object-storage)
- SNS Alarm Topic (→ PagerDuty/Slack) [aws-sns] (topic)
- SQS Email Queue (FIFO, DLQ) [aws-sqs] (queue)
