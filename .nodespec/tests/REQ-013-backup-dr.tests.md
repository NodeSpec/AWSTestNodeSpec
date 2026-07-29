# Test Plan: REQ-013 - Backup & Disaster Recovery

## Components: Aurora PostgreSQL, S3 User Files

## Scenarios
1. RPO <= 1 hour (DB); RTO <= 4 hours (full restoration)
2. RDS daily automated backups + PITR (7-day retention); pre-migration snapshots
3. AWS Backup centralises scheduling for RDS (and EFS/DynamoDB if used)
4. S3 Object Versioning + MFA Delete on user-data buckets
5. Cross-region RDS snapshot copy in secondary AWS region
6. DR run-books documented; quarterly recovery drill with logged results
7. Terraform state in S3 backend with versioning + DynamoDB locking

## Contract Tests
- Verify Aurora PITR retention = 7 days
- Verify cross-region snapshot copy is configured
- Verify S3 versioning enabled on user-uploads bucket
- Verify Terraform state bucket has versioning + locking
