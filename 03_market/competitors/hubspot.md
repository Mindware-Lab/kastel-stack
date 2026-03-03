# HubSpot (Breeze / CRM suite)

## Snapshot
HubSpot is a CRM-centric platform (marketing, sales, service) with embedded AI (e.g., "Breeze" agents) and deep automation inside the HubSpot ecosystem.

## Where it overlaps with Kastel Stack
- Automates business workflows (lead routing, follow-ups, lifecycle stages).
- Strong "ops visibility" inside CRM (activity timelines, attribution, etc.).
- Can act as a central "truth layer" for contacts/deals and outbound sequences.

## Where HubSpot is strong
- CRM-native: excellent for pipeline and lifecycle management.
- Mature UX and reporting; reduces internal operational friction.
- "All-in-one" platform reduces integration work early.

## What HubSpot tends to lack (relative to Kastel Stack)
- **Sovereignty stance** is not first-class. It's primarily a cloud suite.
- **Kernel-level governance**: HubSpot is optimised for "do more inside HubSpot", not "governed execution across arbitrary tools".
- **Ownable appliance mode**: no credible "stop paying and it keeps running" equivalent.
- The "AI safety gate" model (auto-run / draft / escalate) is not the primary product posture across all actions.
- Proof checks ("bank only what holds" via dry-run -> rollback test -> delayed re-check) are not a standard product primitive.

## Kastel Stack positioning (short)
**Not a CRM replacement.** A **governed execution layer** above tools (including HubSpot), with tiered sovereignty and explicit approval gates for money/reputation.

## Partner / integration angle
Kastel Stack can treat HubSpot as:
- A source of signals (new lead, stage change).
- A target for safe actions (update deal stage) through the safety gate.

## Questions to verify in a meeting
- Can customers run any meaningful part "local-only"?
- Are there enforceable approval gates for sensitive actions beyond standard permissions?
- Can you export and continue operating without the platform (exit-by-design)?