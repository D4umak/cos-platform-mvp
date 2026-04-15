# Data model

## Overview

The COS data model follows the book's structure: **company → departments → 11 elements per department → roles → KPIs**.

Everything is scoped to a `company`, and multi-tenancy is enforced at the database level via Supabase Row-Level Security (RLS).

---

## Core entities

### `company`

The tenant root. Every other entity joins back here.

| Column | Type | Notes |
|--------|------|-------|
| `id` | uuid | PK |
| `org_id` | uuid | FK → `org` (the consulting partner's workspace) |
| `name` | text | — |
| `industry` | text | Enum-like |
| `size_headcount` | int | — |
| `revenue_band` | text | `$0-2M`, `$2-10M`, `$10-50M`, `$50M+` |
| `created_at` | timestamptz | — |

### `department`

A standardizable unit. Goal: 11/11 elements completed.

| Column | Type | Notes |
|--------|------|-------|
| `id` | uuid | PK |
| `company_id` | uuid | FK → `company` |
| `name` | text | — |
| `parent_id` | uuid | FK → `department` (self, for tree) |
| `head_role_id` | uuid | FK → `role` (the department head) |
| `scalability_index` | numeric | Cached; recomputed on element change |
| `created_at` | timestamptz | — |

### `element` (the 11)

Each row is one of the 11 standardization elements for a given department.

| Column | Type | Notes |
|--------|------|-------|
| `id` | uuid | PK |
| `department_id` | uuid | FK → `department` |
| `kind` | text | Enum: `ckp`, `kpi_dept`, `roles`, `kpi_roles`, `processes`, `sops`, `cost`, `motivation`, `reporting`, `budget`, `risk_map` |
| `status` | text | `empty`, `draft`, `in_review`, `approved` |
| `content` | jsonb | Shape varies per `kind` — see below |
| `updated_by` | uuid | FK → `user` |
| `updated_at` | timestamptz | — |

Unique index: `(department_id, kind)` — one row per (department, element kind).

### `role`

A position within a department.

| Column | Type | Notes |
|--------|------|-------|
| `id` | uuid | PK |
| `department_id` | uuid | FK → `department` |
| `title` | text | — |
| `grade` | text | Seniority level |
| `responsibility_zones` | jsonb | Array of zones |
| `reports_to_role_id` | uuid | FK → `role` (self) |
| `cost_per_hour` | numeric | For ABC calculations |

### `kpi`

A metric attached to either a department or a role.

| Column | Type | Notes |
|--------|------|-------|
| `id` | uuid | PK |
| `owner_type` | text | `department` \| `role` |
| `owner_id` | uuid | polymorphic FK |
| `name` | text | — |
| `target` | numeric | — |
| `unit` | text | — |
| `cadence` | text | `daily`, `weekly`, `monthly`, `quarterly` |
| `formula` | text | How it's computed |

### `kpi_measurement`

Time series of actual values.

| Column | Type | Notes |
|--------|------|-------|
| `id` | uuid | PK |
| `kpi_id` | uuid | FK → `kpi` |
| `period_start` | date | — |
| `value` | numeric | — |
| `source` | text | `manual`, `integration:jira`, `integration:google`, etc. |

### `audit_finding`

Output of planned or on-demand audits.

| Column | Type | Notes |
|--------|------|-------|
| `id` | uuid | PK |
| `department_id` | uuid | FK → `department` |
| `severity` | text | `low`, `medium`, `high`, `critical` |
| `kind` | text | `systematic`, `reactive`, `self_audit` |
| `finding` | text | — |
| `remediation` | text | — |
| `status` | text | `open`, `in_progress`, `resolved` |

---

## The Scalability Index™

Computed per department as a single 0–100 score that answers *"is this department ready for 10× growth?"*.

High-level formula (exact weights remain proprietary to Stetska Consulting):

```
SI = f(
  completeness_11_elements,      # how many of 11 are approved
  bus_factor,                    # key-person risk
  kpi_coverage,                  # % of roles with defined KPIs
  process_determinism,           # ratio of documented processes
  sop_freshness,                 # avg age of SOPs
  cost_transparency              # are ABC costs filled in?
)
```

Cached on `department.scalability_index`, invalidated on any element update, recomputed by a background job.

---

## Row-level security

Every query is scoped to the authenticated user's `org_id`:

```sql
create policy "users see only their org's companies"
on company for select
using (org_id = auth.jwt() ->> 'org_id');
```

Cascading policies on `department`, `element`, `role`, `kpi`, etc. all join back to `company.org_id`.

---

## MCP tool → table mapping

| MCP tool | Primary table | Operation |
|----------|---------------|-----------|
| `create_department_card` | `department` | INSERT |
| `set_element` | `element` | UPSERT on `(department_id, kind)` |
| `get_department_card` | `department` + `element` | SELECT with join |
| `compute_scalability_index` | `department` | SELECT + cache read |
| `list_roles` | `role` | SELECT by department |
| `set_kpi_measurement` | `kpi_measurement` | INSERT |
| `record_audit_finding` | `audit_finding` | INSERT |

---

## Migrations

Managed via Supabase CLI. Migration files live in `supabase/migrations/` and are applied via `supabase db push`.

Seed data (a sample company with one partially-standardized department) lives in `supabase/seed.sql` — used in CI and local dev.
