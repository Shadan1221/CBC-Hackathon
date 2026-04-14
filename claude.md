# APEX — Autonomous Programming EXecution System
> **Version 2.0 · CBC Hackathon · Agentic Development Challenge**  
> *One prompt. Eight agents. A complete application — built autonomously.*

---

## What Is APEX?

APEX is a multi-agent software factory. You give it a single high-level prompt. It plans, architects, designs, builds, tests, reviews, and documents a complete full-stack application — without any further human input.

This repository is the blueprint. The judges bring the prompt. APEX builds the app.

---

## Immediate Activation Protocol

**When you receive an initial prompt, execute this sequence exactly:**

```
1. Invoke skill: /prompt-interpreter  → parse and structure the prompt
2. Invoke skill: /tech-stack-selector → choose the optimal tech stack
3. Write BUILD_PLAN.md               → full structured project plan
4. Write BUILD_STATE.json            → initialize build tracker
5. Run shell: git init               → initialize version control
6. Activate agent: orchestrator      → begin Phase 1
```

Do NOT write any application code directly. All code is generated through agent workflows.

---

## Agent Roster

| Agent | Activation Phase | Core Responsibility |
|---|---|---|
| **orchestrator** | Phase 1 — always first | Reads prompt, creates BUILD_PLAN.md, activates all downstream agents in correct order, manages BUILD_STATE.json |
| **architect** | Phase 2 | Designs project structure, API contracts, database schema, performance targets. Writes ARCHITECTURE.md |
| **frontend** | Phase 3 (parallel) | React + TypeScript UI. Reads DESIGN.md (auto-fetched), API_CONTRACTS.md. Builds all components |
| **backend** | Phase 3 (parallel) | Node.js + Express API. Reads API_CONTRACTS.md. Auth, WebSockets, validation, Swagger |
| **database** | Phase 3 (parallel) | Prisma schema + migrations + seed data. Reads SCHEMA.md from architect |
| **reviewer** | Phase 4 | Full code review + security audit + performance checks. FIXES all issues found before proceeding |
| **tester** | Phase 4 (parallel with reviewer) | Jest unit tests + Supertest integration tests + Playwright E2E. Must reach 80%+ coverage |
| **documenter** | Phase 5 | README.md, DEPLOYMENT.md, API docs, JSDoc/TSDoc, GitHub Actions CI, final git push |

---

## Execution Pipeline

### ▸ PHASE 1 — INIT (Orchestrator only)
**Goal:** Transform the raw prompt into a complete, structured build plan.

Steps:
1. Run `/prompt-interpreter` on the initial prompt
2. Run `/tech-stack-selector` on the parsed requirements
3. Create `BUILD_PLAN.md` with sections: Overview, Features, Tech Stack, File Tree (planned), API Routes (planned), Database Tables (planned), Agent Assignments, Timeline
4. Create `BUILD_STATE.json` (see State Management section below)
5. `git init` + create `.gitignore` for Node.js + React
6. Log: `[APEX] Phase 1 complete. BUILD_PLAN.md created. Activating architect.`
7. **Activate: architect**

Quality Gate ✓: `BUILD_PLAN.md` must exist and have all required sections before Phase 2.

---

### ▸ PHASE 2 — DESIGN (Architect)
**Goal:** Define the full architecture before any code is written.

Steps:
1. Read `BUILD_PLAN.md` thoroughly
2. Create the complete project directory skeleton (empty dirs, no files yet)
3. Write `ARCHITECTURE.md` — system design decisions, component diagram (ASCII), technology rationale
4. Write `API_CONTRACTS.md` — every endpoint: method, path, request schema, response schema, auth requirements, error codes
5. Write `SCHEMA.md` — all database tables, fields, types, relations, indexes
6. Write `PERF_BUDGETS.md` — performance targets (e.g., API response < 200ms, Lighthouse ≥ 90, bundle < 300KB)
7. Invoke `/design-system-fetcher` skill — fetches DESIGN.md from getdesign.md based on project type, saves as `DESIGN.md`
8. `git add -A && git commit -m "feat(architect): complete system architecture and contracts"`
9. Log: `[APEX] Phase 2 complete. Architecture defined. Activating build phase.`
10. **Activate in parallel: frontend, backend, database**

Quality Gate ✓: `API_CONTRACTS.md`, `SCHEMA.md`, `ARCHITECTURE.md`, `DESIGN.md` must all exist.

---

### ▸ PHASE 3 — BUILD (Frontend + Backend + Database — Parallel)
**Goal:** Generate all application code. Agents run simultaneously and communicate via contract files.

**Frontend Agent tasks:**
1. Read `DESIGN.md` (design system) and `API_CONTRACTS.md`
2. `npm create vite@latest frontend -- --template react-ts`
3. Install: tailwindcss, framer-motion, react-router-dom, axios, zustand, react-query, react-beautiful-dnd (or @dnd-kit/core), recharts, lucide-react
4. Configure Tailwind with design tokens from `DESIGN.md`
5. Build component library: Button, Input, Card, Modal, Badge, Avatar, Dropdown, Table, Chart, Skeleton
6. Build all pages defined in BUILD_PLAN.md
7. Implement WebSocket client for real-time features
8. Ensure WCAG 2.1 AA on all interactive elements (aria-labels, keyboard nav, focus rings, color contrast ≥ 4.5:1)
9. Write JSDoc on all exported components
10. `git add frontend/ && git commit -m "feat(frontend): complete UI implementation"`

**Backend Agent tasks:**
1. Read `API_CONTRACTS.md` and `SCHEMA.md`
2. `npm init` in `/backend`, install express, typescript, prisma, jsonwebtoken, bcryptjs, passport, passport-google-oauth20, socket.io, zod, swagger-jsdoc, swagger-ui-express, express-rate-limit, helmet, cors, dotenv, winston
3. Configure TypeScript (strict: true, no implicit any)
4. Implement ALL endpoints from `API_CONTRACTS.md` exactly as specified
5. Implement JWT auth + Google OAuth via Passport.js
6. Implement Socket.io server for real-time events
7. Add Zod validation schemas for all request bodies
8. Add rate limiting (100 req/15min per IP)
9. Add Swagger/OpenAPI 3.0 documentation at `/api/docs`
10. Add structured logging via Winston
11. `git add backend/ && git commit -m "feat(backend): complete API with auth and WebSockets"`

**Database Agent tasks:**
1. Read `SCHEMA.md`
2. Write complete `prisma/schema.prisma`
3. Run `npx prisma migrate dev --name init`
4. Create `prisma/seed.ts` with realistic seed data (at minimum: 2 users, 2 workspaces, 3 projects, 10 tasks)
5. Add `package.json` seed script
6. Write `scripts/db-reset.sh` and `scripts/db-seed.sh`
7. `git add backend/prisma/ && git commit -m "feat(database): schema, migrations, and seed data"`

Quality Gate ✓: All three agents must complete and their sections committed before Phase 4.

---

### ▸ PHASE 4 — REVIEW + TEST (Reviewer + Tester — Parallel)
**Goal:** Ensure quality, security, and correctness. Fix all issues found.

**Reviewer Agent tasks:**
1. Run `/code-standards` skill on all generated code
2. Run `/security-auditor` skill — check for: SQL injection, XSS, CSRF, hardcoded secrets, missing auth guards, unvalidated inputs, exposed sensitive data
3. Check frontend: no console.log in production code, no any types, all components accessible
4. Check backend: all routes protected (unless explicitly public), all inputs validated via Zod, no raw SQL without Prisma
5. **CRITICAL: FIX all issues found inline** — do not just report
6. Write `REVIEW_REPORT.md` — what was found, what was fixed, what score each category gets
7. `git add -A && git commit -m "fix(reviewer): address all code quality and security issues"`

**Tester Agent tasks:**
1. Read all source code
2. Write Jest unit tests for: all utility functions, all service layer functions, all auth logic
3. Write Supertest integration tests for: every API endpoint in `API_CONTRACTS.md`
4. Write Playwright E2E tests for: user signup, login, create workspace, create kanban board, add/move task, view analytics
5. Run: `npm test --coverage` — must achieve ≥ 80% coverage
6. If tests fail, fix the TEST (not the code) unless the code is genuinely wrong, then fix both
7. `git add -A && git commit -m "test: complete test suite with 80%+ coverage"`

Quality Gate ✓: All tests must pass. `REVIEW_REPORT.md` must show green. Coverage ≥ 80%.

---

### ▸ PHASE 5 — DOCUMENT + DEPLOY (Documenter)
**Goal:** Make the application production-ready with full documentation.

Steps:
1. Write `README.md` with: project overview, tech stack, setup instructions, environment variables, running locally, running tests, deployment, screenshots (ASCII diagrams), API reference overview
2. Write `DEPLOYMENT.md` with: Docker Compose full setup, environment variable reference, production checklist, scaling notes
3. Write `docker-compose.yml` with services: frontend (Nginx), backend (Node), postgres, redis
4. Write `Dockerfile` for both frontend and backend
5. Write `.github/workflows/ci.yml` — lint, type-check, test on every PR
6. Write `.env.example` with all required environment variables documented
7. Add TSDoc/JSDoc to all exported functions/classes missing documentation
8. Update `BUILD_STATE.json` — set all phases to "completed", set final metrics
9. Final commit: `git add -A && git commit -m "feat: APEX autonomous build complete — FlowBoard v1.0.0"`
10. Use GitHub MCP to push to remote repository

Quality Gate ✓: README.md, DEPLOYMENT.md, docker-compose.yml must exist. All tests still passing.

---

## Handoff Protocol

When one agent completes its work, it MUST:
1. Update `BUILD_STATE.json` — set its own status to `"completed"`, log its metrics
2. Write a `HANDOFF_[AGENTNAME].md` note with: what was built, key decisions made, any known limitations, what the next agent needs to know
3. Call the next agent explicitly: `[AGENT: next-agent-name] Your turn. Read HANDOFF_[PREV].md first.`

---

## State Management

APEX tracks build progress in `BUILD_STATE.json`. Initialize this structure in Phase 1:

```json
{
  "project_name": "<extracted from prompt>",
  "started_at": "<ISO timestamp>",
  "current_phase": 1,
  "agents": {
    "orchestrator": "completed",
    "architect": "pending",
    "frontend": "pending",
    "backend": "pending",
    "database": "pending",
    "reviewer": "pending",
    "tester": "pending",
    "documenter": "pending"
  },
  "quality_gates": {
    "phase1_complete": false,
    "phase2_complete": false,
    "phase3_complete": false,
    "phase4_tests_passing": false,
    "phase4_review_clean": false,
    "phase5_complete": false
  },
  "metrics": {
    "files_generated": 0,
    "lines_of_code": 0,
    "test_coverage_pct": 0,
    "security_issues_found": 0,
    "security_issues_fixed": 0,
    "lighthouse_score": 0
  }
}
```

Update this file after EVERY agent completes.

---

## Self-Healing Protocol

If any agent encounters an error or gets stuck:
1. **Retry 1:** Simplify the current subtask and attempt again
2. **Retry 2:** Break the task into smaller steps and rebuild
3. **Retry 3:** Log the error to `ERRORS.md` and escalate to orchestrator
4. **Orchestrator response:** Reassign the failing subtask to a fresh invocation of the same agent with added context

No agent ever fails silently. Every error is logged and addressed.

---

## Final Output Structure

After APEX completes, the repository contains:

```
[project-name]/
├── frontend/                     # React + TypeScript + Vite
│   ├── src/
│   │   ├── components/           # Reusable UI components
│   │   ├── pages/                # Route-level pages
│   │   ├── hooks/                # Custom React hooks
│   │   ├── services/             # API client layer
│   │   ├── store/                # Zustand state management
│   │   └── types/                # TypeScript type definitions
│   ├── tests/                    # Unit + E2E tests
│   ├── Dockerfile
│   └── package.json
├── backend/                      # Node.js + Express + Prisma
│   ├── src/
│   │   ├── routes/               # Express route definitions
│   │   ├── controllers/          # Business logic handlers
│   │   ├── middleware/           # Auth, validation, rate-limit
│   │   ├── services/             # Service layer
│   │   ├── sockets/              # Socket.io event handlers
│   │   └── types/                # TypeScript types
│   ├── prisma/
│   │   ├── schema.prisma
│   │   ├── migrations/
│   │   └── seed.ts
│   ├── tests/                    # Unit + integration tests
│   ├── Dockerfile
│   └── package.json
├── .github/
│   └── workflows/ci.yml          # CI/CD pipeline
├── docker-compose.yml            # Full deployment
├── .env.example                  # Environment variables template
├── README.md                     # Comprehensive project docs
├── DEPLOYMENT.md                 # Deployment guide
├── ARCHITECTURE.md               # Design decisions
├── API_CONTRACTS.md              # Full API specification
├── REVIEW_REPORT.md              # Quality audit results
├── DESIGN.md                     # UI design system (auto-fetched)
├── BUILD_PLAN.md                 # APEX-generated build plan
└── BUILD_STATE.json              # Build progress tracker
```

---

## Code Quality Standards (Non-Negotiable)

- **TypeScript strict mode** — `strict: true`, zero `any` types
- **ESLint + Prettier** — consistent formatting across all files
- **Error boundaries** — all React trees wrapped, all async/await try-caught
- **Input validation** — every API endpoint validates with Zod
- **Authentication** — every non-public route requires verified JWT
- **Environment variables** — no hardcoded secrets anywhere, ever
- **Accessibility** — ARIA labels, keyboard nav, focus management, 4.5:1 contrast
- **Mobile first** — all layouts work at 320px minimum viewport
- **Performance** — no n+1 queries, pagination on list endpoints, lazy loading on images

---

## MCP Tools Reference

| Tool | Server | Usage in APEX |
|---|---|---|
| **Filesystem** | `@modelcontextprotocol/server-filesystem` | Create dirs, write all source files, read contract files |
| **GitHub** | `@modelcontextprotocol/server-github` | Init repo, stage, commit, push, create PR |
| **Fetch** | `@modelcontextprotocol/server-fetch` | Fetch DESIGN.md from getdesign.md GitHub repo |

All three MCPs are pre-configured in `.claude/settings.json`.

---

*APEX was designed for the CBC Hackathon — Agentic Development Challenge.*  
*Built at BITS Pilani using Claude + MCP + multi-agent orchestration.*
