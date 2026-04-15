# COS Platform — MVP

> **Company Operating System** — an MCP-native SaaS for operational standardization of growing companies.

[![Status](https://img.shields.io/badge/status-pre--engagement-blue)](https://github.com/D4umak/cos-platform-mvp)
[![Architecture](https://img.shields.io/badge/architecture-MCP--first-6B46C1)](#architecture)
[![Target delivery](https://img.shields.io/badge/delivery-30%20days-green)](#timeline)

---

## What we're building

COS Platform guides companies through the full cycle of operational standardization using the **11-element architecture** from the Stetska & Kompanets methodology (9 years of practice, 500+ company diagnostics).

The 30-day MVP delivers an **end-to-end diagnostic workflow**: a partner-consultant runs a live diagnostic through an AI client (Claude Desktop / ChatGPT Desktop / Cursor / Teams+Copilot), and the result renders as a shareable heat-map + Scalability Index™ + PDF report.

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

A single 30-day push, delivered end-to-end.

| Week | Milestone |
|------|-----------|
| **1** | Architecture · MCP server skeleton · first 3 tools · Supabase schema · tested in Claude Desktop |
| **2** | Full MCP layer (11 elements + Scalability Index + diagnostic-interview prompt + methodology resources) · end-to-end demo |
| **3** | Web dashboard (heat-map + Scalability Index gauge + org chart + PDF export) · Vercel deploy |
| **4** | Polish · integration tests · partner onboarding flow · demo video · technical handover |

---

## Team

**Denys Chumak** — fullstack, architect, product lead
Co-founder of [tip.live](https://tip.live) and [Sphotonix](https://sphotonix.com). Specializes in fast MVPs, AI-native architectures, and the MCP protocol.

**Andrii Pavlov** — fullstack, backend, DevOps
Co-founder of tip.live and Sphotonix with Denys. Specializes in backend architecture, databases, DevOps, and systems integration.

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

- Denys Chumak — [@D4umak](https://github.com/D4umak)
- Andrii Pavlov — TBD

---

<sub>Built on the [COS methodology](https://stetskaconsulting.com) by Krystyna Stetska and Viktor Kompanets. The methodology, the 11-element architecture, the Scalability Index™, and the PROCESS-5 standard are the intellectual property of Stetska Consulting.</sub>
