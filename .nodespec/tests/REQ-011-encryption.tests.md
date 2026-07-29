# Test Plan: REQ-011 - Data Encryption

## Components: KMS CMK, Secrets Manager

## Scenarios
1. HTTPS enforced; HTTP redirects with HSTS (max-age >= 1 year)
2. TLS 1.0/1.1 disabled on CloudFront and ALB; TLS 1.2+ required
3. CloudFront TLS cert from ACM with auto-renewal
4. RDS storage, backups, snapshots encrypted with CMK
5. All S3 buckets use SSE-KMS with CMK; encryption enforcement policy
6. SQS queues and SNS topics encrypted with CMK
7. CMK annual automatic rotation enabled
8. No secrets in logs, query strings, or error messages

## Contract Tests
- Verify KMS key policy allows only listed services
- Verify envelope encryption: GenerateDataKey per object
- Verify 30-day deletion protection window
- Verify CloudTrail logs Encrypt/Decrypt/GenerateDataKey calls
