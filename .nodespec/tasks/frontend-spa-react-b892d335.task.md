# Task: Frontend SPA (React)

> **Scope:** implement ONLY this node ("Frontend SPA (React)"). Work belonging to other nodes appears here solely as interfaces and coordination points — do not implement or re-derive it.
> This document is DERIVED from the NodeSpec model + catalog (fingerprinted, regenerable via generate_task_docs). Node context/export is the model truth; propose model changes through the proposal flow — hand-edits to model facts here do not change the model.

## Component Purpose

**Role:** Frontend Application
**Technology:** React
**Description:** Client-side web application or SPA

## Your Deliverable

**Working code for this component**, honoring the contracts and criteria below, plus its configuration artifacts and tests.

## Implementation Tasks

Ordered WORK ORDERS synthesized from the model — this node's deliverable kind, contracts, criterion attribution, configuration, and dependency chain. They guarantee coverage, scope, and traceability; they deliberately do NOT contain the implementation detail — that is your job (see the expansion directive below the list).

- [ ] **T1 — Scaffold the React component.**
  Create the source layout, build files, and test harness this node's working code lives in.
  Start from the catalog's suggested structure: `src/App.tsx`, `src/main.tsx`, `vite.config.ts`, `tsconfig.json`.
- [ ] **T2 — Implement the integration with S3 Static Assets (aws-s3) per Contract "Static Build Deployment (CI Upload to S3)" (custom).**
  Build to the contract schema EXACTLY (see Interface Contracts).
  ↳ serves (unverified match): REQ-004 "Static assets are served with Cache-Control: max-age=31536000, immutable via CloudFront" — requirement not mapped to that node; verify or reassign before relying on it
- [ ] **T3 — Implement the integration with CloudFront CDN (aws-cloudfront) per Contract "REST API (ALB to Backend)" (rest).**
  Build to the contract schema EXACTLY (see Interface Contracts).
  ↳ serves: REQ-004 "The Content Security Policy (CSP) header disallows inline scripts and restricts script-src to known CDN origins" — coordinate with CloudFront CDN
- [ ] **T4 — Resolve ownership, then implement: "The application is fully functional on Chrome, Firefox, Safari, and Edge (latest two major versions each)" (REQ-004).**
  [PLACEHOLDER: owner — this node or a sharing node (CloudFront CDN); assign via the requirement mapping, then keep this task here or move it to the owning node's doc]
  ↳ serves: REQ-004 "The application is fully functional on Chrome, Firefox, Safari, and Edge (latest two major versions each)"
- [ ] **T5 — Resolve ownership, then implement: "Core Web Vitals scores meet 'Good' thresholds: LCP < 2.5 s, CLS < 0.1, INP < 200 ms on a simulated 4G connection" (REQ-004).**
  [PLACEHOLDER: owner — this node or a sharing node (CloudFront CDN); assign via the requirement mapping, then keep this task here or move it to the owning node's doc]
  ↳ serves: REQ-004 "Core Web Vitals scores meet 'Good' thresholds: LCP < 2.5 s, CLS < 0.1, INP < 200 ms on a simulated 4G connection"
- [ ] **T6 — Resolve ownership, then implement: "The UI is responsive and usable on viewports from 375 px (mobile) to 1920 px (desktop)" (REQ-004).**
  [PLACEHOLDER: owner — this node or a sharing node (CloudFront CDN); assign via the requirement mapping, then keep this task here or move it to the owning node's doc]
  ↳ serves: REQ-004 "The UI is responsive and usable on viewports from 375 px (mobile) to 1920 px (desktop)"
- [ ] **T7 — Resolve ownership, then implement: "Navigation items and actions hidden by RBAC are not rendered in the DOM (not merely CSS-hidden)" (REQ-004).**
  [PLACEHOLDER: owner — this node or a sharing node (CloudFront CDN); assign via the requirement mapping, then keep this task here or move it to the owning node's doc]
  ↳ serves: REQ-004 "Navigation items and actions hidden by RBAC are not rendered in the DOM (not merely CSS-hidden)"
- [ ] **T8 — Resolve ownership, then implement: "A global error boundary catches unhandled exceptions and shows a user-friendly fallback without exposing stack traces" (REQ-004).**
  [PLACEHOLDER: owner — this node or a sharing node (CloudFront CDN); assign via the requirement mapping, then keep this task here or move it to the owning node's doc]
  ↳ serves: REQ-004 "A global error boundary catches unhandled exceptions and shows a user-friendly fallback without exposing stack traces"
- [ ] **T9 — Verify every acceptance criterion above and tick its box.**
  Completion evidence flows back to the requirement criteria. This node is complete only when every criterion box is ticked and no `[PLACEHOLDER: …]` tag remains open.

**Your first action — expand these work orders.** Each task above guarantees WHAT must be covered, not HOW. Before writing any code or configuration, expand every task with the concrete implementation steps for THIS technology in THIS project — the specific resources, settings, files, schemas, and tests — using the Configuration, Interface Contracts, Technology Guidance, and node context as your references. Record the expanded list in this section via update_artifact (propose_patches) after this doc is accepted, keeping task IDs, criterion citations, and open `[PLACEHOLDER: …]` tags intact. Resolve placeholders with the user through the proposal flow; this node is never complete while one remains open.

## Requirements — Your Scope

### REQ-004: Web Frontend Application
Category: functional | Status: in-progress
_Shared with: CloudFront CDN — their slices live in their own task docs._
The application delivers a responsive single-page application (SPA) or server-side rendered (SSR) frontend served via CloudFront CDN. The UI renders correctly on modern desktop and mobile browsers, enforces client-side RBAC for navigation, and degrades gracefully on slow connections. Static assets are cache-optimised with content-addressed filenames.

**Acceptance criteria — your task boxes:**
- [ ] The application is fully functional on Chrome, Firefox, Safari, and Edge (latest two major versions each)
  → covered by Task T4
- [ ] Core Web Vitals scores meet 'Good' thresholds: LCP < 2.5 s, CLS < 0.1, INP < 200 ms on a simulated 4G connection
  → covered by Task T5
- [ ] The UI is responsive and usable on viewports from 375 px (mobile) to 1920 px (desktop)
  → covered by Task T6
- [ ] Navigation items and actions hidden by RBAC are not rendered in the DOM (not merely CSS-hidden)
  → covered by Task T7
- [ ] A global error boundary catches unhandled exceptions and shows a user-friendly fallback without exposing stack traces
  → covered by Task T8
- [ ] Static assets are served with Cache-Control: max-age=31536000, immutable via CloudFront
  → covered by Task T2
- [ ] The Content Security Policy (CSP) header disallows inline scripts and restricts script-src to known CDN origins
  → covered by Task T3

## Interface Contracts

### SENDS TO: S3 Static Assets (object-storage)
- **Contract:** Static Build Deployment (CI Upload to S3)
- **Protocol:** custom
- **Their Technology:** aws-s3

**Schema:**
```
{
  "steps": [
    "npm run build (content-addressed bundles)",
    "aws s3 sync dist/ s3://static-assets-{env}/ --delete --cache-control max-age=31536000,immutable",
    "aws cloudfront create-invalidation --paths /index.html"
  ],
  "output": {
    "directory": "dist/",
    "filenames": "content-hashed (e.g., main.a1b2c3d4.js)",
    "indexHtml": "Cache-Control: no-cache, must-revalidate"
  },
  "trigger": "merge to main (GitHub Actions)",
  "buildTool": "Vite",
  "nodeVersion": "20 LTS"
}
```

### SENDS TO: CloudFront CDN (cdn)
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

## Technology Guidance

_Reference for executing the Implementation Tasks above — apply where relevant. The task list stands even where this guidance is thin._

**Purpose:** Component-based UI library for building interactive single-page applications

**SDK Initialization:**
```
npm create vite@latest my-app -- --template react-ts && cd my-app && npm install
// src/App.tsx
export default function App() {
  return <div>Hello React</div>;
}
```

**Common API Patterns:**

#### Component with State
Functional component with useState hook
```
function Counter() {
  const [count, setCount] = useState(0);
  return (
    <button onClick={() => setCount(c => c + 1)}>
      Count: {count}
    </button>
  );
}
```

#### Data Fetching
Custom hook for data fetching with loading state
```
function useUsers() {
  const [users, setUsers] = useState<User[]>([]);
  const [loading, setLoading] = useState(true);
  useEffect(() => {
    fetch("/api/users").then(r => r.json()).then(setUsers).finally(() => setLoading(false));
  }, []);
  return { users, loading };
}
```

#### Context Provider
Context API for global state with typed custom hook
```
const AuthContext = createContext<AuthState | null>(null);
export function AuthProvider({ children }: { children: ReactNode }) {
  const [user, setUser] = useState<User | null>(null);
  return (
    <AuthContext.Provider value={{ user, setUser }}>
      {children}
    </AuthContext.Provider>
  );
}
export const useAuth = () => useContext(AuthContext)!;
```

**Configuration Template:**
```
// vite.config.ts
import { defineConfig } from "vite";
import react from "@vitejs/plugin-react";
export default defineConfig({
  plugins: [react()],
  server: { port: 3000 },
  build: { sourcemap: true }
});

// tsconfig.json
{
  "compilerOptions": {
    "target": "ES2020",
    "module": "ESNext",
    "jsx": "react-jsx",
    "strict": true,
    "moduleResolution": "bundler"
  }
}
```

**Best Practices:**
- Use functional components with hooks
- Implement proper state management (lift state up, use context sparingly)
- Memoize expensive computations with useMemo/useCallback
- Use React.lazy for code splitting
- Follow the single responsibility principle per component
- Use TypeScript for type safety
- Implement error boundaries
- Use Suspense for async data loading

**Anti-Patterns to Avoid:**
- Prop drilling through many levels instead of using Context or state management
- Using useEffect for derived state that should be computed during render
- Creating new object/function references in render causing unnecessary re-renders
- Using index as key in lists with dynamic ordering or mutations
- Putting business logic directly in components instead of custom hooks

**Security:** React auto-escapes JSX output preventing most XSS. Never use dangerouslySetInnerHTML with unsanitized input. Validate and sanitize URL-based props (href, src) to prevent javascript: injection. Use Content Security Policy headers. Avoid storing sensitive tokens in localStorage -- use httpOnly cookies. Sanitize user input before passing to third-party libraries that manipulate DOM directly.

**Integration Patterns:**
- React Router for client-side routing with lazy-loaded routes
- TanStack Query (React Query) for server state management and caching
- Zustand or Jotai for lightweight client state management
- Tailwind CSS or CSS Modules for scoped styling
- Vitest + React Testing Library for component testing

**Suggested File Structure:**
- `src/App.tsx` (source)
- `src/main.tsx` (source)
- `vite.config.ts` (config)
- `tsconfig.json` (config)

## Dependency Chain

Startup/initialization order based on edge directions and interaction patterns.

**Must be available BEFORE this node starts:**
- S3 Static Assets (this node calls/depends on it via Static Build Deployment (CI Upload to S3) (custom))
- CloudFront CDN (this node calls/depends on it via REST API (ALB to Backend) (rest))

## Error Handling Contracts

**Errors this node MUST handle from dependencies:**
- HTTP errors from CloudFront CDN ("REST API (ALB to Backend)"): handle 4xx (client error), 5xx (server error), timeouts, and connection refused
