# Test Plan: REQ-006 - File & Object Storage

## Components: S3 User Files, S3 Static Assets

## Scenarios
1. Pre-signed URL upload (5-min expiry, 100MB max, content-type enforced)
2. Block Public Access enabled; no s3:GetObject to *
3. Private files via signed CloudFront URLs (1-hour validity)
4. SSE-KMS encryption with CMK on all objects
5. Object versioning + 30-day soft-delete lifecycle
6. Access logs shipped to s3-access-logs-{env}
7. 100MB file size enforced at pre-signed URL policy level

## Contract Tests
- Verify content-type whitelist (image/*, application/pdf)
- Verify KMS CMK key ID matches application key
- Verify lifecycle policy soft-delete retention = 30 days
