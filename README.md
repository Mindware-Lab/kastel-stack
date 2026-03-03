# Kastel Stack - Sovereign Ops Kernel

Kastel Stack is a policy-gated, auditable automation kernel that helps small teams automate operations safely, with optional sovereign local deployment.

**Automate the routine, escalate the salient, log everything.**
Not "AI runs the company". **Kastel Stack** is a governed execution layer for small teams: agentic automation that stays **policy-gated**, **auditable**, and (at higher tiers) **sovereign/ownable**.

## The promise
- **Delegatable automation:** AI can propose; **policies decide**.
- **Transfer checked, not assumed:** we only "bank" workflows after **proof checks** (dry-run, rollback test, delayed re-check).
- **Digital sovereignty (optional):** run locally as an **ownable appliance** (no lock-in).

## Kernel model (in all tiers)
**Signals in -> Safety gate -> Actions out -> Audit trail -> Proof checks**

- **Signals in:** websites/forms, inbox, calendar, Stripe events, docs/repo updates
- **Safety gate:** Auto-run (safe) / Draft-for-approval / Escalate (high-risk)
- **Actions out:** follow-ups, CRM updates, publishing/SEO hygiene, entitlements, research queue
- **Audit trail:** append-only log with "what / why / rollback"
- **Proof checks:** dry-run harness + rollback tests + delayed re-check queue

## Security tiers (same kernel, different trust boundary)
- **Tier A - Cloud Convenience:** fastest setup, hosted inference; governance still applies
- **Tier B - Hybrid Sovereign:** sensitive context stays local; cloud sees minimal redacted inputs
- **Tier C - Intra-system Appliance:** local inference + local retrieval + local audit log (ownable licence + install + optional maintenance)
- **Tier D - High Assurance:** Tier C plus hardened identity/secrets/comms/docs/network discipline

## Repo map
- `00_overview/` - one-pager, pitch deck, diagrams, roadmap
- `01_business/` - financial model, pricing/packaging, legal outlines
- `02_product/` - architecture, control matrix, workflows, UX
- `03_market/` - competitors, partners, market notes
- `04_ops/` - meetings, decision log, risk register, runbooks
- `05_build/` - prototypes, scripts, infra

## Current focus
Use **IQ Mindware** as the proving ground:
1) Tier A MVP (ship + measure)
2) Tier A hardening (payments/RLS/security)
3) Tier B boundary (local context + redaction)
4) Tier C appliance beta (ownable sovereign mode)
5) Tier D (only where the threat model demands)

## Contact / notes
Keep decisions and evidence in `04_ops/decisions/decision-log.md` and `04_ops/risks/risk-register.md`.