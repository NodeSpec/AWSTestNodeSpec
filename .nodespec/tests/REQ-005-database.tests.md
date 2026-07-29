# Test Plan: REQ-005 - Relational Data Persistence

## Architecture Components Under Test
| Component | Role | Technology |
|-----------|------|------------|
| Aurora PostgreSQL (Multi-AZ, Serverless v2) | database | aws-aurora |
| ECS Fargate Backend (Multi-AZ) | backend-service | aws-fargate |

## Test Scenarios

### Scenario 1: Aurora Multi-AZ deployment
**Validates:** AC-REQ-005-1
- **Given:** The Aurora cluster configuration
- **When:** Terraform plan is inspected
- **Then:** The cluster is Serverless v2 with Multi-AZ enabled across >= 2 AZs

### Scenario 2: Versioned migrations
**Validates:** AC-REQ-005-2
- **Given:** A new schema change
- **When:** The migration file is applied via Flyway
- **Then:** The migration is versioned (V{ver}__{desc}.sql), checked into source control, and applied idempotently

### Scenario 3: Connection pooling
**Validates:** AC-REQ-005-3
- **Given:** The ECS backend connecting to Aurora
- **When:** Multiple concurrent requests arrive
- **Then:** Connections are pooled with a max of 50 per service instance; no unbounded connections are opened

### Scenario 4: PII encryption at application layer
**Validates:** AC-REQ-005-4
- **Given:** A user record with email and full_name
- **When:** The record is written to Aurora
- **Then:** The email and full_name columns contain AES-256 encrypted values; raw PII is not stored in plaintext

### Scenario 5: Secrets Manager credential retrieval
**Validates:** AC-REQ-005-5
- **Given:** The ECS backend starting up
- **When:** It needs database credentials
- **Then:** Credentials are fetched from Secrets Manager (app/db-credentials/{env}); rotation happens automatically every 30 days

### Scenario 6: No hardcoded credentials
**Validates:** AC-REQ-005-6
- **Given:** The application codebase and container image
- **When:** Scanned for credentials (git-secrets, Trivy)
- **Then:** No database passwords, connection strings, or credentials are found in env vars, source code, config files, or image layers

### Scenario 7: PITR enabled
**Validates:** AC-REQ-005-7
- **Given:** The Aurora cluster configuration
- **When:** Backup settings are inspected
- **Then:** PITR is enabled with >= 7-day retention; a snapshot is taken before each schema migration

## Contract Validation Tests
- [ ] Verify PostgreSQL connection uses TLS 1.2+ (ssl=require)
- [ ] Verify Flyway migration naming convention matches schema
- [ ] Verify Secrets Manager secret name matches app/db-credentials/{env}
- [ ] Verify KMS CMK is used for Aurora storage encryption
