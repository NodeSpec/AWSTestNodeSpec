# Test Plan: REQ-017 - IAM Least Privilege & AWS Account Governance

## Components: IAM (SSO + MFA, Scoped Roles)

## Scenarios
1. No IAM user access keys for humans; all via Identity Center (SSO) + MFA
2. All IAM policies use specific Actions/Resources; no *:* outside break-glass
3. IAM Access Analyzer enabled all regions; external-access findings alert within 1 hour
4. AWS Config with CIS Foundations Benchmark Level 1 conformance pack
5. Security Hub with AWS FSBP standard; weekly findings review
6. ECS/Lambda roles scoped to exact S3, SQS, Secrets Manager ARNs
7. Root account login triggers CloudWatch + CloudTrail alert

## Contract Tests
- Verify ecs-task-role-{env} trust policy and permissions match schema
- Verify lambda-email-role-{env} trust policy and permissions match schema
- Verify break-glass IAM user has MFA and usage alerts
- Verify Access Analyzer is enabled in all regions
