# n8n (self-hostable automation)

## Snapshot
n8n is an automation platform with strong flexibility and a self-hosting story, popular for teams who want more control than pure SaaS automation tools.

## Where it overlaps with Kastel Stack
- Workflow automation across tools.
- Self-hosting option aligns with sovereignty goals.
- Could serve as an execution engine beneath the kernel.

## Where n8n is strong
- Self-hosting: you can run it inside your infrastructure boundary.
- Flexible workflows; good for connector-heavy pipelines.
- Community ecosystem and extensibility.

## What n8n tends to lack (relative to Kastel Stack)
- Self-hosting is **not automatically secure**. The operational/security burden shifts to the user.
- Not inherently designed as an LLM-era **governed execution layer** (model untrusted, approval gates, proof checks).
- Does not ship a tiered "ownable appliance" product stance with signed updates, separation of duties, and audit-first posture.
- "Exit-by-design" is possible via self-hosting, but the kernel-level governance and proof lifecycle is still something Kastel Stack adds.

## Kastel Stack positioning (short)
Kastel Stack can be the **governance shell** and treat n8n as an internal execution tool where appropriate.
The product is **trust + sovereignty packaging**, not "a workflow builder".

## Partner / integration angle
- Tier C/D: optionally bundle a hardened, locked-down n8n instance as an internal worker (only if the security story is strong).
- Keep the kernel's safety gate, audit, and approvals above it.

## Questions to verify
- How will we patch and monitor n8n securely in appliance mode?
- Can we restrict n8n's privileges so "write actions" always require approval via the kernel?