# COS Platform — MVP

> **Company Operating System** — an MCP-native SaaS for operational standardization of growing companies.

[![Status](https://img.shields.io/badge/status-pre--engagement-blue)](https://github.com/D4umak/cos-platform-mvp)
[![Architecture](https://img.shields.io/badge/architecture-MCP--first-6B46C1)](#architecture)
[![Delivery](https://img.shields.io/badge/delivery-8%20weeks%20%C2%B7%204%20sprints-green)](#timeline)

---

## What we're building

COS Platform guides companies through the full cycle of operational standardization using the **11-element architecture** from the Stetska & Kompanets methodology (9 years of practice, 500+ company diagnostics).

The 8-week MVP — delivered across **four two-week sprints** with milestone-gated acceptance — produces an **end-to-end diagnostic workflow**: a partner-consultant runs a live diagnostic through an AI client (Claude Desktop / ChatGPT Desktop / Cursor / Teams+Copilot), and the result renders as a shareable heat-map + Scalability Index™ + PDF report.

**Why this is different from every other ops-standardization tool on the market:** COS Platform is built *MCP-first*. Your consultants and clients don't log into yet another portal — they interact with COS through the AI tools they already use. This directly implements the core UX principle from the product spec: *"COS должна быть максимально изолирована от ежедневного взаимодействия с пользователями"* — COS is invisible; people just work.

---

## Architecture

**MCP-first hybrid**: an open-standard Model Context Protocol server at the core, with a thin web dashboard for the things that genuinely need pixels.

```
┌─────────────────────────────────────────────────────────────┐
│  AI Clients (Claude Desktop · ChatGPT · Cursor · Teams)     │
└──────────────────────────┬──────────────────────────────────┘
                           │  Model Context Protocol
                           │  (streamable HTTP · OAuth 2.1)
                           ▼
┌─────────────────────────────────────────────────────────────┐
│                    COS MCP Server                           │
│  ┌──────────────┬──────────────┬──────────────────────┐    │
│  │   Tools      │   Resources  │      Prompts         │    │
│  ├──────────────┼──────────────┼──────────────────────┤    │
│  │ • 11-element │ • Method     │ • Diagnostic         │    │
│  │   CRUD       │   book       │   interview          │    │
│  │ • Scalability│ • PROCESS-5  │ • Department card    │    │
│  │   Index™     │ • ABC formula│   builder            │    │
│  │ • KPI query  │ • SOPs       │ • Weekly review      │    │
│  └──────────────┴──────────────┴──────────────────────┘    │
└──────────────────────────┬──────────────────────────────────┘
                           │
                           ▼
┌─────────────────────────────────────────────────────────────┐
│  Postgres + RLS (multi-tenant) · Supabase                   │
│  Company → Department → 11 Elements → Roles → KPIs          │
└──────────────────────────┬──────────────────────────────────┘
                           │
                           ▼
┌─────────────────────────────────────────────────────────────┐
│  Thin Web Dashboard (React + Vite + Tailwind)               │
│  • Department heat-map (11 × red/yellow/green)              │
│  • Scalability Index™ gauge                                 │
│  • Org chart with element completeness                      │
│  • Branded PDF diagnostic report                            │
└─────────────────────────────────────────────────────────────┘
```

See [`docs/architecture.md`](docs/architecture.md) for the deeper dive.

---

## The 11-element architecture

Every department is standardized against the same 11-element checklist. If not all 11 are in place — the function is not standardized.

| #  | Element | What it captures |
|----|---------|------------------|
| 1  | ЦКП (CKP) | The department's core valuable deliverable |
| 2  | KPI (department) | Metrics at the function level |
| 3  | Roles | Who does what — zones of responsibility |
| 4  | KPI (roles) | Metrics per position |
| 5  | Business processes | Step-by-step value production |
| 6  | Instructions / SOPs | Detailed execution guides |
| 7  | Cost | Operation cost in money and time |
| 8  | Motivation | KPI → compensation link |
| 9  | Reporting | How and to whom the department reports |
| 10 | Budget | Resource envelope |
| 11 | Risk map | What can break and how to respond |

---

## Stack

| Layer | Choice | Why |
|-------|--------|-----|
| MCP server | TypeScript · `@modelcontextprotocol/sdk` | First-class SDK, strong types, broad client support |
| Transport | Streamable HTTP with OAuth 2.1 | Remote-MCP-ready, multi-tenant-safe |
| Database | Postgres via Supabase | RLS for multi-tenancy, auth included, Vercel-friendly |
| Web dashboard | React + Vite + Tailwind + shadcn/ui | Fast to build, production-grade defaults |
| PDF | `@react-pdf/renderer` | Programmatic, brandable |
| Hosting | Vercel (web) + Supabase (db) | Zero-ops for MVP |
| AuthN/Z | Supabase Auth + MCP OAuth | Same identity across web and MCP |

---

## Timeline

Four two-week sprints. Each sprint ends with a demo and milestone acceptance — the client only pays for accepted work.

| Sprint | Weeks | Milestone |
|--------|-------|-----------|
| **1 · Foundation + MCP Core** | 1–2 | Architecture · data model · MCP server skeleton · first 5 tools · Supabase schema · RLS · staging deploy · tested in Claude Desktop |
| **2 · Full MCP Methodology Layer** | 3–4 | All 11 element tools · Scalability Index™ · diagnostic-interview prompt · methodology resources · end-to-end diagnostic works |
| **3 · Dashboard + Visualization** | 5–6 | Production web dashboard · heat-map · Scalability Index gauge · org chart · PDF export · Vercel deploy with custom domain |
| **4 · Integration + Polish + Launch** | 7–8 | Partner onboarding · integration tests · investor demo video · technical documentation · live handover · 2 weeks post-launch support |

Each sprint has written acceptance criteria agreed before the sprint starts.

---

## Team

**Denys Chumak** — engineering, architecture, product lead
Based in the UK, working with teams in UA/PL/UK. Specializes in fast MVPs, AI-native architectures, the MCP protocol, and React/TypeScript/Node/Python.

**Andrii Pavlov** — design, brand vision, content
Owns product design, visual identity, brand strategy, and content production — including the CEO Dashboard look and feel, the branded PDF diagnostic report, the investor demo video, and pitch-deck materials.

### Prior work

**Our own product — created together from 0 → market:**

- **[DreamApp](https://dreamapp.io)** — Denys and Andrii built this as founders, end-to-end: architecture, design, brand, and go-to-market

**Agency engagements — shipped for clients:**

- **tip.live** — SaaS platform delivered end-to-end (engineering by Denys, design + brand by Andrii)
- **Sphotonix** — design, branding, and website for a deep-tech startup (product engineering handled by the client's in-house team)

### Why this team shape matters for COS specifically

We've been on both sides: as founders building our own product (DreamApp), so we understand Krystyna's context from the inside — Seed-round pressure, investor expectations, the pace MVPs demand. And as a client-delivery product team (tip.live, Sphotonix), where we work under contract with fixed scope and timelines.

Beyond that: your consulting partners hand branded PDF diagnostics to their clients — their reputation lives on that document. Your Seed-round investors form a first impression from the demo video and UI in the dataroom. Having a designer + brand lead on the team from Sprint 1 means those artefacts look McKinsey-grade from day one, not retrofitted later.

---

## Repo structure

```
cos-platform-mvp/
├── docs/
│   ├── architecture.md       ← MCP-first deep dive
│   ├── data-model.md         ← 11-element schema
│   ├── roadmap.md            ← Stage 1 → Stage 2.x plan
│   └── decisions/            ← ADRs (architecture decision records)
├── mcp-server/               ← MCP server (TypeScript)
├── web/                      ← Dashboard (React)
├── supabase/                 ← Migrations, seed data, RLS policies
└── .github/workflows/        ← CI
```

---

## Status

**Pre-engagement.** This repository is the public home of the COS Platform MVP engagement between Stetska Consulting and Denys Chumak + Andrii Pavlov.

Full commercial terms are documented separately and shared directly with Stetska Consulting.

---

## Contact

- Denys Chumak — GitHub [@D4umak](https://github.com/D4umak) · Telegram [@previously_unknown](https://t.me/previously_unknown)
- Andrii Pavlov — TBD

For a **free 45-minute discovery call** with Krystyna & Viktor before any commitment, reach out via Telegram. A tailored Statement of Work follows within 2 business days of the call.

---

<sub>Built on the [COS methodology](https://stetskaconsulting.com) by Krystyna Stetska and Viktor Kompanets. The methodology, the 11-element architecture, the Scalability Index™, and the PROCESS-5 standard are the intellectual property of Stetska Consulting.</sub>
