# Test Plan: REQ-002 - User Account & Profile Management

## Architecture Components Under Test
| Component | Role | Technology |
|-----------|------|------------|
| ECS Fargate Backend (Multi-AZ) | backend-service | aws-fargate |
| Aurora PostgreSQL (Multi-AZ, Serverless v2) | database | aws-aurora |

## Test Scenarios

### Scenario 1: Profile update with email re-verification
**Validates:** AC-REQ-002-1
- **Given:** An authenticated user
- **When:** The user updates their email address via PATCH /users/me
- **Then:** A re-verification email is queued via SQS; the old email remains active until the new one is verified

### Scenario 2: Password change requires current password
**Validates:** AC-REQ-002-2
- **Given:** An authenticated user
- **When:** The user submits a password change with an incorrect current password
- **Then:** The API returns 403; with a correct current password, the new password is validated against policy and hashed

### Scenario 3: MFA and session management
**Validates:** AC-REQ-002-3
- **Given:** An authenticated user with active sessions on multiple devices
- **When:** The user views their session list and revokes a specific session
- **Then:** The revoked session's tokens are immediately invalidated; the current session remains active

### Scenario 4: Account deletion with GDPR anonymisation
**Validates:** AC-REQ-002-4
- **Given:** An authenticated user requesting account deletion
- **When:** The user confirms deletion (re-authentication required)
- **Then:** All active tokens are invalidated; PII (email, full_name) is anonymised in Aurora within 30 days; S3 user files are soft-deleted

### Scenario 5: Admin user invitation
**Validates:** AC-REQ-002-5
- **Given:** An admin user
- **When:** The admin sends an invitation email via the management console
- **Then:** An invitation email is queued via SQS with a unique link that expires in 48 hours (configurable)

### Scenario 6: Admin suspend/delete
**Validates:** AC-REQ-002-6
- **Given:** An admin user and a target user account
- **When:** The admin suspends or deletes the target account
- **Then:** The target's status is updated in Aurora; all active tokens for the target are immediately invalidated

### Scenario 7: Token invalidation on suspension
**Validates:** AC-REQ-002-7
- **Given:** A user with active sessions whose account is suspended
- **When:** Any existing token for that user is presented to the API
- **Then:** The API returns 401 with error code ACCOUNT_SUSPENDED

## Contract Validation Tests
- [ ] Verify Aurora PII encryption (AES-256) on email and full_name columns
- [ ] Verify SQS message format for invitation and re-verification emails
- [ ] Verify Secrets Manager credential retrieval for DB access
- [ ] Verify Redis session cache invalidation on account deletion/suspension
