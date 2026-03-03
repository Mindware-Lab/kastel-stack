# Kastel Stack - Sketch Business Plan (v0.1)
**2-3 page outline for an angel conversation (spin-off entity)**
**Founder:** Mark Ashton Smith
**Parent context / proving ground:** HRPLab Ltd (internal case study + validation)
**Funding target:** GBP 15,000 angel seed (plus founder time)

---

## 1) Executive summary

**Kastel Stack builds "delegatable automation" for small teams.**
Not "AI runs the company". Kastel Stack is a **governed execution layer** that automates routine work, routes high-risk decisions to a human, and logs everything with proof checks.

**Core claim:** **Automate the routine, escalate the salient, log everything.**
Kastel Stack is designed to be trustworthy in the LLM era: the model can propose; **policies decide**. Workflows are only "banked" after **dry-run -> rollback test -> delayed re-check**.

**Differentiator:** Tiered sovereignty, including an **ownable appliance tier** where customers can stop paying and still keep operating (optional maintenance/support). This supports digital sovereignty via **anti-chokepoint architecture** (no single platform dependency for core ops).

**Go-to-market:** start by implementing Kastel Stack inside HRPLab (proof-producing case study), then sell to a narrow initial niche (small teams/startups) via founder-led pilots. Once stable, add a marketing operator (e.g., Sydney startup circles) to scale distribution.

**Use of funds:** accelerate reliability and trust via freelancer hardening (payments/webhooks, RLS, security hygiene), plus minimum runway/tooling.

---

## 2) The problem (why now)

Small teams are drowning in operational fragmentation:
- leads captured in one place, follow-ups missed
- payments succeed but entitlements/support/onboarding drift
- "automation" breaks trust because it's opaque, brittle, and hard to undo
- LLM "agents" increase risk: prompt injection, excessive agency, and "black box actions" touching reputation and money

**The gap isn't capability. It's trustable execution.**
Founders need automation that is **governed, auditable, reversible**, and (for some buyers) **sovereign/ownable**.

---

## 3) The solution (what Kastel Stack is)

### 3.1 Kernel model (in every tier)
**Signals in -> Safety gate -> Actions out -> Audit trail -> Proof checks**

- **Signals in:** websites/forms, inbox, calendar, payment events, docs/repo changes
- **Safety gate:**
  1) Auto-run (safe, reversible)
  2) Draft-for-approval (human-in-the-loop)
  3) Escalate (money, legal, sensitive data, uncertainty)
- **Actions out:** follow-ups, CRM-lite updates, publishing/SEO hygiene, entitlements, partner research queue
- **Audit trail:** append-only event log ("what/why/inputs/rollback")
- **Proof checks:** dry-run harness + rollback tests + delayed re-check queue ("tomorrow check")

### 3.2 Tiered sovereignty (what customers buy)
- **Tier A - Cloud convenience:** fastest setup; hosted inference; governance still applies
- **Tier B - Hybrid sovereign:** sensitive context stays local; cloud sees minimal redacted context
- **Tier C - Ownable appliance:** local inference + local retrieval + local audit; licence + installation; optional maintenance; **exit-by-design**
- **Tier D - High assurance:** hardened identity/secrets/comms/docs/network discipline for higher threat models

### 3.3 Finance posture (trust-critical)
Kastel Stack does **not** autonomously move money.
It uses **Finance Intents**: draft invoice/refund/reconciliation -> approval -> execute in PSP/bank UI -> audit log.

---

## 4) Initial wedge (ICP + first offer)

### 4.1 Initial target customers (3-6 month focus)
Start with small teams where "trust + ops reliability" matters and the stack is common:
- micro-SaaS founders selling digital products (Stripe + email + onboarding)
- boutique consultancies/agencies (lead follow-up + reputational risk)
- science/health adjacent small teams (privacy posture valued)

### 4.2 The first repeatable productised workflow
**Lead -> follow-up -> checkout -> entitlement -> onboarding**
- stop dropped leads
- reduce manual interruptions/day
- make payment state and access state match (server-truth entitlements)

Secondary workflow (optional, later):
**Partner research queue** via safety gate + audit.

### 4.3 The initial offer (pilot)
- **Paid pilot install** (done-with-you): configure kernel for the client's stack
- Deliverables: safe automation, approval console, audit trail, a short "before/after" report
- Outcome promise (soft): fewer dropped leads, faster response, fewer operational mistakes

---

## 5) Go-to-market plan

### Phase 1 (0-3 months): founder-led, proof-first
**Goal:** 2-5 paying pilots and 1-2 publishable case studies.
- Implement internally in **HRPLab** first (real signals, real events, real constraints)
- Outreach via direct network + founder channels (introductions, niche communities)
- Publish 2-3 proof assets:
  - diagram (kernel + tiers)
  - case study vignette (metrics + audit screenshots)
  - "trust spec" (what we refuse to automate)

**Primary KPI:** qualified pilot calls/week and pilot conversion rate.

### Phase 2 (3-9 months): add a marketing operator to scale distribution
**Trigger:** stable pilot delivery + clear packaging + low support burden.
- Hire/contract a marketing operator (e.g., in Sydney startup circles)
- Distribution channels:
  - founder workshops ("audit-first automation")
  - accelerator/coworking partnerships
  - referral programme from early pilots
- KPI for marketing: qualified meetings booked per week

### Phase 3 (9-18 months): widen the wedge
- Tier C "sovereign mode" packaged as a premium line (licence + install + optional maintenance)
- Tier A as volume engine later (self-serve onboarding, templated flows)

---

## 6) Delivery and operations model

### 6.1 Implementation approach (keep support sane)
- Standardise the kernel and templates; only customise policy thresholds and connectors
- Default to "draft-for-approval" for reputational/money-adjacent actions
- Bank workflows only after proof checks

### 6.2 Support/maintenance posture by tier
- **Tier A/B:** managed subscription includes updates + basic support
- **Tier C:** ownable licence + install; optional annual maintenance/support; system runs without ongoing payment
- **Tier D:** assurance services + annual SLA; stricter runbooks and patch discipline

### 6.3 Security baseline (non-negotiables)
- least privilege connectors
- secrets hygiene (no keys in repos)
- webhook verification + idempotency
- append-only audit log
- "break glass" procedure and backups

---

## 7) Competition and positioning (brief)

Kastel Stack is not trying to out-feature automation builders or CRMs.
It differentiates on:
- **governed execution** as the product (safety gate + approval)
- **audit + rollback** as a first-class primitive
- **proof checks** ("bank only what holds")
- **tiered sovereignty**, including an **ownable appliance** tier (exit-by-design)

Automation builders (Zapier/Make) and CRMs (HubSpot) optimise for breadth and convenience.
Self-host tools (e.g., n8n) can be powerful but shift security burden to the user.
Kastel Stack packages the "safe way to automate" and the "safe way to self-host" into a coherent product.

---

## 8) Financial plan (summary view)

A GBP 15k seed is used to:
- fund the reliability hardening needed for trust (payments/webhooks, RLS, security hygiene)
- provide limited runway/tooling for founder execution
- produce one internal case study + 2-5 pilots to validate pricing/packaging

**Revenue model by tier:**
- Tier A/B: subscription (managed service) + small setup fee
- Tier C: licence + installation/workshops + optional annual maintenance
- Tier D: high assurance deployment + annual support/SLA

(See `01_business/model/sovereign_ops_kernel_speculative_financials_updated.xlsx` for the speculative monthly model.)

---

## 9) Team plan

### Now (0-3 months)
- Founder (Mark): architecture, build, pilot delivery
- Freelancer (hardening): payments/webhooks correctness, RLS/security review, release hygiene

### Later (3-9 months, once pilots validate)
- Marketing operator (contract or hire): startup niche distribution, partner channels, workshops, pipeline

### Later expansion (9-18 months)
- Part-time support/ops (if Tier C/D grows)
- Additional engineering (connector library + reliability tooling)

---

## 10) Risks and mitigations (sketch)

1) **Scope creep** (trying to be "ops for everyone")
   -> Mitigate via a narrow wedge + repeatable workflows + templates.

2) **Security/reliability failure** (payments/entitlements/data leaks)
   -> Mitigate via freelancer hardening + strict gates + audit + proof checks.

3) **Distribution risk** (no predictable pipeline)
   -> Mitigate via founder-led pilots then hire a marketer with one KPI (qualified calls/week).

4) **Support burden in sovereign tiers**
   -> Mitigate with standard appliance spec + signed updates + explicit support boundaries + optional maintenance.

5) **Competition from incumbents**
   -> Mitigate by owning the trust + sovereignty niche (exit-by-design, local-first tiers, audit-first posture).

---

## 11) Milestones (first 12 weeks)

- **Weeks 1-2:** Tier A MVP running inside HRPLab (end-to-end: lead -> checkout -> entitlement -> onboarding)
- **Weeks 3-4:** Hardening pass (webhooks/idempotency, RLS, secrets hygiene, monitoring)
- **Weeks 5-6:** Tier B boundary (local context + redaction)
- **Weeks 7-10:** Tier C prototype (if pilots demand; otherwise defer)
- **Weeks 11-12:** 1-2 paid pilots + publishable case study (metrics + proof fragment)

---

## 12) What the investor is funding (the ask)

**GBP 15,000** to reach a decision point:
- a stable, hardened kernel proven in HRPLab
- 2-5 pilots to validate pricing and demand
- a publishable proof story (trust posture + audit trail + outcomes)

**Next decision point (post-pilots):**
- scale via marketing operator + templated deployments (Tier A/B)
- productise "sovereign mode" (Tier C) as a premium line