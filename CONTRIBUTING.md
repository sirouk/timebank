# Contributing Guide – Parent-Child Behavioral Free Time Rewards System

Welcome! This document gives new collaborators everything they need to understand the project, abide by its strict rules, and contribute effectively.

---

## 1. Project Overview
A cross-platform web & mobile application that allows parents to reward children with "free-time" minutes based on behavior. The app supports:
* Weekly collateral risk & settlement workflow.
* Real-time session timer for children.
* Parent-controlled parameters, penalties, ledger, and exports (PDF/CSV).
* Multi-parent, multi-child families.
* Progressive Web App features and a future Stripe subscription add-on.

**Tech Stack**  
Front-end: React Native (Expo) + NativeWind (Tailwind), TypeScript.  
Back-end: Cloudflare Workers (TypeScript) – serverless.  
Database: Supabase (Postgres + Auth + Storage).  
CI/CD: GitHub Actions → Cloudflare Pages/Workers.  
Testing: Jest, React Native Testing Library, Playwright.  
Lints/Format: ESLint, Prettier, Husky.

---

## 2. Linear Workflow
Every task follows the same 6-step cycle:
1. **Discovery & Understanding** – read code/docs, gather facts.  
2. **Acknowledgement** – restate understanding & constraints.  
3. **Planning** – design details, acceptance criteria, subtasks.  
4. **Development** – code & tests in small PRs.  
5. **Testing** – local + CI validation.  
6. **Implementation** – merge, deploy, mark checklist.

> Never jump steps. Ask for clarification if anything is unclear.

---

## 3. Difficulty Scale & Task Decomposition
| Value | Label | Description |
| ----- | ------ | ----------- |
| 1 | Trivial | Boilerplate or tiny config tweak |
| 2 | Easy | Straight-forward implementation |
| 3 | Moderate | Integration between two modules |
| 4 | Hard | Multi-step integration, several edge cases |
| 5 | Very Hard | Complex business logic **and** heavy integration |

All tasks rated **≥3** are broken into subtasks ≤2. See `DEVELOPMENT_PLAN.md` for the full breakdown.

---

## 4. Milestones & Progress Tracking
High-level milestones live in **DEVELOPMENT_PLAN.md**. Day-to-day progress is tracked in **PROGRESS_CHECKLIST.md** using GitHub-style checkboxes.

> **Rule:** Only mark a parent task complete when every subtask is checked.

---

## 5. Core Rules & Guidelines (must-follow)
Below is the condensed rule-set you **must** follow. Violation may result in PR rejection or removal from the project.

### Managed Expectations
* Understand whether the change is new or modifies existing code. Collect adequate context first.  
* If information is missing, pause and ask questions before coding.

### Linear Development Process
Discovery → Understanding → Acknowledgement → Planning → Development → Testing → Implementation. No skipping.

### Code Quality & Architecture
* No hacks or workarounds; follow library/framework docs.  
* Single-responsibility functions; descriptive naming.  
* Self-documenting code plus comments for complex logic.  
* Proper error handling and edge-case coverage.

### Testing & CI
* All new or changed code **must** include tests (unit and/or e2e).  
* Tests run in local CLI and GitHub Actions CI. Breakage blocks merge.  
* Aim for growing coverage with each PR.

### Documentation & Dependencies
* Update README or docs for any new package, env-var, or architectural decision.  
* Ensure onboarding remains < 10 min for fresh clone.

### Environment & Safety
* Anticipate interactive prompts in terminal commands.  
* Never run destructive commands like `rm -rf /*`.  
* Wait for command completion before next step.  
* Use `.venv` or Node version managers if relevant.

### Data Management
* Define types for all data structures.  
* Handle data freshness & syncing strategies.  
* Remove stale data when no longer needed.

### Styling & UX
* Minimalist, clean UI with consistent component usage.  
* Fully responsive; Expo Web PWA capabilities.  
* Follow Tailwind/NativeWind conventions.

### Progressive Web App
* Include manifest, icons, service worker, offline caching.  
* Lighthouse scores ≥ 90 for Performance, PWA, Accessibility.

### Incremental Progress
* Prefer many small PRs to one monolithic PR.  
* Each PR should compile & pass tests independently.

---

## 6. Repository Structure (monorepo)
```
apps/
  mobile-web/   # Expo project (web + native)
workers/
  api/          # Cloudflare Worker code
packages/
  ui-components/  # Shared RN + Web components
  db-types/       # Generated DB types
  utils/          # Shared helpers
.github/
  workflows/    # CI pipeline
```

---

## 7. Getting Started Locally (M1-1.1 to M1-1.3)
```bash
# clone
$ git clone <repo>
$ cd parent-child-free-time

# install & setup monorepo (pnpm preferred)
$ pnpm install

# start Expo
$ pnpm --filter apps/mobile-web dev

# start Cloudflare Worker (separate tab)
$ pnpm --filter workers/api dev
```

Ensure `.env` files are in place (see README) before running.

---

## 8. How to Contribute a PR
1. **Branch** from `main` using naming: `feat/MX-X.Y-description` or `fix/MX-X.Y-description`.  
2. Mark corresponding checkbox(es) in `PROGRESS_CHECKLIST.md` (unchecked).  
3. Follow workflow steps. Add/ update tests.  
4. Open PR with link to checklist lines changed.  
5. Once approved & merged, the checkbox will be reviewed and ticked on `main`.

---

## 9. Contacts & Support
* Project Lead / Product Owner: <add-Slack/Email>.  
* Cloudflare & Supabase credentials: see LastPass vault.  
* CI Secrets maintenance: contact DevOps.

---

Happy coding & thank you for contributing responsibly!  
_Keep this guide up to date when the rules, workflows, or stack evolve._ 