# Roadmap

## Stage 1 · MVP (current — 30 days)

End-to-end diagnostic workflow delivered through MCP + thin dashboard.

### Week 1 · Architecture + Core

- [ ] Discovery sessions with Krystyna & Viktor
- [ ] Data model: Company → Department → 11 Elements → Roles → KPIs
- [ ] MCP server skeleton (TypeScript, streamable HTTP, OAuth 2.1)
- [ ] Tools shipped: `create_department_card`, `set_element`, `get_department_card`
- [ ] Supabase project + RLS policies + initial migrations
- [ ] End-to-end test in Claude Desktop

### Week 2 · Full MCP layer

- [ ] All 11 elements as tools (CKP, KPI × 2, roles, processes, SOPs, cost, motivation, reporting, budget, risks)
- [ ] `compute_scalability_index(dept_id)` — the killer IP metric
- [ ] Prompts: diagnostic interview, department-card-from-transcript, SOP generator, weekly review
- [ ] Resources: methodology excerpts, PROCESS-5 standard, ABC formula
- [ ] End-to-end: partner runs a live diagnostic in Claude Desktop

### Week 3 · Dashboard + visualization

- [ ] React + Vite + Tailwind web app
- [ ] Department heat-map (11 elements × red/yellow/green)
- [ ] Scalability Index gauge on CEO Dashboard
- [ ] Org chart with element completeness indicators
- [ ] Branded PDF diagnostic report export
- [ ] Vercel deployment with Supabase Auth

### Week 4 · Polish + handover

- [ ] Partner onboarding: MCP install in Claude Desktop in <5 min
- [ ] Integration tests, edge cases, error handling
- [ ] 3-minute demo video for investors
- [ ] Technical documentation (architecture, API, schema, Stage 2 plan)
- [ ] Live handover demo with Krystyna + Viktor

---

## Stage 2.1 · Workflow + Integrations (months 2–3)

- [ ] Workflow Engine for deterministic and configurable processes
- [ ] Rhythm Management module (standups, weeklies, MBR, QBR templates)
- [ ] Jira integration (read KPI signals)
- [ ] Google Workspace integration (docs, calendar, tasks)
- [ ] Upgraded multi-tenancy for partner-consultant workflows

## Stage 2.2 · Analytics (months 4–5)

- [ ] CEO Dashboard v2 with cross-department KPI aggregation
- [ ] Analytics engine (metric hooks from every subsystem)
- [ ] Audit module (planned + on-demand + self-audit)
- [ ] Anomaly detection for KPI drift

## Stage 2.3 · Partner portal + flywheel (months 6–7)

- [ ] SOM partner portal (project management, client handoff)
- [ ] White-label mode for consulting firms
- [ ] Revenue-share tracking and payouts
- [ ] Certificate management (COS Consultant levels 1/2/3)

---

## Stage 3 · AI + Autopilot (months 8–12)

- [ ] Recommendation engine across all subsystems
- [ ] Economic modeling ("should we automate function X?")
- [ ] Behavioral modeling ("given parameter set X, recommend Y")
- [ ] First COS Autopilot agents in production

## Stage 4 · Scale (months 12+)

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
