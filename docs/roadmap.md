# Roadmap

## Stage 1 · MVP — 4 sprints × 2 weeks = 8 weeks

End-to-end diagnostic workflow delivered through MCP + thin dashboard.
Milestone-gated: client pays per accepted sprint.

---

### 🏗️ Sprint 1 · Weeks 1–2 · Foundation + MCP Core

**Goal:** Working MCP server with basic Department Card CRUD, accessible via Claude Desktop.

**Deliverables:**

- [ ] Discovery sessions with Krystyna & Viktor; signed scope
- [ ] Architecture decision: stack, integration with existing Stetska Consulting DB
- [ ] Data model: Company → Department → 11 Elements → Roles → KPIs
- [ ] Supabase project with RLS policies and initial migrations
- [ ] MCP server skeleton (TypeScript, streamable HTTP, OAuth 2.1)
- [ ] Tools shipped: `create_department_card`, `set_element`, `get_department_card`, `list_departments`, `list_companies`
- [ ] GitHub Actions CI configured
- [ ] Staging deployment

**Acceptance criteria:**

- Partner installs COS MCP in Claude Desktop in <5 minutes
- Full Department Card CRUD through conversation with Claude
- Data persists in Supabase; RLS isolates tenants
- End-to-end CI test passes

---

### 🧠 Sprint 2 · Weeks 3–4 · Full MCP Methodology Layer

**Goal:** All 11 elements available as tools, Scalability Index™ computed, methodology embedded as LLM context.

**Deliverables:**

- [ ] All 11 elements as MCP tools:
  - [ ] CKP (core valuable deliverable)
  - [ ] KPI (department)
  - [ ] Roles
  - [ ] KPI (roles)
  - [ ] Business processes
  - [ ] Instructions / SOPs
  - [ ] Cost (ABC method)
  - [ ] Motivation
  - [ ] Reporting
  - [ ] Budget
  - [ ] Risk map
- [ ] `compute_scalability_index(department_id)` — the killer IP metric, cached with invalidation
- [ ] MCP Prompts library:
  - [ ] `run-diagnostic-interview` — 45-min structured interview
  - [ ] `build-department-card-from-transcript` — extract 11 elements from transcript
  - [ ] `generate-sop` — generate SOP from process description
  - [ ] `weekly-review-template` — weekly review builder
- [ ] MCP Resources library:
  - [ ] Methodology book excerpts
  - [ ] PROCESS-5 standard with examples
  - [ ] ABC cost formula with worked examples
  - [ ] 11-element descriptions with best practices

**Acceptance criteria:**

- Partner runs complete diagnostic session with Claude and ends with filled Department Card — zero manual data entry
- Scalability Index computes correctly against sample data
- All prompts auto-pull correct resources for context

---

### 🎨 Sprint 3 · Weeks 5–6 · Dashboard + Visualization

**Goal:** Production web dashboard for visual stakeholders — CEOs, investors, clients.

**Deliverables:**

- [ ] React + Vite + Tailwind + shadcn/ui web app
- [ ] Supabase Auth (same identity as MCP OAuth)
- [ ] **CEO Dashboard:**
  - [ ] Department heat-map (11 × red/yellow/green)
  - [ ] Scalability Index gauge
  - [ ] Org chart with element completeness
  - [ ] Key KPI summary
- [ ] **Department detail view:**
  - [ ] Full 11-element card
  - [ ] Inline edit for approved elements
  - [ ] Audit findings list
- [ ] Branded PDF diagnostic report export (react-pdf)
- [ ] Vercel deployment with custom domain

**Acceptance criteria:**

- CEO logs in and sees organizational health in <5 seconds
- Heat-map visually conveys state across all departments and elements
- PDF export renders correctly for tested companies
- Multi-tenant isolation verified

---

### 🚀 Sprint 4 · Weeks 7–8 · Integration + Polish + Launch

**Goal:** Partner-ready, investor-demo-ready, production-grade.

**Deliverables:**

- [ ] Partner onboarding flow: install MCP <5 min, first diagnostic <30 min
- [ ] Integration tests (Playwright + MCP Inspector)
- [ ] Error handling, loading/empty states across UI
- [ ] 3-minute investor demo video (narrated walkthrough)
- [ ] Technical documentation:
  - [ ] API reference (MCP tools, resources, prompts)
  - [ ] DB schema + migration history
  - [ ] Deployment guide for Viktor's team
  - [ ] Stage 2 implementation notes
- [ ] Live handover session with Krystyna + Viktor (60 min)
- [ ] 2 weeks post-launch support: bug fixes, minor adjustments
- [ ] *Optional:* pilot with one real client partner during Sprint 4

**Acceptance criteria:**

- A new partner-consultant installs and uses the platform without hands-on help
- Viktor deploys a new version independently
- Demo video ready to share with investors
- All critical user paths covered by tests

---

## Stage 2 · Expand (optional, post-MVP)

Same cadence: 8-week blocks of four two-week sprints. Client continues pay-per-sprint.

### Stage 2.1 · Workflow + Integrations (weeks 9–16)

- [ ] Workflow Engine for deterministic and configurable processes
- [ ] Rhythm Management module (standups, weeklies, MBR, QBR templates)
- [ ] Jira integration (read KPI signals)
- [ ] Google Workspace integration (docs, calendar, tasks)
- [ ] Upgraded multi-tenancy for partner-consultant workflows

### Stage 2.2 · Analytics (weeks 17–24)

- [ ] CEO Dashboard v2 with cross-department KPI aggregation
- [ ] Analytics engine (metric hooks from every subsystem)
- [ ] Audit module (planned + on-demand + self-audit)
- [ ] Anomaly detection for KPI drift

### Stage 2.3 · Partner portal + flywheel (weeks 25–32)

- [ ] SOM partner portal (project management, client handoff)
- [ ] White-label mode for consulting firms
- [ ] Revenue-share tracking and payouts
- [ ] Certificate management (COS Consultant levels 1/2/3)

---

## Stage 3 · AI + Autopilot (post–Stage 2)

- [ ] Recommendation engine across all subsystems
- [ ] Economic modeling ("should we automate function X?")
- [ ] Behavioral modeling ("given parameter set X, recommend Y")
- [ ] First COS Autopilot agents in production

## Stage 4 · Scale

- [ ] Self-service onboarding
- [ ] Industry template marketplace
- [ ] M&A due-diligence module (scalability score → valuation multiplier)
- [ ] Franchise model for consulting partners
- [ ] Mobile app for front-line employees

---

## Principle

> Stage 0 is critical. Before writing code, the team must: (1) align on all open questions; (2) produce detailed sub-specs per subsystem; (3) define MVP scope for the first client; (4) pick the technology stack.
>
> COS is not a project with a deadline — it's a product with iterations.

— from the product specification, §8

---

## Why 4 sprints instead of one 30-day push

- **Client risk capped at one sprint.** If after Sprint 1 the collaboration isn't working, the client has paid for one sprint and received a working MCP server they can continue building on.
- **Milestone artefact every 2 weeks** — demoable to investors or clients without waiting for the final day.
- **Faster feedback loop.** Viktor and Krystyna test every 2 weeks, course-correcting early.
- **Quality.** 8 weeks is a realistic envelope for a production-grade MVP with the feature scope above, vs. compressing into 4 weeks.
