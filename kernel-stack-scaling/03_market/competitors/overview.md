# Competitor landscape - Kastel Stack

This folder contains short competitor notes to support positioning, partner screening, and investor diligence.

## The category
Kastel Stack sits in the overlap of:
- workflow automation (connectors + triggers)
- "agentic" layers (LLM-assisted planning/drafting)
- governance/security (approval gates, audit trails, rollback)
- sovereignty (local-first / ownable deployments)

**Our wedge is not "more AI".**
It is **delegatable execution**: **Safety gate + Audit trail + Proof checks + Sovereignty tiers**.

---

## The Kastel Stack differentiators (repeat these)
1) **Safety gate (salience routing):**
   - Auto-run (safe, reversible)
   - Draft-for-approval (human-in-the-loop)
   - Escalate (money, legal, sensitive data, uncertainty)

2) **Audit + rollback:**
   - append-only event log
   - "what happened / why allowed / inputs / rollback link"
   - replay capability for workflows

3) **Proof checks ("bank only what holds"):**
   - dry-run harness
   - rollback tests
   - delayed re-check queue ("tomorrow check")
   - drift detection -> route back to draft/escalate

4) **Tiered sovereignty (trust boundary choices):**
   - Tier A: Cloud convenience
   - Tier B: Hybrid (local context + redaction boundary)
   - Tier C: Ownable appliance (local inference + local audit) - *exit-by-design*
   - Tier D: High assurance (hardened identity/secrets/docs/comms/network)

---

## Competitor map (what they are, where we differ)

### Zapier (`zapier.md`)
- **What it is:** mainstream automation with huge connector coverage and "agentic" direction.
- **Overlap:** automation across tools.
- **Where we differ:** sovereignty/ownable mode + proof checks + safety gate as core product posture.

### Make (`make.md`)
- **What it is:** visual automation builder with growing AI/agent features.
- **Overlap:** connector-driven multi-step workflows.
- **Where we differ:** productised governance (approval/rollback/replay) and sovereignty tiers.

### HubSpot (`hubspot.md`)
- **What it is:** CRM-native suite with AI embedded across marketing/sales/service.
- **Overlap:** lifecycle automation, pipeline management.
- **Where we differ:** stack-agnostic kernel + ownable appliance + "exit-by-design". HubSpot is a platform; we are a governed layer above platforms.

### n8n (`n8n.md`)
- **What it is:** self-hostable automation engine.
- **Overlap:** workflows + self-hosting potential.
- **Where we differ:** we provide the governance shell and sovereignty packaging (signed updates, approval gates, proof checks). Self-hosting is not automatically secure; our product is the "safe way to self-host".

### InforZone (`inforzone.md`)
- **What it is:** appears sync-first (CRM/recruiting/email) with "audit/dry-run" messaging.
- **Overlap:** integration glue + trust language.
- **Where we differ:** broader kernel (safety gate + proof checks + sovereignty tiers). Potentially a partner as a connector module.

---

## How to use these notes in a meeting
For any competitor/partner candidate, answer:

1) **Scope:** Are they a workflow builder, a CRM platform, a sync tool, or a governed execution layer?
2) **Governance primitives:** Do they have enforceable approval gates, immutable auditing, rollback/replay?
3) **Sovereignty:** Can customers run local inference + local audit + keep operating if they stop paying?
4) **Threat posture:** How do they bound prompt injection and excessive agency (beyond prompt "rules")?
5) **Exit-by-design:** Can customers export workflows + logs and continue operating without the vendor?

If the answer is "no" for (2) and (3), Kastel Stack remains meaningfully differentiated.
If the answer is "yes" for (2) but "no" for (3), we can still compete with Tier C/D.
If the answer is "yes" for (2) and (3), treat them as a serious competitor or a high-value partner candidate.

---

## Recommended next additions
- Add 2-3 more competitor notes as needed:
  - payment orchestration layer (for multi-PSP routing)
  - "sovereign cloud" providers (if Tier D is a target market)
  - security posture of common self-host tools (patching/update channels)