# Architecture — MCP-first hybrid

## The design principle

The COS product specification states a core UX rule:

> "COS должна быть максимально изолирована от ежедневного взаимодействия с пользователями. Люди не должны чувствовать, что они работают в системе стандартизации — они просто работают."

> *"COS must be maximally isolated from daily user interaction. People shouldn't feel they're working in a standardization system — they just work."*

A classical SaaS portal architecturally contradicts this principle. The more features you add, the more users have to live in your UI. MCP flips this: users consume COS through the AI tools they already use, and the "UI" is their existing tool's chat surface.

This document captures why MCP-first is the right architecture for COS specifically, and how the hybrid design works.

---

## Why MCP-first for COS specifically

Four structural reasons — more than "AI is on trend."

### 1. The 11-element architecture is already tool-shaped

Every element is a structured CRUD operation with validation rules. These map 1:1 to MCP tools:

```ts
create_department_card({ company_id, department_name })
set_element({ dept_id, element: "ckp" | "kpi" | "roles" | ..., value })
compute_scalability_index({ dept_id })
list_sops_for_role({ role_id })
record_audit_finding({ dept_id, severity, note })
```

The methodology **is** the tool surface. There's no translation layer between "what a consultant does" and "what an API exposes."

### 2. The methodology is retrieval-shaped

Nine years of practice, the book, 11 innovations, PROCESS-5, ABC formulas — this is a knowledge base. MCP `resources` + `prompts` turn this into contextual guidance the LLM pulls on demand:

- **Resource:** `cos://methodology/process-5` — the PROCESS-5 standard definition
- **Resource:** `cos://methodology/abc-formula` — ABC cost calculation formula + worked examples
- **Prompt:** `run-diagnostic-interview` — guides the LLM through a 45-minute structured interview
- **Prompt:** `build-department-card-from-transcript` — extracts 11 elements from a meeting transcript

The moat: *"Ask Claude about operational standardization and it gives you Stetska-grade answers, because the COS MCP is active in your workspace."*

### 3. Partner-consultants are the ideal MCP users

SOM partners are knowledge workers who already live in LLMs. Giving them Claude Desktop + the COS MCP turns a week-long diagnostic into a same-day exercise:

| Step | Before (manual) | After (COS MCP) |
|------|-----------------|-----------------|
| Interview | 3–5 hours × 2 rounds | 1 hour, LLM captures structured data |
| Transcription | 2 hours | Automatic |
| Card creation | 1 day of typing | Minutes — `build-department-card-from-transcript` |
| Heat-map | Manual Excel | Auto-rendered from Scalability Index |
| PDF report | 1 day | One click from dashboard |

**5–10× productivity gain per partner** — which directly accelerates the SOM flywheel.

### 4. "Standardize → Document → Automate → Optimize" is an LLM pipeline

The 2nd innovation in the COS methodology (strict sequence) is exactly what an agent is good at:

1. **Standardize** — run structured interview, fill 11-element card
2. **Document** — generate SOPs and role descriptions
3. **Automate** — deploy workflows based on standardized data
4. **Optimize** — monitor metrics, flag deviations, recommend changes

MCP lets this pipeline run in the client's existing AI tool, not in yet another portal.

---

## Why hybrid, not MCP-only

Some things genuinely need pixels. A CEO looking at a heat-map in a 30-second glance doesn't want to chat. A shareable PDF for board meetings needs real layout. An org chart showing 11-element completeness across 20 departments is a visualization problem.

So: **MCP for the interactive workflow**, **thin web dashboard for the visualization layer**, **one backend**.

| Surface | What lives there |
|---------|------------------|
| **MCP (Claude Desktop etc.)** | Diagnostic interview · Department Card CRUD · SOP generation · Ad-hoc queries · Weekly review prep |
| **Web dashboard** | CEO Dashboard · Heat-map · Scalability Index gauge · Org chart · PDF report · Billing |
| **Shared Postgres** | Single source of truth — both surfaces read/write the same data |

---

## Multi-tenancy

One Supabase project. RLS policies enforce tenant isolation at the database level:

- `company` → scopes by `org_id`
- `department` → joins to `company`
- All other tables → cascade via `department_id` or `company_id`

MCP OAuth 2.1 issues per-user tokens scoped to their workspace. A partner-consultant working with multiple clients sees each client as a separate workspace in Claude Desktop — no data crossover.

---

## Relationship to the existing Stetska Consulting platform

The platform Stetska Consulting has been using in consulting projects continues to run. The MVP in this repo is **additive** — it consumes and extends that data, not replaces it.

Migration strategy is documented in `docs/migration.md` (shared privately with Viktor Kompanets).

---

## Non-goals for MVP

Listed explicitly so scope creep is visible:

- Workflow Engine (deterministic + configurable processes) → Stage 2.1
- External integrations (Jira, Asana, Google Workspace, Slack) → Stage 2.1
- Motivation, cost, budgeting UI (tools exist in MCP, UI later) → Stage 2.2
- Modeling, recommendations, systematic audit → Stage 3
- M&A due-diligence module → Stage 4
- Mobile app → later
- White-label for consulting partners → Stage 2.3

---

## Open questions carried from product spec §7

These remain open from the working session on 2026-03-22 and will be resolved in the Week 1 discovery phase:

1. Personal employee cabinet — full portal or widgets-in-existing-tools?
2. Communication module — built-in or Slack/Teams integration?
3. AI agent — MVP from day 1 (our recommendation) or defer to Phase 3?
4. Pricing model — tier-based (per headcount) vs module-based (per subsystem)?
5. Pilot segment — operational companies or funds/holdings?

---

## Decision records

Architecture decisions will be logged as ADRs in `docs/decisions/`, following the [Michael Nygard format](https://cognitect.com/blog/2011/11/15/documenting-architecture-decisions.html).
