# Task: ElastiCache Redis (Cluster Mode, Multi-AZ)

> **Scope:** implement ONLY this node ("ElastiCache Redis (Cluster Mode, Multi-AZ)"). Work belonging to other nodes appears here solely as interfaces and coordination points — do not implement or re-derive it.
> This document is DERIVED from the NodeSpec model + catalog (fingerprinted, regenerable via generate_task_docs). Node context/export is the model truth; propose model changes through the proposal flow — hand-edits to model facts here do not change the model.

## Component Purpose

**Role:** Cache
**Technology:** AWS ElastiCache
**Description:** In-memory data caching layer

## Your Deliverable

This is a provider-managed service — no application code implements it.
- **Provisioning configuration (IaC)** — declare the service as config artifacts: existence, sizing, wiring, permissions. The IaC tool is NOT declared on this project's platform container — CONFIRM the tool with the user (Terraform / OpenTofu / Pulumi / provider-native / CDK) before authoring artifacts; do NOT assume one.
- **Connection contracts** for every interface below

## Implementation Tasks

Ordered WORK ORDERS synthesized from the model — this node's deliverable kind, contracts, criterion attribution, configuration, and dependency chain. They guarantee coverage, scope, and traceability; they deliberately do NOT contain the implementation detail — that is your job (see the expansion directive below the list).

- [ ] **T1 — Provision AWS ElastiCache via IaC.**
  Author the provisioning definition as config artifacts bound to this node — existence, wiring, permissions, deployed under AWS VPC (Private Network).
  [PLACEHOLDER: config — no user configuration recorded for this node; confirm sizing/domains/settings with the user]
- [ ] **T2 — Expose the interface ECS Fargate Backend (Multi-AZ) consumes, per Contract "Redis Cache (ElastiCache)" (custom).**
  Record the endpoint/identifiers ECS Fargate Backend (Multi-AZ) needs in this node's config artifacts — coordinate with ECS Fargate Backend (Multi-AZ).
  Build to the contract schema EXACTLY (see Interface Contracts).
  ↳ serves (unverified match): REQ-008 "ElastiCache (if used) is in cluster mode across multiple AZs" — requirement not mapped to that node; verify or reassign before relying on it
  ↳ serves: REQ-009 "Frequently read, rarely written data (e.g., configuration, lookup tables) is cached in ElastiCache Redis with appropriate TTLs" — coordinate with ECS Fargate Backend (Multi-AZ)
  ↳ serves: REQ-009 "Cache-aside pattern is used; cache miss falls back to the database and populates the cache" — coordinate with ECS Fargate Backend (Multi-AZ)
  ↳ serves: REQ-009 "CDN cache-hit ratio for static assets is ≥ 95% in steady state" — coordinate with ECS Fargate Backend (Multi-AZ)
- [ ] **T3 — Configure the service to satisfy: "Application compute (ECS/EKS tasks or EC2 ASG) spans a minimum of two Availability Zones at all times" (REQ-008).**
  No interface contract maps to this criterion — it is this node's internal responsibility.
  ↳ serves: REQ-008 "Application compute (ECS/EKS tasks or EC2 ASG) spans a minimum of two Availability Zones at all times"
- [ ] **T4 — Configure the service to satisfy: "RDS Aurora is deployed in Multi-AZ; automated failover completes within 30 seconds" (REQ-008).**
  No interface contract maps to this criterion — it is this node's internal responsibility.
  ↳ serves: REQ-008 "RDS Aurora is deployed in Multi-AZ; automated failover completes within 30 seconds"
- [ ] **T5 — Configure the service to satisfy: "Auto Scaling policies maintain a minimum of 2 healthy instances and scale out when average CPU exceeds 60% for 3 consecutive minutes" (REQ-008).**
  No interface contract maps to this criterion — it is this node's internal responsibility.
  ↳ serves: REQ-008 "Auto Scaling policies maintain a minimum of 2 healthy instances and scale out when average CPU exceeds 60% for 3 consecutive minutes"
- [ ] **T6 — Configure the service to satisfy: "An Application Load Balancer health check marks an instance unhealthy within 30 seconds and stops routing traffic to it" (REQ-008).**
  No interface contract maps to this criterion — it is this node's internal responsibility.
  ↳ serves: REQ-008 "An Application Load Balancer health check marks an instance unhealthy within 30 seconds and stops routing traffic to it"
- [ ] **T7 — Configure the service to satisfy: "Monthly uptime SLO is 99.9%; an SLA breach monitoring alert fires when error rate exceeds 1% over a 5-minute window" (REQ-008).**
  No interface contract maps to this criterion — it is this node's internal responsibility.
  ↳ serves: REQ-008 "Monthly uptime SLO is 99.9%; an SLA breach monitoring alert fires when error rate exceeds 1% over a 5-minute window"
- [ ] **T8 — Configure the service to satisfy: "Chaos engineering run-books (simulating AZ failure) are documented and executed at least quarterly" (REQ-008).**
  No interface contract maps to this criterion — it is this node's internal responsibility.
  ↳ serves: REQ-008 "Chaos engineering run-books (simulating AZ failure) are documented and executed at least quarterly"
- [ ] **T9 — Resolve ownership, then implement: "p95 API response time is ≤ 300 ms and p99 ≤ 800 ms under a load of 500 concurrent users" (REQ-009).**
  [PLACEHOLDER: owner — this node or a sharing node (ECS Fargate Backend (Multi-AZ), Application Load Balancer); assign via the requirement mapping, then keep this task here or move it to the owning node's doc]
  ↳ serves: REQ-009 "p95 API response time is ≤ 300 ms and p99 ≤ 800 ms under a load of 500 concurrent users"
- [ ] **T10 — Resolve ownership, then implement: "The system auto-scales to handle 5× baseline traffic within 3 minutes without manual intervention" (REQ-009).**
  [PLACEHOLDER: owner — this node or a sharing node (ECS Fargate Backend (Multi-AZ), Application Load Balancer); assign via the requirement mapping, then keep this task here or move it to the owning node's doc]
  ↳ serves: REQ-009 "The system auto-scales to handle 5× baseline traffic within 3 minutes without manual intervention"
- [ ] **T11 — Resolve ownership, then implement: "Database read replicas are used for read-heavy analytical queries to offload the primary writer" (REQ-009).**
  [PLACEHOLDER: owner — this node or a sharing node (ECS Fargate Backend (Multi-AZ), Application Load Balancer); assign via the requirement mapping, then keep this task here or move it to the owning node's doc]
  ↳ serves: REQ-009 "Database read replicas are used for read-heavy analytical queries to offload the primary writer"
- [ ] **T12 — Resolve ownership, then implement: "A k6 or Locust load test suite is run in CI on every release candidate; builds fail if p95 exceeds the SLO threshold" (REQ-009).**
  [PLACEHOLDER: owner — this node or a sharing node (ECS Fargate Backend (Multi-AZ), Application Load Balancer); assign via the requirement mapping, then keep this task here or move it to the owning node's doc]
  ↳ serves: REQ-009 "A k6 or Locust load test suite is run in CI on every release candidate; builds fail if p95 exceeds the SLO threshold"
- [ ] **T13 — Verify every acceptance criterion above and tick its box.**
  Completion evidence flows back to the requirement criteria. This node is complete only when every criterion box is ticked and no `[PLACEHOLDER: …]` tag remains open.

**Your first action — expand these work orders.** Each task above guarantees WHAT must be covered, not HOW. Before writing any code or configuration, expand every task with the concrete implementation steps for THIS technology in THIS project — the specific resources, settings, files, schemas, and tests — using the Configuration, Interface Contracts, Technology Guidance, and node context as your references. Record the expanded list in this section via update_artifact (propose_patches) after this doc is accepted, keeping task IDs, criterion citations, and open `[PLACEHOLDER: …]` tags intact. Resolve placeholders with the user through the proposal flow; this node is never complete while one remains open.

## Requirements — Your Scope

### REQ-008: High Availability & Fault Tolerance
Category: non-functional | Status: in-progress
The application must tolerate the loss of any single Availability Zone without service interruption. All stateful components run in Multi-AZ configurations. Compute is deployed across at least two AZs with auto-scaling. The system targets 99.9% monthly uptime (SLO), permitting no more than ~43 minutes of downtime per month.

**Acceptance criteria — your task boxes:**
- [ ] Application compute (ECS/EKS tasks or EC2 ASG) spans a minimum of two Availability Zones at all times
  → covered by Task T3
- [ ] RDS Aurora is deployed in Multi-AZ; automated failover completes within 30 seconds
  → covered by Task T4
- [ ] ElastiCache (if used) is in cluster mode across multiple AZs
  → covered by Task T2
- [ ] Auto Scaling policies maintain a minimum of 2 healthy instances and scale out when average CPU exceeds 60% for 3 consecutive minutes
  → covered by Task T5
- [ ] An Application Load Balancer health check marks an instance unhealthy within 30 seconds and stops routing traffic to it
  → covered by Task T6
- [ ] Monthly uptime SLO is 99.9%; an SLA breach monitoring alert fires when error rate exceeds 1% over a 5-minute window
  → covered by Task T7
- [ ] Chaos engineering run-books (simulating AZ failure) are documented and executed at least quarterly
  → covered by Task T8

### REQ-009: Performance & Scalability
Category: non-functional | Status: in-progress
_Shared with: ECS Fargate Backend (Multi-AZ), Application Load Balancer — their slices live in their own task docs._
The application must handle baseline traffic with headroom to scale elastically under load. API response times must remain within SLO thresholds at both normal and peak load. A caching layer (ElastiCache Redis) reduces database pressure for hot read paths. Performance budgets are enforced in CI.

**Acceptance criteria — your task boxes:**
- [ ] p95 API response time is ≤ 300 ms and p99 ≤ 800 ms under a load of 500 concurrent users
  → covered by Task T9
- [ ] The system auto-scales to handle 5× baseline traffic within 3 minutes without manual intervention
  → covered by Task T10
- [ ] Frequently read, rarely written data (e.g., configuration, lookup tables) is cached in ElastiCache Redis with appropriate TTLs
  → covered by Task T2
- [ ] Cache-aside pattern is used; cache miss falls back to the database and populates the cache
  → covered by Task T2
- [ ] Database read replicas are used for read-heavy analytical queries to offload the primary writer
  → covered by Task T11
- [ ] A k6 or Locust load test suite is run in CI on every release candidate; builds fail if p95 exceeds the SLO threshold
  → covered by Task T12
- [ ] CDN cache-hit ratio for static assets is ≥ 95% in steady state
  → covered by Task T2

## Interface Contracts

### RECEIVES FROM: ECS Fargate Backend (Multi-AZ) (backend-service)
- **Contract:** Redis Cache (ElastiCache)
- **Protocol:** custom
- **Their Technology:** aws-fargate

**Schema:**
```
{
  "tls": true,
  "auth": "token from Secrets Manager",
  "mode": "cluster-mode, Multi-AZ, auto-failover",
  "port": 6379,
  "ttls": {
    "session": "900s",
    "lookupTable": "3600s",
    "userProfile": "300s"
  },
  "engine": "Redis 7+ on ElastiCache",
  "pattern": "cache-aside",
  "eviction": "allkeys-lru",
  "keyFormat": "app:{entity}:{id}"
}
```

## Technology Guidance

_Reference for executing the Implementation Tasks above — apply where relevant. The task list stands even where this guidance is thin._

**Purpose:** Fully managed in-memory cache compatible with Valkey, Redis OSS, and Memcached. Use for microsecond-latency reads, session storage, hot-key lookups, rate limiting, and taking read pressure off the primary database. Serverless mode removes capacity management; node-based clusters give fine-grained control over node types and placement. Valkey is the forward path (open-source Redis successor); prefer it for new workloads.

**Best Practices:**
- Prefer serverless for new workloads — instant scaling, no node management
- Choose Valkey over Redis OSS for new caches (open governance, lower cost on ElastiCache)
- Set TTLs on every key class — an unbounded cache is a memory leak with latency
- Use cluster mode / multi-AZ for anything a cache outage would take down
- Never treat the cache as a source of truth — rebuildable from the database by design

**Anti-Patterns to Avoid:**
- Caching without an invalidation story (stale reads become correctness bugs)
- Using the cache as a durable store or queue
- One giant shared cache for unrelated workloads — blast radius and noisy neighbors
- Skipping AUTH/encryption on caches holding user data

**Suggested File Structure:**
- `infra/aws/elasticache.tf` (config)

## Dependency Chain

Startup/initialization order based on edge directions and interaction patterns.

**Depends on THIS node being available:**
- ECS Fargate Backend (Multi-AZ) (initiates Redis Cache (ElastiCache) against this node (custom))

**Parent Container:** AWS VPC (Private Network) (vpc)
