# Task: Application Load Balancer

> **Scope:** implement ONLY this node ("Application Load Balancer"). Work belonging to other nodes appears here solely as interfaces and coordination points — do not implement or re-derive it.
> This document is DERIVED from the NodeSpec model + catalog (fingerprinted, regenerable via generate_task_docs). Node context/export is the model truth; propose model changes through the proposal flow — hand-edits to model facts here do not change the model.

## Component Purpose

**Role:** Load Balancer
**Technology:** AWS Elastic Load Balancing
**Description:** Traffic distribution across service instances

## Your Deliverable

This is a provider-managed service — no application code implements it.
- **Provisioning configuration (IaC)** — declare the service as config artifacts: existence, sizing, wiring, permissions. The IaC tool is NOT declared on this project's platform container — CONFIRM the tool with the user (Terraform / OpenTofu / Pulumi / provider-native / CDK) before authoring artifacts; do NOT assume one.
- **Connection contracts** for every interface below

## Implementation Tasks

Ordered WORK ORDERS synthesized from the model — this node's deliverable kind, contracts, criterion attribution, configuration, and dependency chain. They guarantee coverage, scope, and traceability; they deliberately do NOT contain the implementation detail — that is your job (see the expansion directive below the list).

- [ ] **T1 — Provision AWS Elastic Load Balancing via IaC.**
  Author the provisioning definition as config artifacts bound to this node — existence, wiring, permissions, deployed under AWS VPC (Private Network).
  [PLACEHOLDER: config — no user configuration recorded for this node; confirm sizing/domains/settings with the user]
- [ ] **T2 — Declare the wiring to ECS Fargate Backend (Multi-AZ) (aws-fargate) per Contract "REST API (ALB to Backend)" (rest).**
  Build to the contract schema EXACTLY (see Interface Contracts).
  ↳ serves (unverified match): REQ-010 "The VPC has separate public subnets (ALB, NAT Gateway) and private subnets (compute, database, cache) across ≥2 AZs" — requirement not mapped to that node; verify or reassign before relying on it
  ↳ serves: REQ-003 "All API routes are prefixed with /api/v1/ and the version is negotiable via Accept header" — coordinate with ECS Fargate Backend (Multi-AZ)
  ↳ serves: REQ-003 "An OpenAPI 3.1 specification is auto-generated and served at /api/docs" — coordinate with ECS Fargate Backend (Multi-AZ)
  ↳ serves: REQ-009 "p95 API response time is ≤ 300 ms and p99 ≤ 800 ms under a load of 500 concurrent users" — coordinate with ECS Fargate Backend (Multi-AZ)
- [ ] **T3 — Declare the wiring to AWS WAF (CloudFront + ALB) (aws-waf) per Contract "AWS WAF (Web Application Firewall)" (custom).**
  Build to the contract schema EXACTLY (see Interface Contracts).
  ↳ serves: REQ-010 "AWS WAF is attached to CloudFront and the ALB with managed rule groups: AWSManagedRulesCommonRuleSet, AWSManagedRulesSQLiRuleSet, AWSManagedRulesKnownBadInputsRuleSet" — coordinate with AWS WAF (CloudFront + ALB)
  ↳ serves: REQ-010 "WAF rate-based rule blocks IPs sending > 2000 requests per 5 minutes" — coordinate with AWS WAF (CloudFront + ALB)
  ↳ serves: REQ-010 "AWS Shield Standard is enabled; Shield Advanced is evaluated if the application is assessed as a DDoS target" — coordinate with AWS WAF (CloudFront + ALB)
- [ ] **T4 — Expose the interface CloudFront CDN consumes, per Contract "REST API (ALB to Backend)" (rest).**
  Record the endpoint/identifiers CloudFront CDN needs in this node's config artifacts — coordinate with CloudFront CDN.
  Build to the contract schema EXACTLY (see Interface Contracts).
- [ ] **T5 — Resolve ownership, then implement: "No compute instance or database has a public IP address or is directly reachable from the internet" (REQ-010).**
  [PLACEHOLDER: owner — this node or a sharing node (AWS WAF (CloudFront + ALB)); assign via the requirement mapping, then keep this task here or move it to the owning node's doc]
  ↳ serves: REQ-010 "No compute instance or database has a public IP address or is directly reachable from the internet"
- [ ] **T6 — Resolve ownership, then implement: "Security groups permit inbound traffic only on required ports from explicitly named sources (no 0.0.0.0/0 inbound on compute or DB tiers)" (REQ-010).**
  [PLACEHOLDER: owner — this node or a sharing node (AWS WAF (CloudFront + ALB)); assign via the requirement mapping, then keep this task here or move it to the owning node's doc]
  ↳ serves: REQ-010 "Security groups permit inbound traffic only on required ports from explicitly named sources (no 0.0.0.0/0 inbound on compute or DB tiers)"
- [ ] **T7 — Resolve ownership, then implement: "All inter-service traffic within the VPC uses TLS 1.2+ even on private network segments" (REQ-010).**
  [PLACEHOLDER: owner — this node or a sharing node (AWS WAF (CloudFront + ALB)); assign via the requirement mapping, then keep this task here or move it to the owning node's doc]
  ↳ serves: REQ-010 "All inter-service traffic within the VPC uses TLS 1.2+ even on private network segments"
- [ ] **T8 — Resolve ownership, then implement: "Unauthenticated requests to protected endpoints return HTTP 401 with a machine-readable error code" (REQ-003).**
  [PLACEHOLDER: owner — this node or a sharing node (ECS Fargate Backend (Multi-AZ)); assign via the requirement mapping, then keep this task here or move it to the owning node's doc]
  ↳ serves: REQ-003 "Unauthenticated requests to protected endpoints return HTTP 401 with a machine-readable error code"
- [ ] **T9 — Resolve ownership, then implement: "Responses conform to a consistent JSON envelope: { data, meta, errors }" (REQ-003).**
  [PLACEHOLDER: owner — this node or a sharing node (ECS Fargate Backend (Multi-AZ)); assign via the requirement mapping, then keep this task here or move it to the owning node's doc]
  ↳ serves: REQ-003 "Responses conform to a consistent JSON envelope: { data, meta, errors }"
- [ ] **T10 — Resolve ownership, then implement: "Rate limiting is enforced at 1000 req/min per authenticated user and 60 req/min for unauthenticated public endpoints; violations return HTTP 429 with Retry-After header" (REQ-003).**
  [PLACEHOLDER: owner — this node or a sharing node (ECS Fargate Backend (Multi-AZ)); assign via the requirement mapping, then keep this task here or move it to the owning node's doc]
  ↳ serves: REQ-003 "Rate limiting is enforced at 1000 req/min per authenticated user and 60 req/min for unauthenticated public endpoints; violations return HTTP 429 with Retry-After header"
- [ ] **T11 — Resolve ownership, then implement: "CORS is configured to allow only explicitly whitelisted origins" (REQ-003).**
  [PLACEHOLDER: owner — this node or a sharing node (ECS Fargate Backend (Multi-AZ)); assign via the requirement mapping, then keep this task here or move it to the owning node's doc]
  ↳ serves: REQ-003 "CORS is configured to allow only explicitly whitelisted origins"
- [ ] **T12 — Resolve ownership, then implement: "All request/response bodies exceeding 1 MB are rejected with HTTP 413" (REQ-003).**
  [PLACEHOLDER: owner — this node or a sharing node (ECS Fargate Backend (Multi-AZ)); assign via the requirement mapping, then keep this task here or move it to the owning node's doc]
  ↳ serves: REQ-003 "All request/response bodies exceeding 1 MB are rejected with HTTP 413"
- [ ] **T13 — Resolve ownership, then implement: "The system auto-scales to handle 5× baseline traffic within 3 minutes without manual intervention" (REQ-009).**
  [PLACEHOLDER: owner — this node or a sharing node (ElastiCache Redis (Cluster Mode, Multi-AZ), ECS Fargate Backend (Multi-AZ)); assign via the requirement mapping, then keep this task here or move it to the owning node's doc]
  ↳ serves: REQ-009 "The system auto-scales to handle 5× baseline traffic within 3 minutes without manual intervention"
- [ ] **T14 — Resolve ownership, then implement: "Frequently read, rarely written data (e.g., configuration, lookup tables) is cached in ElastiCache Redis with appropriate TTLs" (REQ-009).**
  [PLACEHOLDER: owner — this node or a sharing node (ElastiCache Redis (Cluster Mode, Multi-AZ), ECS Fargate Backend (Multi-AZ)); assign via the requirement mapping, then keep this task here or move it to the owning node's doc]
  ↳ serves: REQ-009 "Frequently read, rarely written data (e.g., configuration, lookup tables) is cached in ElastiCache Redis with appropriate TTLs"
- [ ] **T15 — Resolve ownership, then implement: "Cache-aside pattern is used; cache miss falls back to the database and populates the cache" (REQ-009).**
  [PLACEHOLDER: owner — this node or a sharing node (ElastiCache Redis (Cluster Mode, Multi-AZ), ECS Fargate Backend (Multi-AZ)); assign via the requirement mapping, then keep this task here or move it to the owning node's doc]
  ↳ serves: REQ-009 "Cache-aside pattern is used; cache miss falls back to the database and populates the cache"
- [ ] **T16 — Resolve ownership, then implement: "Database read replicas are used for read-heavy analytical queries to offload the primary writer" (REQ-009).**
  [PLACEHOLDER: owner — this node or a sharing node (ElastiCache Redis (Cluster Mode, Multi-AZ), ECS Fargate Backend (Multi-AZ)); assign via the requirement mapping, then keep this task here or move it to the owning node's doc]
  ↳ serves: REQ-009 "Database read replicas are used for read-heavy analytical queries to offload the primary writer"
- [ ] **T17 — Resolve ownership, then implement: "A k6 or Locust load test suite is run in CI on every release candidate; builds fail if p95 exceeds the SLO threshold" (REQ-009).**
  [PLACEHOLDER: owner — this node or a sharing node (ElastiCache Redis (Cluster Mode, Multi-AZ), ECS Fargate Backend (Multi-AZ)); assign via the requirement mapping, then keep this task here or move it to the owning node's doc]
  ↳ serves: REQ-009 "A k6 or Locust load test suite is run in CI on every release candidate; builds fail if p95 exceeds the SLO threshold"
- [ ] **T18 — Resolve ownership, then implement: "CDN cache-hit ratio for static assets is ≥ 95% in steady state" (REQ-009).**
  [PLACEHOLDER: owner — this node or a sharing node (ElastiCache Redis (Cluster Mode, Multi-AZ), ECS Fargate Backend (Multi-AZ)); assign via the requirement mapping, then keep this task here or move it to the owning node's doc]
  ↳ serves: REQ-009 "CDN cache-hit ratio for static assets is ≥ 95% in steady state"
- [ ] **T19 — Verify every acceptance criterion above and tick its box.**
  Completion evidence flows back to the requirement criteria. This node is complete only when every criterion box is ticked and no `[PLACEHOLDER: …]` tag remains open.

**Your first action — expand these work orders.** Each task above guarantees WHAT must be covered, not HOW. Before writing any code or configuration, expand every task with the concrete implementation steps for THIS technology in THIS project — the specific resources, settings, files, schemas, and tests — using the Configuration, Interface Contracts, Technology Guidance, and node context as your references. Record the expanded list in this section via update_artifact (propose_patches) after this doc is accepted, keeping task IDs, criterion citations, and open `[PLACEHOLDER: …]` tags intact. Resolve placeholders with the user through the proposal flow; this node is never complete while one remains open.

## Requirements — Your Scope

### REQ-010: Network Security & Perimeter Defence
Category: non-functional | Status: in-progress
_Shared with: AWS WAF (CloudFront + ALB) — their slices live in their own task docs._
All application components run inside a private VPC. Only the Application Load Balancer and CloudFront distribution are internet-facing. AWS WAF is attached to both CloudFront and the ALB. Private subnets have no direct internet route; outbound traffic egresses through a managed NAT Gateway. Security groups follow least-privilege inbound rules.

**Acceptance criteria — your task boxes:**
- [ ] The VPC has separate public subnets (ALB, NAT Gateway) and private subnets (compute, database, cache) across ≥2 AZs
  → covered by Task T2
- [ ] No compute instance or database has a public IP address or is directly reachable from the internet
  → covered by Task T5
- [ ] AWS WAF is attached to CloudFront and the ALB with managed rule groups: AWSManagedRulesCommonRuleSet, AWSManagedRulesSQLiRuleSet, AWSManagedRulesKnownBadInputsRuleSet
  → covered by Task T3
- [ ] WAF rate-based rule blocks IPs sending > 2000 requests per 5 minutes
  → covered by Task T3
- [ ] Security groups permit inbound traffic only on required ports from explicitly named sources (no 0.0.0.0/0 inbound on compute or DB tiers)
  → covered by Task T6
- [ ] All inter-service traffic within the VPC uses TLS 1.2+ even on private network segments
  → covered by Task T7
- [ ] AWS Shield Standard is enabled; Shield Advanced is evaluated if the application is assessed as a DDoS target
  → covered by Task T3

### REQ-003: RESTful API Layer
Category: functional | Status: in-progress
_Shared with: ECS Fargate Backend (Multi-AZ) — their slices live in their own task docs._
The application exposes a versioned RESTful API (v1) as the single integration surface for the frontend and any future third-party clients. All endpoints require authentication unless explicitly marked public. The API follows JSON:API or OpenAPI 3.1 conventions, returns consistent error envelopes, and enforces per-user rate limiting.

**Acceptance criteria — your task boxes:**
- [ ] All API routes are prefixed with /api/v1/ and the version is negotiable via Accept header
  → covered by Task T2
- [ ] Unauthenticated requests to protected endpoints return HTTP 401 with a machine-readable error code
  → covered by Task T8
- [ ] Responses conform to a consistent JSON envelope: { data, meta, errors }
  → covered by Task T9
- [ ] Rate limiting is enforced at 1000 req/min per authenticated user and 60 req/min for unauthenticated public endpoints; violations return HTTP 429 with Retry-After header
  → covered by Task T10
- [ ] An OpenAPI 3.1 specification is auto-generated and served at /api/docs
  → covered by Task T2
- [ ] CORS is configured to allow only explicitly whitelisted origins
  → covered by Task T11
- [ ] All request/response bodies exceeding 1 MB are rejected with HTTP 413
  → covered by Task T12

### REQ-009: Performance & Scalability
Category: non-functional | Status: in-progress
_Shared with: ElastiCache Redis (Cluster Mode, Multi-AZ), ECS Fargate Backend (Multi-AZ) — their slices live in their own task docs._
The application must handle baseline traffic with headroom to scale elastically under load. API response times must remain within SLO thresholds at both normal and peak load. A caching layer (ElastiCache Redis) reduces database pressure for hot read paths. Performance budgets are enforced in CI.

**Acceptance criteria — your task boxes:**
- [ ] p95 API response time is ≤ 300 ms and p99 ≤ 800 ms under a load of 500 concurrent users
  → covered by Task T2
- [ ] The system auto-scales to handle 5× baseline traffic within 3 minutes without manual intervention
  → covered by Task T13
- [ ] Frequently read, rarely written data (e.g., configuration, lookup tables) is cached in ElastiCache Redis with appropriate TTLs
  → covered by Task T14
- [ ] Cache-aside pattern is used; cache miss falls back to the database and populates the cache
  → covered by Task T15
- [ ] Database read replicas are used for read-heavy analytical queries to offload the primary writer
  → covered by Task T16
- [ ] A k6 or Locust load test suite is run in CI on every release candidate; builds fail if p95 exceeds the SLO threshold
  → covered by Task T17
- [ ] CDN cache-hit ratio for static assets is ≥ 95% in steady state
  → covered by Task T18

## Interface Contracts

### SENDS TO: ECS Fargate Backend (Multi-AZ) (backend-service)
- **Contract:** REST API (ALB to Backend)
- **Protocol:** rest
- **Spec Format:** openapi
- **Their Technology:** aws-fargate

**Schema:**
```
{
  "cors": "whitelist-only origins",
  "openapi": "3.1.0",
  "version": "v1",
  "basePath": "/api/v1",
  "envelope": "{data, meta, errors}",
  "security": "JWT bearer (15-min access token)",
  "bodyLimit": "1MB max (413 on exceed)",
  "endpoints": [
    "/auth/register",
    "/auth/login",
    "/auth/refresh",
    "/auth/mfa/setup",
    "/users/me",
    "/users/me/sessions",
    "/files/upload-url",
    "/health (public)"
  ],
  "rateLimits": {
    "authenticated": "1000 req/min per user",
    "unauthenticated": "60 req/min per IP"
  }
}
```

### RECEIVES FROM: CloudFront CDN (cdn)
- **Contract:** REST API (ALB to Backend)
- **Protocol:** rest
- **Spec Format:** openapi
- **Their Technology:** aws-cloudfront

**Schema:**
```
{
  "cors": "whitelist-only origins",
  "openapi": "3.1.0",
  "version": "v1",
  "basePath": "/api/v1",
  "envelope": "{data, meta, errors}",
  "security": "JWT bearer (15-min access token)",
  "bodyLimit": "1MB max (413 on exceed)",
  "endpoints": [
    "/auth/register",
    "/auth/login",
    "/auth/refresh",
    "/auth/mfa/setup",
    "/users/me",
    "/users/me/sessions",
    "/files/upload-url",
    "/health (public)"
  ],
  "rateLimits": {
    "authenticated": "1000 req/min per user",
    "unauthenticated": "60 req/min per IP"
  }
}
```

### SENDS TO: AWS WAF (CloudFront + ALB) (waf)
- **Contract:** AWS WAF (Web Application Firewall)
- **Protocol:** custom
- **Their Technology:** aws-waf

**Schema:**
```
{
  "logging": "CloudWatch Logs /aws/waf/{env}",
  "attachedTo": [
    "CloudFront distribution",
    "Application Load Balancer"
  ],
  "customRules": [
    {
      "name": "RateLimit",
      "type": "rate-based",
      "limit": 2000,
      "scope": "per source IP",
      "action": "block",
      "windowSeconds": 300
    }
  ],
  "defaultAction": "allow",
  "managedRuleGroups": [
    {
      "name": "AWSManagedRulesCommonRuleSet",
      "priority": 1
    },
    {
      "name": "AWSManagedRulesSQLiRuleSet",
      "priority": 2
    },
    {
      "name": "AWSManagedRulesKnownBadInputsRuleSet",
      "priority": 3
    }
  ]
}
```

## Technology Guidance

_Reference for executing the Implementation Tasks above — apply where relevant. The task list stands even where this guidance is thin._

**Purpose:** Automatically distributes incoming application traffic across multiple targets such as EC2 instances, containers, and IP addresses in one or more Availability Zones.

**Best Practices:**
- Use Application Load Balancer for HTTP/HTTPS traffic with path-based routing
- Use Network Load Balancer for TCP/UDP with ultra-low latency
- Configure health checks with appropriate thresholds
- Enable cross-zone load balancing
- Use target groups to route to different services
- Enable access logging for monitoring and debugging
- Configure SSL/TLS termination at the load balancer
- Integrate with AWS WAF for Application Load Balancer

**Anti-Patterns to Avoid:**
- Not configuring health checks
- Using a single AZ for targets
- Not enabling access logging
- Overly permissive security groups
- Not using sticky sessions when needed for stateful apps

**Suggested File Structure:**
- `infra/aws/load-balancer.tf` (config)

## Dependency Chain

Startup/initialization order based on edge directions and interaction patterns.

**Must be available BEFORE this node starts:**
- ECS Fargate Backend (Multi-AZ) (this node calls/depends on it via REST API (ALB to Backend) (rest))
- AWS WAF (CloudFront + ALB) (this node calls/depends on it via AWS WAF (Web Application Firewall) (custom))

**Depends on THIS node being available:**
- CloudFront CDN (calls this node via REST API (ALB to Backend) (rest))

## Error Handling Contracts

**Errors this node MUST emit to consumers:**
- HTTP error responses to CloudFront CDN ("REST API (ALB to Backend)"): return proper 4xx for validation errors, 401/403 for auth failures, 5xx for internal errors with correlation IDs

**Errors this node MUST handle from dependencies:**
- HTTP errors from ECS Fargate Backend (Multi-AZ) ("REST API (ALB to Backend)"): handle 4xx (client error), 5xx (server error), timeouts, and connection refused

**Parent Container:** AWS VPC (Private Network) (vpc)
