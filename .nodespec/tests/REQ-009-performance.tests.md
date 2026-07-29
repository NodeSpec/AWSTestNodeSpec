# Test Plan: REQ-009 - Performance & Scalability

## Components: ECS Fargate, ElastiCache, ALB

## Scenarios
1. p95 <= 300ms, p99 <= 800ms at 500 concurrent users (k6)
2. Auto-scale to 5x baseline within 3 min, no 5xx during scale-out
3. Cache-aside: first read from Aurora populates Redis with schema TTLs
4. Cache miss falls back to DB and populates cache
5. Read replica deferred from v1 (noted)
6. k6 load test in CI; build fails if p95 > 300ms
7. CloudFront cache-hit ratio >= 95% in steady state

## Contract Tests
- Verify Redis TTLs (session:900s, lookup:3600s, profile:300s)
- Verify Redis key format app:{entity}:{id}
- Verify CloudFront CachingOptimized for static assets
