# Test Plan: REQ-008 - High Availability & Fault Tolerance

## Components: ElastiCache, ECS, Aurora, ALB

## Scenarios
1. ECS tasks span >= 2 AZs at all times
2. Aurora Multi-AZ failover within 30 seconds
3. ElastiCache cluster-mode across multiple AZs
4. Auto Scaling: min 2 tasks, scale on CPU > 60% for 3 min
5. ALB health check marks unhealthy within 30s
6. 99.9% SLO alarm on error rate > 1% over 5 min
7. Chaos run-books documented and executed quarterly

## Contract Tests
- Verify ElastiCache auto-failover enabled
- Verify ECS desiredCount >= 2 with AZ-spread
- Verify ALB health check config
