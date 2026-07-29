# Task: CloudFront CDN

> **Scope:** implement ONLY this node ("CloudFront CDN"). Work belonging to other nodes appears here solely as interfaces and coordination points — do not implement or re-derive it.
> This document is DERIVED from the NodeSpec model + catalog (fingerprinted, regenerable via generate_task_docs). Node context/export is the model truth; propose model changes through the proposal flow — hand-edits to model facts here do not change the model.

## Component Purpose

**Role:** CDN
**Technology:** AWS CloudFront
**Description:** Content delivery network edge node

## Your Deliverable

This is a provider-managed service — no application code implements it.
- **Provisioning configuration (IaC)** — declare the service as config artifacts: existence, sizing, wiring, permissions. The IaC tool is NOT declared on this project's platform container — CONFIRM the tool with the user (Terraform / OpenTofu / Pulumi / provider-native / CDK) before authoring artifacts; do NOT assume one.
- **Connection contracts** for every interface below

## Implementation Tasks

Ordered WORK ORDERS synthesized from the model — this node's deliverable kind, contracts, criterion attribution, configuration, and dependency chain. They guarantee coverage, scope, and traceability; they deliberately do NOT contain the implementation detail — that is your job (see the expansion directive below the list).

- [ ] **T1 — Provision AWS CloudFront via IaC.**
  Author the provisioning definition as config artifacts bound to this node — existence, wiring, permissions, deployed under AWS Cloud Platform.
  [PLACEHOLDER: config — no user configuration recorded for this node; confirm sizing/domains/settings with the user]
- [ ] **T2 — Declare the wiring to AWS WAF (CloudFront + ALB) (aws-waf) per Contract "AWS WAF (Web Application Firewall)" (custom).**
  Build to the contract schema EXACTLY (see Interface Contracts).
  ↳ serves (unverified match): REQ-004 "The application is fully functional on Chrome, Firefox, Safari, and Edge (latest two major versions each)" — requirement not mapped to that node; verify or reassign before relying on it
- [ ] **T3 — Declare the wiring to Application Load Balancer (aws-elb) per Contract "REST API (ALB to Backend)" (rest).**
  Build to the contract schema EXACTLY (see Interface Contracts).
  ↳ serves (unverified match): REQ-004 "The Content Security Policy (CSP) header disallows inline scripts and restricts script-src to known CDN origins" — requirement not mapped to that node; verify or reassign before relying on it
- [ ] **T4 — Declare the wiring to S3 Static Assets (aws-s3) per Contract "CloudFront Distribution (Static Assets)" (custom).**
  Build to the contract schema EXACTLY (see Interface Contracts).
  ↳ serves (unverified match): REQ-004 "Static assets are served with Cache-Control: max-age=31536000, immutable via CloudFront" — requirement not mapped to that node; verify or reassign before relying on it
- [ ] **T5 — Expose the interface Frontend SPA (React) consumes, per Contract "REST API (ALB to Backend)" (rest).**
  Record the endpoint/identifiers Frontend SPA (React) needs in this node's config artifacts — coordinate with Frontend SPA (React).
  Build to the contract schema EXACTLY (see Interface Contracts).
- [ ] **T6 — Resolve ownership, then implement: "Core Web Vitals scores meet 'Good' thresholds: LCP < 2.5 s, CLS < 0.1, INP < 200 ms on a simulated 4G connection" (REQ-004).**
  [PLACEHOLDER: owner — this node or a sharing node (Frontend SPA (React)); assign via the requirement mapping, then keep this task here or move it to the owning node's doc]
  ↳ serves: REQ-004 "Core Web Vitals scores meet 'Good' thresholds: LCP < 2.5 s, CLS < 0.1, INP < 200 ms on a simulated 4G connection"
- [ ] **T7 — Resolve ownership, then implement: "The UI is responsive and usable on viewports from 375 px (mobile) to 1920 px (desktop)" (REQ-004).**
  [PLACEHOLDER: owner — this node or a sharing node (Frontend SPA (React)); assign via the requirement mapping, then keep this task here or move it to the owning node's doc]
  ↳ serves: REQ-004 "The UI is responsive and usable on viewports from 375 px (mobile) to 1920 px (desktop)"
- [ ] **T8 — Resolve ownership, then implement: "Navigation items and actions hidden by RBAC are not rendered in the DOM (not merely CSS-hidden)" (REQ-004).**
  [PLACEHOLDER: owner — this node or a sharing node (Frontend SPA (React)); assign via the requirement mapping, then keep this task here or move it to the owning node's doc]
  ↳ serves: REQ-004 "Navigation items and actions hidden by RBAC are not rendered in the DOM (not merely CSS-hidden)"
- [ ] **T9 — Resolve ownership, then implement: "A global error boundary catches unhandled exceptions and shows a user-friendly fallback without exposing stack traces" (REQ-004).**
  [PLACEHOLDER: owner — this node or a sharing node (Frontend SPA (React)); assign via the requirement mapping, then keep this task here or move it to the owning node's doc]
  ↳ serves: REQ-004 "A global error boundary catches unhandled exceptions and shows a user-friendly fallback without exposing stack traces"
- [ ] **T10 — Verify every acceptance criterion above and tick its box.**
  Completion evidence flows back to the requirement criteria. This node is complete only when every criterion box is ticked and no `[PLACEHOLDER: …]` tag remains open.

**Your first action — expand these work orders.** Each task above guarantees WHAT must be covered, not HOW. Before writing any code or configuration, expand every task with the concrete implementation steps for THIS technology in THIS project — the specific resources, settings, files, schemas, and tests — using the Configuration, Interface Contracts, Technology Guidance, and node context as your references. Record the expanded list in this section via update_artifact (propose_patches) after this doc is accepted, keeping task IDs, criterion citations, and open `[PLACEHOLDER: …]` tags intact. Resolve placeholders with the user through the proposal flow; this node is never complete while one remains open.

## Requirements — Your Scope

### REQ-004: Web Frontend Application
Category: functional | Status: in-progress
_Shared with: Frontend SPA (React) — their slices live in their own task docs._
The application delivers a responsive single-page application (SPA) or server-side rendered (SSR) frontend served via CloudFront CDN. The UI renders correctly on modern desktop and mobile browsers, enforces client-side RBAC for navigation, and degrades gracefully on slow connections. Static assets are cache-optimised with content-addressed filenames.

**Acceptance criteria — your task boxes:**
- [ ] The application is fully functional on Chrome, Firefox, Safari, and Edge (latest two major versions each)
  → covered by Task T2
- [ ] Core Web Vitals scores meet 'Good' thresholds: LCP < 2.5 s, CLS < 0.1, INP < 200 ms on a simulated 4G connection
  → covered by Task T6
- [ ] The UI is responsive and usable on viewports from 375 px (mobile) to 1920 px (desktop)
  → covered by Task T7
- [ ] Navigation items and actions hidden by RBAC are not rendered in the DOM (not merely CSS-hidden)
  → covered by Task T8
- [ ] A global error boundary catches unhandled exceptions and shows a user-friendly fallback without exposing stack traces
  → covered by Task T9
- [ ] Static assets are served with Cache-Control: max-age=31536000, immutable via CloudFront
  → covered by Task T4
- [ ] The Content Security Policy (CSP) header disallows inline scripts and restricts script-src to known CDN origins
  → covered by Task T3

## Interface Contracts

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

### SENDS TO: Application Load Balancer (load-balancer)
- **Contract:** REST API (ALB to Backend)
- **Protocol:** rest
- **Spec Format:** openapi
- **Their Technology:** aws-elb

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

### RECEIVES FROM: Frontend SPA (React) (frontend-app)
- **Contract:** REST API (ALB to Backend)
- **Protocol:** rest
- **Spec Format:** openapi
- **Their Technology:** react

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

### SENDS TO: S3 Static Assets (object-storage)
- **Contract:** CloudFront Distribution (Static Assets)
- **Protocol:** custom
- **Their Technology:** aws-s3

**Schema:**
```
{
  "csp": "script-src 'self' cdn-origins; default-src 'self'",
  "tls": {
    "hsts": "max-age=31536000; includeSubDomains",
    "minimum": "TLSv1.2_2021",
    "certificate": "ACM auto-renewal"
  },
  "waf": "attached",
  "origins": [
    {
      "type": "S3",
      "access": "Origin Access Identity",
      "bucket": "static-assets-{env}"
    },
    {
      "type": "ALB",
      "protocol": "HTTPS only",
      "pathPattern": "/api/*"
    }
  ],
  "apiBehavior": {
    "cache": "disabled",
    "forwardHeaders": "all viewer headers"
  },
  "defaultBehavior": {
    "cache": "CachingOptimized",
    "compress": true,
    "cacheControl": "max-age=31536000, immutable"
  }
}
```

## Technology Guidance

_Reference for executing the Implementation Tasks above — apply where relevant. The task list stands even where this guidance is thin._

**Purpose:** AWS's global content delivery network (CDN) that caches and delivers content from edge locations worldwide. Use when you need low-latency delivery of static assets (images, CSS, JS), dynamic API acceleration, or DDoS protection at the edge. CloudFront integrates natively with S3, ALB, API Gateway, and Lambda@Edge/CloudFront Functions for edge compute. Ideal for web applications needing global performance, video streaming, and any workload where proximity to end users matters. CloudFront also provides SSL/TLS termination at the edge with AWS Certificate Manager. Don't use when your users are all in a single region and latency is already acceptable -- a simple ALB or S3 with Transfer Acceleration may suffice. Don't use for real-time bidirectional communication (WebSockets) unless you specifically configure it. Avoid when cost sensitivity is extreme and CloudFlare or a simpler CDN would meet your needs at lower cost.

**SDK Initialization:**
```
# AWS CLI create distribution
aws cloudfront create-distribution --distribution-config file://dist-config.json
# Terraform is preferred for CloudFront configuration (see configurationTemplate)
```

**Common API Patterns:**

#### Create Invalidation
Invalidate all cached content after a deployment
```
aws cloudfront create-invalidation --distribution-id E1234567890 --paths "/*"
```

#### CloudFront Function
CloudFront Function for SPA URL rewriting
```
function handler(event) {
  var request = event.request;
  var uri = request.uri;
  // SPA routing: rewrite paths without extensions to /index.html
  if (!uri.includes('.')) {
    request.uri = '/index.html';
  }
  return request;
}
```

#### Signed URL
Generate signed URL for private content access
```
import { getSignedUrl } from "@aws-sdk/cloudfront-signer";
const signedUrl = getSignedUrl({
  url: `https://d123.cloudfront.net/private/file.pdf`,
  keyPairId: process.env.CF_KEY_PAIR_ID!,
  privateKey: process.env.CF_PRIVATE_KEY!,
  dateLessThan: new Date(Date.now() + 3600000).toISOString()
});
```

**Configuration Template:**
```
resource "aws_cloudfront_distribution" "main" {
  enabled             = true
  default_root_object = "index.html"
  aliases             = ["app.example.com"]
  origin {
    domain_name              = aws_s3_bucket.assets.bucket_regional_domain_name
    origin_id                = "s3-assets"
    origin_access_control_id = aws_cloudfront_origin_access_control.main.id
  }
  default_cache_behavior {
    allowed_methods        = ["GET", "HEAD"]
    cached_methods         = ["GET", "HEAD"]
    target_origin_id       = "s3-assets"
    cache_policy_id        = "658327ea-f89d-4fab-a63d-7e88639e58f6" # CachingOptimized
    viewer_protocol_policy = "redirect-to-https"
    compress               = true
  }
  viewer_certificate {
    acm_certificate_arn      = aws_acm_certificate.main.arn
    ssl_support_method       = "sni-only"
    minimum_protocol_version = "TLSv1.2_2021"
  }
  restrictions {
    geo_restriction { restriction_type = "none" }
  }
}
```

**Best Practices:**
- Use Origin Access Control (OAC) to restrict S3 access exclusively through CloudFront
- Set appropriate Cache-Control headers on origins to control edge caching behavior
- Use cache policies and origin request policies instead of legacy cache behavior settings
- Enable compression (gzip, brotli) for text-based content
- Use Lambda@Edge or CloudFront Functions for URL rewriting, auth, and A/B testing
- Configure custom error pages for 4xx/5xx responses
- Use CloudFront access logs in S3 for traffic analysis and debugging

**Anti-Patterns to Avoid:**
- Using CloudFront without Origin Access Control allowing direct S3 access
- Setting excessively long TTLs for dynamic content causing stale data
- Not invalidating cache after deployments leaving users on old versions
- Forwarding unnecessary headers/cookies to origin defeating cache effectiveness
- Using a single behavior for all content instead of separating static and dynamic paths
- Not enabling compression wasting bandwidth on text assets

**Security:** Use Origin Access Control (OAC) to prevent direct access to S3 origins. Attach AWS WAF to CloudFront for bot protection, rate limiting, and IP filtering. Enforce HTTPS with redirect-to-https viewer protocol policy. Use TLS 1.2+ minimum protocol version. Use signed URLs or signed cookies for private content. Set appropriate security headers via response headers policy (CSP, HSTS, X-Frame-Options).

**Integration Patterns:**
- S3 + CloudFront for static website hosting with global edge delivery
- ALB + CloudFront for dynamic API acceleration with edge caching
- AWS WAF for web application firewall protection at the CDN edge

**Suggested File Structure:**
- `infra/aws/cloudfront.tf` (config)

## Dependency Chain

Startup/initialization order based on edge directions and interaction patterns.

**Must be available BEFORE this node starts:**
- AWS WAF (CloudFront + ALB) (this node calls/depends on it via AWS WAF (Web Application Firewall) (custom))
- Application Load Balancer (this node calls/depends on it via REST API (ALB to Backend) (rest))
- S3 Static Assets (this node calls/depends on it via CloudFront Distribution (Static Assets) (custom))

**Depends on THIS node being available:**
- Frontend SPA (React) (calls this node via REST API (ALB to Backend) (rest))

## Error Handling Contracts

**Errors this node MUST emit to consumers:**
- HTTP error responses to Frontend SPA (React) ("REST API (ALB to Backend)"): return proper 4xx for validation errors, 401/403 for auth failures, 5xx for internal errors with correlation IDs

**Errors this node MUST handle from dependencies:**
- HTTP errors from Application Load Balancer ("REST API (ALB to Backend)"): handle 4xx (client error), 5xx (server error), timeouts, and connection refused

**Parent Container:** AWS Cloud Platform (aws)
