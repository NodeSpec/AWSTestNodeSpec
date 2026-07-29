# Test Plan: REQ-016 - Secrets & Configuration Management

## Components: Secrets Manager

## Scenarios
1. All secrets in Secrets Manager, fetched at startup via SDK
2. Auto-rotation enabled for DB credentials (30-day interval, AWS-managed Lambda)
3. Non-sensitive config in SSM Parameter Store (SecureString/String)
4. Container images scanned for baked-in secrets (Dockerfile detection in CI)
5. IAM task roles scoped to specific secret ARNs (GetSecretValue only)
6. Pre-commit hook (git-secrets/truffleHog) runs in CI
7. CloudTrail alert on unexpected GetSecretValue caller

## Contract Tests
- Verify secret names match app/db-credentials/{env}, app/jwt-signing-key/{env}, app/oauth-client/{env}
- Verify rotation config: enabled=true, intervalDays=30 for DB creds
- Verify IAM policy scopes to specific ARNs (no wildcard)
- Verify CloudTrail + alarm integration for unexpected principal
