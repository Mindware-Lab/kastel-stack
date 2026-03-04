# InforZone (Toolbox / sync + "email intelligence")

## Snapshot
InforZone appears positioned around **system-to-system sync** (CRM/recruiting/email) plus an "AI email intelligence" layer and audit/dry-run messaging.

## Where it overlaps with Kastel Stack
- Integration glue across business systems.
- "Reliability" language: audit trails, dry-run, trust.
- Automation-oriented value prop: fewer gaps between systems, fewer dropped leads.

## Where InforZone is strong
- Narrow, tangible use-case: sync between specific systems.
- If their "dry-run" is real, that's a useful reliability primitive.
- Likely good at connector engineering (auth, token refresh, mapping, retries).

## What InforZone tends to lack (relative to Kastel Stack)
- **Scope**: looks like sync-first, not a full ops kernel (safety gate, approvals, proof checks as a universal layer).
- **Tiered sovereignty**: no clear "ownable appliance" story as the core product posture.
- **Model untrusted** / policy-gated execution as the centrepiece.
- "Exit-by-design" (customer can stop paying and still run the system) is unlikely in a SaaS sync tool.

## Kastel Stack positioning (short)
Kastel Stack is the **governance + safety + sovereignty kernel**. InforZone is potentially a **connector/sync module** inside that kernel.

## Partner / integration angle
- Use InforZone for certain connectors/sync flows.
- Kastel Stack provides the safety gate, audit ledger, proof checks, and sovereign Tier C/D packaging.

## Questions to verify in a meeting
- Is "dry-run" a genuine simulator with diffs + rollback plan, or a light test mode?
- Self-hosting/private deployment options: do they exist and what is the boundary?
- Do they have an explicit "approval gate" model for outbound comms and money-adjacent actions?