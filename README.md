# kastel-stack (implementation monorepo)

This repository is now organised for implementing and operating the Kastel Stack kernel in a single monorepo.

## Active structure
- `domains/` - domain services and workflows
- `contracts/` - versioned cross-domain schemas/contracts
- `kernel/` - G-Loop, Psi Governor, Safety Gate, orchestration logic
- `shared/` - shared libraries (event envelope, policy helpers, SDK)

## Domain folders
- `domains/d1-audience-demand/`
- `domains/d2-commerce-entitlements/`
- `domains/d3-delivery-coaching/`
- `domains/d4-outcomes-evidence/`
- `domains/d5-partnerships-talent/` (later)
- `domains/d6-platform-governance/` (later)

## Archive
Previous business-plan-first structure is preserved in:
- `kernel-stack-scaling/`

## Monorepo rule
Keep this as one repo through Tier A and early Tier B. Consider splitting `d2` and `d6` later only when release cadence/security/ownership requires it.