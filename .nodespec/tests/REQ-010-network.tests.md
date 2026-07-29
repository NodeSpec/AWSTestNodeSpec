# Test Plan: REQ-010 - Network Security & Perimeter Defence

## Components: AWS WAF, ALB, VPC

## Scenarios
1. VPC: public subnets (ALB) + private subnets (compute, DB, cache) >= 2 AZs
2. No public IPs on ECS, Aurora, or ElastiCache
3. WAF: CommonRuleSet, SQLiRuleSet, KnownBadInputsRuleSet on CF + ALB
4. WAF rate-based rule blocks IPs at > 2000 req/5min
5. Security groups: no 0.0.0.0/0 inbound on compute or DB
6. All VPC internal traffic uses TLS 1.2+
7. Shield Standard enabled; Advanced evaluation documented

## Contract Tests
- Verify WAF WebACL attached to CloudFront and ALB
- Verify rate-based rule = 2000/300s
- Verify WAF logging to /aws/waf/{env}
