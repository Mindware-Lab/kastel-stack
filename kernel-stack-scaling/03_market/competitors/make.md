# Make (automation builder + emerging AI agent features)

## Snapshot
Make is a visual automation builder (scenarios across many apps) with strong connector coverage and an "agentic" direction layered on top.

## Where it overlaps with Kastel Stack
- Workflow automation across tools.
- Event-driven triggers and multi-step pipelines.
- Can be used as a connector layer in early-stage ops.

## Where Make is strong
- Very fast to ship integrations and prototypes.
- Good at "glue work" across SaaS tools.
- Lower engineering burden than building everything from scratch.

## What Make tends to lack (relative to Kastel Stack)
- **Governed execution** as the primary posture (policy gating, approvals, rollback/replay) rather than "automation building".
- **Sovereign mode**: not designed around local inference + local audit + customer-owned operation.
- A first-class "proof checks" lifecycle (bank only what holds) across automations.
- A coherent "safety gate" narrative that maps onto LLM-era risks (prompt injection, excessive agency).

## Kastel Stack positioning (short)
Kastel Stack is the **trust layer**: safety gate + audit + proof checks + sovereignty tiers.
Make can be an implementation tool inside Tier A to move quickly, but it's not the product.

## Partner / integration angle
- Early: use Make as the connector fabric while building the kernel.
- Later: replace hot paths with a hardened worker and keep Make only where safe.

## Questions to verify
- Can we enforce approval gates and immutable auditing consistently across scenarios?
- Can we export and run without Make (exit-by-design)?