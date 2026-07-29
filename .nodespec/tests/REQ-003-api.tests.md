# Test Plan: REQ-003 - RESTful API Layer

## Architecture Components Under Test
| Component | Role | Technology |
|-----------|------|------------|
| Application Load Balancer | load-balancer | aws-elb |
| ECS Fargate Backend (Multi-AZ) | backend-service | aws-fargate |

## Test Scenarios

### Scenario 1: API versioning
**Validates:** AC-REQ-003-1
- **Given:** A client sending requests to the API
- **When:** The request targets /api/v1/ endpoints or uses Accept header versioning
- **Then:** All routes are correctly prefixed; version negotiation returns the requested version or 406

### Scenario 2: Authentication enforcement
**Validates:** AC-REQ-003-2
- **Given:** An unauthenticated client
- **When:** The client requests a protected endpoint without a valid JWT
- **Then:** The API returns HTTP 401 with {errors: [{code: 'UNAUTHORIZED'}]} envelope

### Scenario 3: Response envelope consistency
**Validates:** AC-REQ-003-3
- **Given:** Any API response (success or error)
- **When:** The response is returned to the client
- **Then:** The body conforms to {data, meta, errors} envelope; meta includes requestId and timestamp

### Scenario 4: Rate limiting
**Validates:** AC-REQ-003-4
- **Given:** An authenticated user exceeding 1000 requests per minute
- **When:** The rate limit is exceeded
- **Then:** The API returns HTTP 429 with a Retry-After header; unauthenticated endpoints enforce 60 req/min per IP

### Scenario 5: OpenAPI documentation
**Validates:** AC-REQ-003-5
- **Given:** A client requesting API documentation
- **When:** GET /api/docs is called
- **Then:** A valid OpenAPI 3.1 specification is returned that documents all endpoints

### Scenario 6: CORS enforcement
**Validates:** AC-REQ-003-6
- **Given:** A browser request from a non-whitelisted origin
- **When:** The preflight OPTIONS request is sent
- **Then:** The response lacks Access-Control-Allow-Origin; whitelisted origins receive proper CORS headers

### Scenario 7: Body size limit
**Validates:** AC-REQ-003-7
- **Given:** A client sending a request body exceeding 1 MB
- **When:** The request is received by the API
- **Then:** The API returns HTTP 413 before processing the body

## Contract Validation Tests
- [ ] Verify ALB forwards X-Forwarded-For and X-Request-Id headers to ECS
- [ ] Verify CloudFront -> ALB routing for /api/* path pattern
- [ ] Verify WAF rate-based rule (2000/5min) fires before application rate limiting
