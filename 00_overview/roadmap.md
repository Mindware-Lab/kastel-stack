# Kastel Stack - Roadmap (v0.2)

This roadmap is designed for investor clarity and execution discipline. It assumes:
- Founder-led build (40 hrs/week, Codex-accelerated)
- Freelancer hardening at key points (payments/RLS/security)
- HRPLab Ltd as the proving ground / case study
- Tier progression: **A -> B -> C** (Tier D only if threat model demands)

---

## North Star
**Automate the routine, escalate the salient, log everything.**  
We only "bank" workflows after **proof checks** (dry-run -> rollback test -> delayed re-check), via the **G-Loop Proof Engine** (Execute-Observe-Update with portability checks).

---

## Tier stacks (reference)

### Tier A - Cloud Convenience
- Websites: GitHub Pages
- Backend: Supabase (Auth + Postgres + RLS)
- Payments: Stripe Checkout + webhooks (server-truth entitlements)
- Automation/connectors: Make/Zapier (early)
- LLM: hosted provider behind tool allowlists + schema validation
- Console UI: hosted React/Next.js
- Audit log: Postgres `events` (append-only) + daily export
- Monitoring: Sentry + basic uptime alerts

### Tier B - Hybrid Sovereign
- Tier A control plane (Supabase + hosted console) plus:
- Local context node: local docs + local retrieval (vector) + **redaction proxy**
- Private tunnel: WireGuard/Tailscale (cloud worker <-> local node)
- Hosted LLM sees only redacted/minimal context
- Optional: mirror sensitive audit subset locally

### Tier C - Ownable Appliance
- Customer appliance: mini-PC (single node or gateway + inference split)
- Local LLM: LM Studio or Ollama
- Local DB: Postgres (or SQLite) + local vector store
- Local console UI served from appliance
- Payments: Stripe remains cloud; webhooks verified locally; entitlements become local truth after verification
- Updates: signed release channel + optional maintenance/support

### Tier D - High Assurance (later, only if required)
- Tier C plus:
- Identity: Keycloak + strong MFA, least privilege roles
- Secrets: Vault (or equivalent), rotation discipline
- Docs: Nextcloud + ONLYOFFICE; E2EE folders for Restricted subset (or CryptPad)
- Messaging: Matrix (Synapse + Element) (Signal as pragmatic small-team option)
- Network: allowlisted egress + segmentation
- Observability: Prometheus/Grafana + centralised logs + secure alerts
- Ops: incident runbooks, offline backups, restoration drills, signed updates/SBOM discipline
- Payments resilience: multi-rail design (primary PSP + dormant backup + pay-by-bank where relevant + invoice/bank transfer), without autonomous money movement (Finance Intents only)

---

## Phase 0 - Repo + kernel invariants (Week 0-1)

### Goal
Lock the kernel so tiers become deployment choices, not rewrites.

### Deliverables
- Repo structure complete (overview/business/product/market/ops/build)
- Kernel invariants documented in `02_product/architecture/`:
  - ontology v0.1
  - event schema (`events` append-only)
  - safety gate rules (Auto-run / Draft / Escalate)
  - proof checks lifecycle ("bank only what holds")
  - G-Loop outline (Execute-Observe-Update + swap tests)

### Definition of done
- All invariants exist as versioned artefacts
- A single diagram summarises: **Signals -> Gate -> Actions -> Audit -> Proof checks**
- A "banking checklist" exists for promoting a workflow to Auto-run

---

## Phase 1 - Tier A MVP (Weeks 1-2)

### Goal
Run HRPLab's core operational loop end-to-end with auditability and approvals.

### Deliverables
- Websites (GitHub Pages) with lead capture forms
- Supabase tables for leads/consent/events
- Stripe Checkout live (test + production mode)
- Console v0:
  - queue for drafts and escalations
  - "why" panel (policy reason)
- Basic onboarding sequence (draft-for-approval by default)
- Entitlement gating v0 (UI gates off server-truth fields)

### Definition of done
- A lead can go: **capture -> follow-up draft -> approved send -> checkout -> entitlement -> onboarding**
- Every step emits an audit event with:
  - actor
  - input refs
  - policy rule
  - outcome
- At least one "escalate" path exists (money/reputation action requires approval)

---

## Phase 2 - Tier A hardening (Weeks 3-4)

### Goal
Make the system trustworthy enough for paid pilots (payments correctness and data safety).

### Deliverables (freelancer-led)
- Stripe webhook endpoint:
  - signature verification
  - idempotent processing
  - replay tests
- Entitlements updated by webhook truth only
- Supabase RLS review + fixes + indexes
- Secrets hygiene:
  - no keys in repos
  - environment separation (dev/staging/prod)

### Deliverables (founder-led)
- G-Loop Proof Engine v0:
  - Execute-Observe-Update recording (outcome signals per workflow run)
  - "banking checklist" + workflow promotion rules
- Proof checks v0:
  - dry-run harness for workflows
  - delayed re-check queue ("tomorrow check")
- Basic monitoring + alerting

### Definition of done
- Webhooks are replay-safe; duplicates do not corrupt state
- A user cannot spoof entitlements client-side
- RLS policies prevent cross-user access
- At least one workflow is "banked" only after proof checks and logged as such

---

## Phase 3 - Tier B boundary (Weeks 5-6)

### Goal
Introduce a real sovereignty boundary without appliance complexity: sensitive context stays local.

### Deliverables
- Local context node running inside HRPLab boundary:
  - local doc store + indexing
  - local retrieval (vector store)
  - redaction proxy (hosted LLM sees minimal context)
- Private tunnel (WireGuard/Tailscale) to local node
- Console shows provenance:
  - "local context used"
  - "redaction applied"
- Updated policy rules:
  - any workflow requiring sensitive docs must route through local node
- G-Loop v0 adds portability checks:
  - wrapper swap probe (same workflow across two contexts)
  - stake swap probe (higher volume or higher-risk sample stays draft-for-approval until stable)

### Definition of done
- Sensitive internal documents are never sent raw to hosted inference
- The system still functions end-to-end (no loss of core capability)
- Audit events record whether the local boundary was used
- At least one "swap test" is run and recorded before banking a workflow

---

## Phase 4 - Tier C appliance beta (Weeks 7-10) *(optional / demand-driven)*

### Goal
Productise "sovereign mode": local inference + local audit + ownable operation.

### Deliverables
- Appliance build:
  - local DB (Postgres/SQLite)
  - local LLM (LM Studio or Ollama)
  - local retrieval index
  - local console UI
- "Ownable licence" packaging:
  - installer script
  - config export/import
  - backup/restore steps
  - break-glass admin procedure
- Signed updates channel (basic)
- Optional annual maintenance plan spec
- Payments resilience v0 (Tier C posture):
  - Stripe webhooks verified locally
  - documented "secondary rail" plan (configured but dormant), plus invoice/bank transfer fallback for larger clients
  - Finance Intents enforced (no autonomous money movement)

### Definition of done
- Appliance runs without cloud dependence for inference, retrieval, and audit log
- Customer can stop paying and keep operating (no lock-in)
- A standard install workshop checklist exists
- At least one pilot-ready appliance build is reproducible from the repo
- A "sovereign payments" policy exists (what is allowed, what is never automated)

---

## Phase 5 - Pilot programme + proof assets (Weeks 11-12)

### Goal
Validate willingness to pay and produce proof fragments for distribution.

### Deliverables
- 1-2 paid pilots (Tier A/B; Tier C if ready)
- Case study pack:
  - before/after metrics
  - audit trail screenshots
  - "what we refuse to automate" list
  - at least one G-Loop proof fragment (what changed, what check, what got banked)
- Sales assets:
  - one-pager updated
  - pitch deck updated
  - competitor map summary

### Definition of done
- At least one external team runs Kastel Stack for a real workflow
- Metrics improve in at least 2 of:
  - response latency
  - manual interruptions/day
  - follow-up completion rate
  - error rate / rollback success
- You can tell a credible "trust + sovereignty" story with artefacts

---

## Phase 6 - Distribution scale (Months 4-9)

### Trigger
Stable pilots + repeatable deployment checklist + low support burden.

### Deliverables
- Hire/contract a marketing operator (startup niches e.g., Sydney)
- Launch engine:
  - workshops
  - founder community partnerships
  - referral programme
- Self-serve Tier A onboarding (later; only when ready)
- "Productised pilot" offer sheet (scope, timeline, price, success metrics)

### Definition of done
- Predictable pipeline:
  - qualified calls booked per week
  - pilot-to-paid conversion rate tracked
- Repeatable "install + policy tune" runbook (Tier A/B)
- Clear Tier C entry criteria (when to sell the appliance vs managed)

---

## Phase 7 - Tier D (High Assurance) *(only if required)*

### Trigger
A buyer requires hardened comms/docs/identity/network discipline (or biosensor/wellness lane demands it).

### Deliverables
- Hardened docs and comms stack (self-hosted)
- Key management and strict secrets discipline
- Segmented network posture
- Assurance runbooks + SLA package
- Tier D payments posture:
  - multiple lawful rails configured (primary + backup PSP + pay-by-bank where relevant + invoice/bank transfer)
  - strict Finance Intent approvals (optionally two-person rule)
  - segregation of finance operations environment

### Definition of done
- Tier D is a premium, high-touch deployment with explicit boundaries and procedures
- No Tier D promises without operational capacity to support them

---

## KPIs to track from day 1 (proof posture)
- Lead response latency (median)
- Follow-up completion rate
- Manual interruptions/day (count)
- Automation failure rate + rollback success rate
- Number of "banked workflows"
- Support burden (hours/week)
- G-Loop health: swap-test pass rate, drift events/month, "banked scripts" count

---

## Who does what (execution model)

### Founder
- architecture + build
- core workflow implementation
- pilot delivery and evidence production
- G-Loop proof engine v0/v1

### Freelancer (hardening)
- payments/webhooks correctness
- RLS + security posture review
- secrets hygiene and release discipline
- installer and update packaging (Tier C)

### Marketing operator (later)
- pipeline creation in target niches
- partnerships and workshops
- lead qualification and follow-up process
- case study distribution and social proof gathering