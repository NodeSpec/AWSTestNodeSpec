# Test Plan: REQ-001 - User Authentication & Authorization

## Architecture Components Under Test
| Component | Role | Technology |
|-----------|------|------------|
| ECS Fargate Backend (Multi-AZ) | backend-service | aws-fargate |

## Test Scenarios

### Scenario 1: Registration with email verification
**Validates:** AC-REQ-001-1
- **Given:** A new user with a valid email address
- **When:** The user submits a registration form with email/password
- **Then:** A verification email is queued via SQS and the account remains inactive until the email link is clicked

### Scenario 2: OAuth 2.0 / OIDC sign-in
**Validates:** AC-REQ-001-2
- **Given:** A user with a valid Google account
- **When:** The user initiates OAuth sign-in via the /auth/login endpoint with provider=google
- **Then:** The system completes the OIDC flow, creates or links the account, and returns JWT tokens

### Scenario 3: MFA via TOTP
**Validates:** AC-REQ-001-3
- **Given:** An authenticated user who has enabled TOTP MFA
- **When:** The user logs in with correct email/password
- **Then:** The system requires a valid TOTP code before issuing access tokens; Admin role can enforce MFA for all users in a role

### Scenario 4: Token expiry and rotation
**Validates:** AC-REQ-001-4
- **Given:** A user with a valid session
- **When:** The access token expires after 15 minutes
- **Then:** The client can exchange the refresh token (HttpOnly cookie) for a new access token; the old refresh token is invalidated; refresh tokens expire after 7 days of inactivity

### Scenario 5: Brute-force protection
**Validates:** AC-REQ-001-5
- **Given:** An attacker attempting to guess a password
- **When:** 10 consecutive failed login attempts occur for the same account
- **Then:** The account is locked; each failed attempt increases delay exponentially; the account can be unlocked by an admin or via email verification

### Scenario 6: Audit logging
**Validates:** AC-REQ-001-6
- **Given:** Any authentication event occurs (login, logout, MFA challenge, token refresh)
- **When:** The event completes (success or failure)
- **Then:** A structured JSON audit log entry is written to CloudWatch with userId, eventType, timestamp, sourceIP, and outcome

### Scenario 7: RBAC enforcement
**Validates:** AC-REQ-001-7
- **Given:** Users with Admin, Member, and Viewer roles
- **When:** Each role attempts to access endpoints restricted to a higher role
- **Then:** The API returns 403 Forbidden; the frontend does not render restricted navigation items in the DOM

### Scenario 8: Password policy enforcement
**Validates:** AC-REQ-001-8
- **Given:** A user attempting to set a password
- **When:** The password is shorter than 12 characters or lacks complexity requirements
- **Then:** The API rejects the password with a descriptive error; accepted passwords are hashed with bcrypt cost factor >= 12

## Contract Validation Tests
- [ ] Verify Redis cache-aside pattern for session storage (900s TTL)
- [ ] Verify JWT tokens conform to REST API contract (bearer auth, 15-min expiry)
- [ ] Verify Secrets Manager retrieval for JWT signing key
- [ ] Verify CloudWatch log format matches structured JSON schema
- [ ] Verify X-Ray trace segments include userId annotation on auth events
