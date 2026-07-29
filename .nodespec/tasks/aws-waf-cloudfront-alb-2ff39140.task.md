# Task: AWS WAF (CloudFront + ALB)

> **Scope:** implement ONLY this node ("AWS WAF (CloudFront + ALB)"). Work belonging to other nodes appears here solely as interfaces and coordination points — do not implement or re-derive it.
> This document is DERIVED from the NodeSpec model + catalog (fingerprinted, regenerable via generate_task_docs). Node context/export is the model truth; propose model changes through the proposal flow — hand-edits to model facts here do not change the model.

## Component Purpose

**Role:** WAF
**Technology:** AWS WAF
**Description:** Web Application Firewall for traffic filtering

## Your Deliverable

This is a provider-managed service — no application code implements it.
- **Provisioning configuration (IaC)** — declare the service as config artifacts: existence, sizing, wiring, permissions. The IaC tool is NOT declared on this project's platform container — CONFIRM the tool with the user (Terraform / OpenTofu / Pulumi / provider-native / CDK) before authoring artifacts; do NOT assume one.
- **Connection contracts** for every interface below

## Implementation Tasks

Ordered WORK ORDERS synthesized from the model — this node's deliverable kind, contracts, criterion attribution, configuration, and dependency chain. They guarantee coverage, scope, and traceability; they deliberately do NOT contain the implementation detail — that is your job (see the expansion directive below the list).

- [ ] **T1 — Provision AWS WAF via IaC.**
  Author the provisioning definition as config artifacts bound to this node — existence, wiring, permissions, deployed under AWS Cloud Platform.
  Configuration delegated by the user — choose sensible defaults per the Technology Guidance and record them (see ## Configuration).
- [ ] **T2 — Expose the interface CloudFront CDN consumes, per Contract "AWS WAF (Web Application Firewall)" (custom).**
  Record the endpoint/identifiers CloudFront CDN needs in this node's config artifacts — coordinate with CloudFront CDN.
  Build to the contract schema EXACTLY (see Interface Contracts).
  ↳ serves (unverified match): REQ-010 "AWS WAF is attached to CloudFront and the ALB with managed rule groups: AWSManagedRulesCommonRuleSet, AWSManagedRulesSQLiRuleSet, AWSManagedRulesKnownBadInputsRuleSet" — requirement not mapped to that node; verify or reassign before relying on it
  ↳ serves (unverified match): REQ-010 "WAF rate-based rule blocks IPs sending > 2000 requests per 5 minutes" — requirement not mapped to that node; verify or reassign before relying on it
  ↳ serves (unverified match): REQ-010 "AWS Shield Standard is enabled; Shield Advanced is evaluated if the application is assessed as a DDoS target" — requirement not mapped to that node; verify or reassign before relying on it
- [ ] **T3 — Expose the interface Application Load Balancer consumes, per Contract "AWS WAF (Web Application Firewall)" (custom).**
  Record the endpoint/identifiers Application Load Balancer needs in this node's config artifacts — coordinate with Application Load Balancer.
  Build to the contract schema EXACTLY (see Interface Contracts).
- [ ] **T4 — Resolve ownership, then implement: "The VPC has separate public subnets (ALB, NAT Gateway) and private subnets (compute, database, cache) across ≥2 AZs" (REQ-010).**
  [PLACEHOLDER: owner — this node or a sharing node (Application Load Balancer); assign via the requirement mapping, then keep this task here or move it to the owning node's doc]
  ↳ serves: REQ-010 "The VPC has separate public subnets (ALB, NAT Gateway) and private subnets (compute, database, cache) across ≥2 AZs"
- [ ] **T5 — Resolve ownership, then implement: "No compute instance or database has a public IP address or is directly reachable from the internet" (REQ-010).**
  [PLACEHOLDER: owner — this node or a sharing node (Application Load Balancer); assign via the requirement mapping, then keep this task here or move it to the owning node's doc]
  ↳ serves: REQ-010 "No compute instance or database has a public IP address or is directly reachable from the internet"
- [ ] **T6 — Resolve ownership, then implement: "Security groups permit inbound traffic only on required ports from explicitly named sources (no 0.0.0.0/0 inbound on compute or DB tiers)" (REQ-010).**
  [PLACEHOLDER: owner — this node or a sharing node (Application Load Balancer); assign via the requirement mapping, then keep this task here or move it to the owning node's doc]
  ↳ serves: REQ-010 "Security groups permit inbound traffic only on required ports from explicitly named sources (no 0.0.0.0/0 inbound on compute or DB tiers)"
- [ ] **T7 — Resolve ownership, then implement: "All inter-service traffic within the VPC uses TLS 1.2+ even on private network segments" (REQ-010).**
  [PLACEHOLDER: owner — this node or a sharing node (Application Load Balancer); assign via the requirement mapping, then keep this task here or move it to the owning node's doc]
  ↳ serves: REQ-010 "All inter-service traffic within the VPC uses TLS 1.2+ even on private network segments"
- [ ] **T8 — Verify every acceptance criterion above and tick its box.**
  Completion evidence flows back to the requirement criteria. This node is complete only when every criterion box is ticked and no `[PLACEHOLDER: …]` tag remains open.

**Your first action — expand these work orders.** Each task above guarantees WHAT must be covered, not HOW. Before writing any code or configuration, expand every task with the concrete implementation steps for THIS technology in THIS project — the specific resources, settings, files, schemas, and tests — using the Configuration, Interface Contracts, Technology Guidance, and node context as your references. Record the expanded list in this section via update_artifact (propose_patches) after this doc is accepted, keeping task IDs, criterion citations, and open `[PLACEHOLDER: …]` tags intact. Resolve placeholders with the user through the proposal flow; this node is never complete while one remains open.

## Configuration

**Delegated to you (user choice):** select sensible defaults for this technology per the Technology Guidance, record them as config artifacts bound to this node, and state them when expanding the work orders — the user reviews them there.

## Requirements — Your Scope

### REQ-010: Network Security & Perimeter Defence
Category: non-functional | Status: in-progress
_Shared with: Application Load Balancer — their slices live in their own task docs._
All application components run inside a private VPC. Only the Application Load Balancer and CloudFront distribution are internet-facing. AWS WAF is attached to both CloudFront and the ALB. Private subnets have no direct internet route; outbound traffic egresses through a managed NAT Gateway. Security groups follow least-privilege inbound rules.

**Acceptance criteria — your task boxes:**
- [ ] The VPC has separate public subnets (ALB, NAT Gateway) and private subnets (compute, database, cache) across ≥2 AZs
  → covered by Task T4
- [ ] No compute instance or database has a public IP address or is directly reachable from the internet
  → covered by Task T5
- [ ] AWS WAF is attached to CloudFront and the ALB with managed rule groups: AWSManagedRulesCommonRuleSet, AWSManagedRulesSQLiRuleSet, AWSManagedRulesKnownBadInputsRuleSet
  → covered by Task T2
- [ ] WAF rate-based rule blocks IPs sending > 2000 requests per 5 minutes
  → covered by Task T2
- [ ] Security groups permit inbound traffic only on required ports from explicitly named sources (no 0.0.0.0/0 inbound on compute or DB tiers)
  → covered by Task T6
- [ ] All inter-service traffic within the VPC uses TLS 1.2+ even on private network segments
  → covered by Task T7
- [ ] AWS Shield Standard is enabled; Shield Advanced is evaluated if the application is assessed as a DDoS target
  → covered by Task T2

## Interface Contracts

### RECEIVES FROM: CloudFront CDN (cdn)
- **Contract:** AWS WAF (Web Application Firewall)
- **Protocol:** custom
- **Their Technology:** aws-cloudfront

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

### RECEIVES FROM: Application Load Balancer (load-balancer)
- **Contract:** AWS WAF (Web Application Firewall)
- **Protocol:** custom
- **Their Technology:** aws-elb

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

**Purpose:** Web application firewall that protects web applications from common exploits and bots. Attaches to ALB, API Gateway, CloudFront, or AppSync to filter HTTP/HTTPS requests. Provides managed rule groups (OWASP Top 10, bot control, IP reputation) and custom rules using rate limiting, geo-matching, and regex patterns.

**Best Practices:**
- Use AWS Managed Rules as a baseline (AWSManagedRulesCommonRuleSet for OWASP Top 10)
- Enable logging to S3 or CloudWatch for audit and debugging
- Start with Count mode before switching rules to Block to avoid false positives
- Use rate-based rules to protect against DDoS and brute-force attacks
- Apply IP reputation lists to block known malicious sources
- Create custom rules for application-specific protections

**Anti-Patterns to Avoid:**
- Deploying rules directly in Block mode without testing in Count mode first
- Not monitoring WAF metrics and logs for false positives
- Applying WAF only to ALB while leaving CloudFront or API Gateway unprotected
- Creating overly broad rules that block legitimate traffic

**Suggested File Structure:**
- `terraform/waf.tf` (config)
- `terraform/waf-logging.tf` (config)

## Dependency Chain

Startup/initialization order based on edge directions and interaction patterns.

**Depends on THIS node being available:**
- CloudFront CDN (initiates AWS WAF (Web Application Firewall) against this node (custom))
- Application Load Balancer (initiates AWS WAF (Web Application Firewall) against this node (custom))

**Parent Container:** AWS Cloud Platform (aws)
