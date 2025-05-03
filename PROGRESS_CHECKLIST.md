# Progress Checklist – Parent-Child Behavioral Free Time Rewards System

Below we mirror every milestone, task, and subtask as GitHub-style checkboxes so they can be checked off in pull requests.

> **Convention**  
> • Top-level tasks reference their Milestone ID (e.g., **M1-1.2**).  
> • Sub-tasks are indented one level beneath their parent and inherit the parent's ID.

---

## M1 – Repository & CI Scaffold
- [ ] **M1-1.1** Initialize monorepo (pnpm or npm workspaces)
- [ ] **M1-1.2** Set up Expo project (`apps/mobile-web`) in TypeScript
  - [ ] Run `expo init` with blank TS template
  - [ ] Configure web target & test in browser
  - [ ] Add NativeWind & Tailwind config
- [ ] **M1-1.3** Add Cloudflare Worker template (`workers/api`)
  - [ ] Run `wrangler init` TypeScript template
  - [ ] Set up local dev script (`wrangler dev`)
  - [ ] Add hello-world endpoint returning JSON
- [ ] **M1-1.4** Configure shared TypeScript project references
  - [ ] Align tsconfigs across packages
  - [ ] Path aliases & module resolution check
- [ ] **M1-1.5** Install & configure ESLint, Prettier, Husky hooks
- [ ] **M1-1.6** Add Jest + React Native Testing Library
  - [ ] Install Jest & preset-expo
  - [ ] Configure monorepo jest.config
  - [ ] Add sample component test
- [ ] **M1-1.7** Create GitHub Actions workflow
  - [ ] Install deps & cache node_modules
  - [ ] Run lint & unit tests
  - [ ] Cloudflare Pages deploy

## M2 – Database & Auth Foundation
- [ ] **M2-2.1** Spin up Supabase project & link remote Postgres
- [ ] **M2-2.2** Define initial database schema
  - [ ] Write SQL or Prisma schema for families, parents, children
  - [ ] Add parameters, cycles, collateral, sessions tables
- [ ] **M2-2.3** Generate typed client (`db-types` pkg)
  - [ ] Select generator (Prisma or Supabase CLI)
  - [ ] Generate types into `packages/db-types`
  - [ ] Wire workspace import paths
- [ ] **M2-2.4** Configure Supabase Auth (email + password)
  - [ ] Enable email login in Supabase
  - [ ] Add RLS policies for parents/children
  - [ ] Test sign-up & data access
- [ ] **M2-2.5** Store secrets in GitHub & Cloudflare

## M3 – Cloudflare Worker – Core API
- [ ] **M3-3.1** Implement middleware for Supabase JWT verification
  - [ ] Parse Supabase JWT, validate
  - [ ] Attach user context to request
- [ ] **M3-3.2** CRUD endpoints: families, children, parameters
  - [ ] Families routes
  - [ ] Children routes
  - [ ] Parameters routes with Zod validation
- [ ] **M3-3.3** End-to-end tests for API with `miniflare`
  - [ ] Configure miniflare for Worker
  - [ ] Write happy-path & auth-failure tests
  - [ ] Add CI job for e2e API tests

## M4 – Front-End Foundations
- [ ] **M4-4.1** Authentication screens (login, signup, forgot pwd)
  - [ ] Build form components with NativeWind
  - [ ] Wire Supabase Auth functions
  - [ ] Add validation & error handling
- [ ] **M4-4.2** React Navigation setup (stack + tab)
- [ ] **M4-4.3** Dashboard skeletons (Parent / Child)
  - [ ] Create role-based stack navigators
  - [ ] Add placeholder summary cards
  - [ ] Fetch cycle summary via React Query
- [ ] **M4-4.4** Global state (Zustand or Redux Toolkit)
  - [ ] Implement auth slice/store
  - [ ] Add family context slice
  - [ ] Persist state across reloads
- [ ] **M4-4.5** Basic unit + snapshot tests

## M5 – Collateral & Weekly Cycle Flow
- [ ] **M5-5.1** API endpoints: collateral, cycles, penalties
  - [ ] Collateral POST/GET
  - [ ] Penalty PATCH
- [ ] **M5-5.2** Parent UI: weekly collateral approval
  - [ ] Design wizard/modal component
  - [ ] Integrate collateral approval API
  - [ ] Add success/failure toast
- [ ] **M5-5.3** Child UI: risk selection & confirmation
  - [ ] Build slider/input component
  - [ ] Validate against weekly cap
  - [ ] Submit risk to API & show result
- [ ] **M5-5.4** Settlement logic (return unused minutes, streak update)
  - [ ] Worker cron / scheduled function to auto-settle
  - [ ] Ledger update queries
  - [ ] Notification trigger (optional)
- [ ] **M5-5.5** Ledger view (parent & child)
  - [ ] Implement paginated list
  - [ ] Fetch ledger data with React Query
  - [ ] Add role-based filters & styling
- [ ] **M5-5.6** Integration tests across front-end & API
  - [ ] Configure Playwright for Expo web
  - [ ] Seed test users & data
  - [ ] Implement risk→spend→settle flow test

## M6 – Real-Time Timer & Session Logging
- [ ] **M6-6.1** Countdown component (web + native)
  - [ ] Create reusable timer hook
  - [ ] Build Timer UI component
  - [ ] Handle pause/resume callbacks
- [ ] **M6-6.2** Persist session start/stop to API
  - [ ] Mutation endpoint for session logs
  - [ ] Offline queue & retry logic
  - [ ] Reconcile on reconnect
- [ ] **M6-6.3** Prevent screen sleep on mobile (expo-keep-awake)
- [ ] **M6-6.4** Unit tests & edge-case handling (refresh mid-session)
  - [ ] Test resume on refresh
  - [ ] Verify offline queue flush
  - [ ] Error-path coverage

## M7 – Reporting (PDF & CSV Exports)
- [ ] **M7-7.1** Worker utility to generate CSV via `json2csv`
  - [ ] Transform ledger JSON → CSV
  - [ ] Stream response via Worker streams
  - [ ] Unit tests for CSV output
- [ ] **M7-7.2** Worker utility to generate PDF via `pdf-lib`
  - [ ] Design templated layout
  - [ ] Embed fonts & table rendering
- [ ] **M7-7.3** Store export files to Supabase Storage or Cloudflare R2
  - [ ] Integrate with R2 SDK
  - [ ] Upload PDF/CSV with metadata
  - [ ] Generate signed URL
- [ ] **M7-7.4** Parent UI to request & download exports
  - [ ] Trigger export API call
  - [ ] Poll job status
  - [ ] Download & open file viewer

## M8 – PWA Polish & Offline Support
- [ ] **M8-8.1** Add Web manifest & icons
- [ ] **M8-8.2** Service worker caching (CRAFT)
  - [ ] Define caching strategy
  - [ ] Implement service worker in Expo web
  - [ ] Test ledger offline retrieval
- [ ] **M8-8.3** Lighthouse optimisation to ≥ 90
  - [ ] Audit performance issues
  - [ ] Tune bundle splitting / lazy-loading

## M9 – Stripe Subscription (Stretch)
- [ ] **M9-9.1** Stripe account & products setup
- [ ] **M9-9.2** Billing API via Cloudflare Worker
  - [ ] Create checkout session endpoint
  - [ ] Webhook handler
- [ ] **M9-9.3** Front-end paywall & subscription management
  - [ ] Integrate Stripe customer portal
  - [ ] Route guard for premium features
  - [ ] UI for sub status & cancel

---

**How to use this file**:  
Check off items as you complete them in PRs. Sub-tasks can be checked independently; a parent task is considered complete when all its sub-tasks are checked. 