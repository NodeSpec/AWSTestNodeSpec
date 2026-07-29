# Test Plan: REQ-012 - Observability: Logging, Metrics & Alerting

## Components: CloudWatch Logs, X-Ray, SNS Alarm Topic

## Scenarios
1. Structured JSON logs with mandatory fields (timestamp, level, traceId, service, userId, message)
2. CloudWatch Logs retention: 90 days hot, S3 Glacier after 1 year
3. X-Ray: 5% nominal sampling + 100% of errors; end-to-end tracing
4. CloudWatch dashboard: requestRate, p95/p99, errorRate, dbPool, cacheHit, alb5xx
5. Alarms fire via SNS: error rate > 1%/5min, p95 > 500ms/5min, CRITICAL log
6. CloudTrail enabled all regions with log file validation
7. Security Hub HIGH/CRITICAL findings alert within 15 minutes

## Contract Tests
- Verify log format matches mandatoryFields schema
- Verify X-Ray sampling rules (nominal 5%, errors 100%)
- Verify SNS topic subscription endpoints (PagerDuty, Slack)
- Verify CloudWatch alarm thresholds match schema
