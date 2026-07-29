# Test Plan: REQ-007 - Email Notification Service

## Components: SQS Queue, Lambda Worker, Amazon SES

## Scenarios
1. Async email: ECS queues to SQS FIFO, Lambda sends via SES
2. Domain verification: DKIM + SPF + DMARC (p=quarantine)
3. Bounce/complaint: permanent bounce suppressed; complaint > 0.1% alerts
4. Templates: version-controlled HTML+text with Handlebars substitution
5. Unsubscribe links on marketing emails (CAN-SPAM/GDPR)
6. Delivery status logged per userId to CloudWatch

## Contract Tests
- Verify SQS FIFO message body schema
- Verify DLQ maxReceiveCount = 3
- Verify SES bounce/complaint SNS notification schema
