# Test Plan: REQ-004 - Web Frontend Application

## Architecture Components Under Test
| Component | Role | Technology |
|-----------|------|------------|
| Frontend SPA (React) | frontend-app | react |
| CloudFront CDN | cdn | aws-cloudfront |

## Test Scenarios

### Scenario 1: Cross-browser compatibility
**Validates:** AC-REQ-004-1
- **Given:** The deployed SPA
- **When:** Loaded in Chrome, Firefox, Safari, and Edge (latest 2 versions each)
- **Then:** All features render and function correctly without JS errors

### Scenario 2: Core Web Vitals
**Validates:** AC-REQ-004-2
- **Given:** The production SPA served via CloudFront
- **When:** Lighthouse audit runs on a simulated 4G connection
- **Then:** LCP < 2.5s, CLS < 0.1, INP < 200ms

### Scenario 3: Responsive layout
**Validates:** AC-REQ-004-3
- **Given:** The SPA rendered at various viewport widths
- **When:** Viewport ranges from 375px (mobile) to 1920px (desktop)
- **Then:** All content is usable without horizontal scrolling; navigation adapts appropriately

### Scenario 4: RBAC DOM exclusion
**Validates:** AC-REQ-004-4
- **Given:** A user with Viewer role
- **When:** The page renders navigation and action items
- **Then:** Admin-only and Member-only items are not present in the DOM (not just CSS-hidden)

### Scenario 5: Error boundary
**Validates:** AC-REQ-004-5
- **Given:** A React component that throws an unhandled exception
- **When:** The error propagates
- **Then:** The global error boundary catches it and shows a user-friendly fallback; no stack traces are exposed in the rendered output

### Scenario 6: Static asset caching
**Validates:** AC-REQ-004-6
- **Given:** Static assets served via CloudFront from S3
- **When:** A browser fetches JS/CSS/image assets
- **Then:** Response includes Cache-Control: max-age=31536000, immutable; filenames are content-hashed

### Scenario 7: CSP header
**Validates:** AC-REQ-004-7
- **Given:** Any page served by the application
- **When:** The response headers are inspected
- **Then:** CSP disallows inline scripts; script-src restricted to 'self' and known CDN origins

## Contract Validation Tests
- [ ] Verify build output deploys to S3 with content-hashed filenames
- [ ] Verify CloudFront serves index.html with no-cache and assets with immutable
- [ ] Verify Frontend SPA -> CloudFront API calls use JWT bearer auth
