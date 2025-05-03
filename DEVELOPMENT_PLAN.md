# Development Plan – Parent-Child Behavioral Free Time Rewards System

## Difficulty Scale
| Value | Label       | Description                                 |
|-------|-------------|---------------------------------------------|
| 1     | Trivial     | Simple configuration or boilerplate change. |
| 2     | Easy        | Straight-forward implementation, minimal complexity. |
| 3     | Moderate    | Requires integration between two components or deeper understanding. |
| 4     | Hard        | Multi-step integration, non-trivial edge-cases, or new patterns. |
| 5     | Very Hard   | Complex business logic **and** heavy integration; deserves to be decomposed into subtasks. |

_All steps rated **4** or **5** are further decomposed into smaller subtasks to keep each unit ≤ 3._

---

## Milestone Overview
| ID | Title                                                     | Primary Goal                                                |
|----|-----------------------------------------------------------|-------------------------------------------------------------|
| M1 | Repository & CI Scaffold                                  | Monorepo layout, Expo + Worker boilerplates, automated CI   |
| M2 | Database & Auth Foundation                                | Supabase Postgres schema, auth setup, typed client          |
| M3 | Cloudflare Worker – Core API                              | Auth-protected CRUD endpoints for base resources            |
| M4 | Front-End Foundations (Login + Dashboard Skeleton)        | Parent & Child auth flows, initial screens                  |
| M5 | Collateral & Weekly Cycle Flow (Full-Stack)               | Risk time, penalties, settlement, ledger UI                 |
| M6 | Real-Time Timer & Session Logging                         | In-browser countdown, auto session creation                 |
| M7 | Reporting (PDF & CSV Exports)                             | Generate & download cycle reports                           |
| M8 | PWA Polish & Offline Support                              | Lighthouse ≥ 90, service worker, install prompts            |
| M9 | (Stretch) Subscription Billing via Stripe                 | SaaS monetization                                           |

---

## Detailed Milestones & Tasks

### M1 – Repository & CI Scaffold
| # | Task | Difficulty | Notes / Subtasks |
|---|------|------------|------------------|
| 1.1 | Initialize monorepo (pnpm or npm workspaces) | 2 | Create `apps/`, `workers/`, `packages/` folders. |
| 1.2 | Set up **Expo** project (`apps/mobile-web`) in TypeScript | 3 | **Subtasks:**<br>• Run `expo init` with blank TS template (2)<br>• Configure web target & test in browser (2)<br>• Add NativeWind & Tailwind config (2) |
| 1.3 | Add **Cloudflare Worker** template (`workers/api`) | 3 | **Subtasks:**<br>• Run `wrangler init` TypeScript template (2)<br>• Set up local dev script (`wrangler dev`) (2)<br>• Add hello-world endpoint returning JSON (2) |
| 1.4 | Configure shared **TypeScript** project references | 4 | **Subtasks:**<br>• Align tsconfigs across packages (3)<br>• Path aliases & module resolution check (3) |
| 1.5 | Install & configure **ESLint**, **Prettier**, **Husky** hooks | 2 | Standard configs. |
| 1.6 | Add **Jest** + React Native Testing Library | 3 | **Subtasks:**<br>• Install Jest & preset-expo (2)<br>• Configure monorepo jest.config (2)<br>• Add sample component test (2) |
| 1.7 | Create **GitHub Actions** workflow | 4 | **Subtasks:**<br>• Install deps & cache node_modules (2)<br>• Run lint & unit tests (2)<br>• Cloudflare Pages deploy (3) |

### M2 – Database & Auth Foundation
| # | Task | Difficulty | Notes / Subtasks |
|---|------|------------|------------------|
| 2.1 | Spin up Supabase project & link remote Postgres | 2 | Configure environment variables locally and in CI. |
| 2.2 | Define initial **database schema** | 4 | **Subtasks:**<br>• Write SQL or Prisma schema for families, parents, children (3)<br>• Add parameters, cycles, collateral, sessions tables (3) |
| 2.3 | Generate typed client (`db-types` pkg) | 3 | **Subtasks:**<br>• Select generator (Prisma or Supabase CLI) (2)<br>• Generate types into `packages/db-types` (2)<br>• Wire workspace import paths (2) |
| 2.4 | Configure **Supabase Auth** (email + password) | 3 | **Subtasks:**<br>• Enable email login in Supabase (2)<br>• Add RLS policies for parents/children (2)<br>• Test sign-up & data access (2) |
| 2.5 | Store secrets in GitHub & Cloudflare | 2 | `supabase_url`, `anon_key`, `service_role`. |

### M3 – Cloudflare Worker – Core API
| # | Task | Difficulty | Notes / Subtasks |
|---|------|------------|------------------|
| 3.1 | Implement middleware for Supabase JWT verification | 4 | **Subtasks:**<br>• Parse Supabase JWT, validate (3)<br>• Attach user context to request (2) |
| 3.2 | CRUD endpoints: families, children, parameters | 4 | **Subtasks:**<br>• Families routes (3)<br>• Children routes (3)<br>• Parameters routes with Zod validation (3) |
| 3.3 | End-to-end tests for API with `miniflare` | 3 | **Subtasks:**<br>• Configure miniflare for Worker (2)<br>• Write happy-path & auth-failure tests (2)<br>• Add CI job for e2e API tests (2) |

### M4 – Front-End Foundations
| # | Task | Difficulty | Notes / Subtasks |
|---|------|------------|------------------|
| 4.1 | Authentication screens (login, signup, forgot pwd) | 3 | **Subtasks:**<br>• Build form components with NativeWind (2)<br>• Wire Supabase Auth functions (2)<br>• Add validation & error handling (2) |
| 4.2 | React Navigation setup (stack + tab) | 2 | Basic skeleton. |
| 4.3 | Dashboard skeletons (Parent / Child) | 3 | **Subtasks:**<br>• Create role-based stack navigators (2)<br>• Add placeholder summary cards (2)<br>• Fetch cycle summary via React Query (2) |
| 4.4 | Global state (Zustand or Redux Toolkit) | 3 | **Subtasks:**<br>• Implement auth slice/store (2)<br>• Add family context slice (2)<br>• Persist state across reloads (2) |
| 4.5 | Basic unit + snapshot tests | 2 | Ensure components render. |

### M5 – Collateral & Weekly Cycle Flow
| # | Task | Difficulty | Notes / Subtasks |
|---|------|------------|------------------|
| 5.1 | API endpoints: collateral, cycles, penalties | 4 | **Subtasks:**<br>• Collateral POST/GET (3)<br>• Penalty PATCH (3) |
| 5.2 | Parent UI: weekly collateral approval | 3 | **Subtasks:**<br>• Design wizard/modal component (2)<br>• Integrate collateral approval API (2)<br>• Add success/failure toast (2) |
| 5.3 | Child UI: risk selection & confirmation | 3 | **Subtasks:**<br>• Build slider/input component (2)<br>• Validate against weekly cap (2)<br>• Submit risk to API & show result (2) |
| 5.4 | Settlement logic (return unused minutes, streak update) | 5 | **Subtasks:**<br>• Worker cron / scheduled function to auto-settle (3)<br>• Ledger update queries (3)<br>• Notification trigger (optional) (3) |
| 5.5 | Ledger view (parent & child) | 3 | **Subtasks:**<br>• Implement paginated list (2)<br>• Fetch ledger data with React Query (2)<br>• Add role-based filters & styling (2) |
| 5.6 | Integration tests across front-end & API | 3 | **Subtasks:**<br>• Configure Playwright for Expo web (2)<br>• Seed test users & data (2)<br>• Implement risk→spend→settle flow test (2) |

### M6 – Real-Time Timer & Session Logging
| # | Task | Difficulty | Notes / Subtasks |
|---|------|------------|------------------|
| 6.1 | Countdown component (web + native) | 3 | **Subtasks:**<br>• Create reusable timer hook (2)<br>• Build Timer UI component (2)<br>• Handle pause/resume callbacks (2) |
| 6.2 | Persist session start/stop to API | 3 | **Subtasks:**<br>• Mutation endpoint for session logs (2)<br>• Offline queue & retry logic (2)<br>• Reconcile on reconnect (2) |
| 6.3 | Prevent screen sleep on mobile (expo-keep-awake) | 2 | Minor API. |
| 6.4 | Unit tests and edge-case handling (refresh mid-session) | 3 | **Subtasks:**<br>• Test resume on refresh (2)<br>• Verify offline queue flush (2)<br>• Error-path coverage (2) |

### M7 – Reporting (PDF & CSV Exports)
| # | Task | Difficulty | Notes / Subtasks |
|---|------|------------|------------------|
| 7.1 | Worker utility to generate CSV via `json2csv` | 3 | **Subtasks:**<br>• Transform ledger JSON → CSV (2)<br>• Stream response via Worker streams (2)<br>• Unit tests for CSV output (2) |
| 7.2 | Worker utility to generate PDF via `pdf-lib` | 4 | **Subtasks:**<br>• Design templated layout (3)<br>• Embed fonts & table rendering (3) |
| 7.3 | Store export files to Supabase Storage or Cloudflare R2 | 3 | **Subtasks:**<br>• Integrate with R2 SDK (2)<br>• Upload PDF/CSV with metadata (2)<br>• Generate signed URL (2) |
| 7.4 | Parent UI to request & download exports | 3 | **Subtasks:**<br>• Trigger export API call (2)<br>• Poll job status (2)<br>• Download & open file viewer (2) |

### M8 – PWA Polish & Offline Support
| # | Task | Difficulty | Notes / Subtasks |
|---|------|------------|------------------|
| 8.1 | Add Web manifest & icons | 2 | `expo generate:web-icons`. |
| 8.2 | Service worker caching (CRAFT) | 3 | **Subtasks:**<br>• Define caching strategy (2)<br>• Implement service worker in Expo web (2)<br>• Test ledger offline retrieval (2) |
| 8.3 | Lighthouse optimisation to ≥ 90 | 4 | **Subtasks:**<br>• Audit performance issues (3)<br>• Tune bundle splitting / lazy-loading (3) |

### M9 – Stripe Subscription (Stretch)
| # | Task | Difficulty | Notes / Subtasks |
|---|------|------------|------------------|
| 9.1 | Stripe account & products setup | 2 | Sandbox first. |
| 9.2 | Billing API via Cloudflare Worker | 4 | **Subtasks:**<br>• Create checkout session endpoint (3)<br>• Webhook handler (3) |
| 9.3 | Front-end paywall & subscription management | 3 | **Subtasks:**<br>• Integrate Stripe customer portal (2)<br>• Route guard for premium features (2)<br>• UI for sub status & cancel (2) |

---

## Execution Strategy
We will tackle one milestone at a time, following the recursive linear workflow for each **task** (or **subtask** if difficulty ≥ 4):
1. Discovery & Understanding
2. Acknowledgement
3. Planning (detailed design / acceptance criteria)
4. Development (code + tests)
5. Testing (local & CI)
6. Implementation (merge & deploy)

Progress will be recorded in project issues / pull requests corresponding to these task IDs (e.g., `M1-1.2`).

---

_This document should live at the root of the repository and evolve via pull requests as requirements change or tasks are completed._ 