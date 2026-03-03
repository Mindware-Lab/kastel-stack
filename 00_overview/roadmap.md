# Kastel Stack - Roadmap (v0.1)

This roadmap is designed for investor clarity and execution discipline. It assumes:
- Founder-led build (40 hrs/week)
- Freelancer hardening at key points
- HRPLab Ltd as the proving ground / case study
- Tier progression: **A -> B -> C** (Tier D only if threat model demands)

---

## North Star
**Automate the routine, escalate the salient, log everything.**
We only "bank" workflows after **proof checks** (dry-run -> rollback test -> delayed re-check).

---

## Phase 0 - Repo + kernel invariants (Week 0-1)

### Deliverables
- Repo structure complete (overview/business/product/market/ops/build)
- Kernel invariants documented:
  - ontology v0.1
  - event schema (`events` append-only)
  - safety gate rules (Auto-run / Draft / Escalate)
  - proof checks lifecycle ("bank only what holds")

### Definition of done
- All invariants exist as versioned artefacts in `02_product/architecture/`
- A single diagram summarises: **Signals -> Gate -> Actions -> Audit -> Proof checks**

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
- Entitlement gating v0 (client UI gates off server-truth fields)

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
- Proof checks v0:
  - dry-run harness for workflows
  - delayed re-check queue (tomorrow check)
- Basic monitoring + alerting

### Definition of done
- Webhooks are replay-safe; duplicates do not corrupt state
- A user cannot spoof entitlements client-side
- RLS policies prevent cross-user access
- A "banked workflow" checklist exists and is applied to at least one workflow

---

## Phase 3 - Tier B boundary (Weeks 5-6)

### Goal
Introduce a real sovereignty boundary without appliance complexity: sensitive context stays local.

### Deliverables
- Local "context node" running inside HRPLab boundary:
  - local doc store + indexing
  - local retrieval (vector store)
  - redaction proxy (cloud LLM sees minimal context)
- Private tunnel (WireGuard/Tailscale) to local node
- Console shows provenance:
  - "local context used"
  - "redaction applied"
- Updated policy rules:
  - any workflow requiring sensitive docs must route through local node

### Definition of done
- Sensitive internal documents are never sent raw to hosted inference
- The system still functions end-to-end (no loss of core capability)
- Audit events record whether local boundary was used

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

### Definition of done
- Appliance runs without cloud dependence for inference, retrieval, and audit log
- Customer can stop paying and keep operating (no lock-in)
- A standard install workshop checklist exists
- At least one pilot-ready appliance build is reproducible from the repo

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
- Sales assets:
  - one-pager updated
  - pitch deck updated
  - competitor map summary

### Definition of done
- At least one external team runs Kastel Stack for a real workflow
- Metrics show improvement in at least 2 of:
  - response latency
  - manual interruptions/day
  - follow-up completion rate
  - error rate / rollback success
- You can tell a credible "trust" story with artefacts

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

### Definition of done
- Predictable pipeline:
  - qualified calls booked per week
  - pilot-to-paid conversion rate tracked
- Repeatable "install + policy tune" runbook

---

## Phase 7 - Tier D (High Assurance) *(only if required)*

### Trigger
A buyer requires hardened comms/docs/identity/network discipline (or biosensor/wellness lane demands it).

### Deliverables
- Hardened docs and comms stack (self-hosted)
- Key management and strict secrets discipline
- Segmented network posture
- Assurance runbooks + SLA package

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

---

## Who does what (execution model)

### Founder
- architecture + build
- core workflow implementation
- pilot delivery and evidence production

### Freelancer (hardening)
- payments/webhooks correctness
- RLS + security posture review
- installer and update packaging (Tier C)

### Marketing operator (later)
- pipeline creation in target niches
- partnerships and workshops
- lead qualification and follow-up process