# Kastel Stack — Sovereign Ops Kernel

**Automate the routine, escalate the salient, log everything.**  
Kastel Stack is a **governed execution layer** for small teams: agentic automation that stays **policy-gated**, **auditable**, and (at higher tiers) **sovereign/ownable**.

## The promise
- **Delegatable automation:** the AI can propose; **policies decide**.
- **Proof checks:** we only “bank” workflows after **dry-run → rollback test → delayed re-check**.
- **Digital sovereignty (optional):** run locally as an **ownable appliance** with a clean exit path.

---

## Kernel model (same in all tiers)

**Signals in → Safety gate → Actions out → Audit trail → Proof checks**

- **Signals in:** websites/forms, inbox, calendar, payment events, docs/repo updates
- **Safety gate:** Auto-run (safe) / Draft-for-approval / Escalate (high-risk)
- **Actions out:** follow-ups, CRM updates, publishing/SEO hygiene, entitlements, research queue
- **Audit trail:** append-only log with “what / why / rollback”
- **Proof checks:** dry-run harness + rollback tests + delayed re-check queue

---

## Security tiers (stacks)

> The tier chooses the **trust boundary**. The kernel stays the same.

### Tier A — Cloud Convenience (fastest to ship)
**Use when:** speed and simplicity matter most.

**Stack**
- Websites: GitHub Pages (static)
- Backend: Supabase (Auth + Postgres + RLS)
- Payments: Stripe (Checkout + webhooks)
- Automation/connectors: Make/Zapier (early)
- LLM: hosted provider behind strict tool allowlists + schema validation
- Console: hosted React/Next.js
- Audit log: Postgres `events` (append-only) + daily export

### Tier B — Hybrid Sovereign (privacy boundary)
**Use when:** sensitive context must stay inside your boundary, but you still want a cloud control plane.

**Stack**
- Cloud control plane: Supabase + hosted console
- Local “context node”: local doc store + retrieval index + **redaction proxy**
- Hosted LLM sees only minimal redacted context
- Private tunnel: WireGuard/Tailscale between cloud worker and local node

### Tier C — Intra-system Appliance (ownable sovereign mode)
**Use when:** sovereignty is the product. Customer can keep running if they stop paying.

**Stack**
- Appliance: customer-run mini-PC (single node or gateway+inference split)
- Local inference: LM Studio or Ollama
- Local data: Postgres (or SQLite) + local vector store
- Local console UI served from appliance
- Updates: signed release channel + optional maintenance plan

**Commercial model**
- **One-off licence + installation/workshops**
- **Optional annual maintenance/support**
- Promise: **stop paying and it still runs** (you lose updates/support, not functionality)

### Tier D — High Assurance (maximum control, minimum blast radius)
**Use when:** high-threat / high sensitivity. Assume hostile inputs and harden comms/docs/identity.

**Stack (adds to Tier C)**
- Identity: self-hosted IdP (e.g., Keycloak) + strong MFA
- Secrets: Vault (or equivalent), aggressive rotation, no secrets in model context
- Docs: Nextcloud + ONLYOFFICE; E2EE folders for “Restricted” subset; CryptPad as E2EE-first alternative
- Messaging: Matrix (Synapse + Element) for governable internal comms; Signal as pragmatic small-team option
- Network: allowlisted egress + segmented nodes
- Observability: Prometheus/Grafana + centralised logs + secure alerts
- Supply chain: pinned deps, signed releases, minimal plugin surface, SBOM discipline
- Incident readiness: runbooks, offline backups, restoration drills, “break glass” procedure

---

## Payments & banking (NAPA-aligned rails)

**Design goal:** *anti-chokepoint architecture*, not “magic processors outside the system”.

**Principles**
- **Diverse settlement rails** (reduce de-platforming/freeze risk through redundancy).
- Treat mainstream platforms as **Blue Rail** tools only if they pass a **power test**: net gain, modular exit, no governance/IP capture, preserve rail separation, avoid prohibited counterparties.
- Keep **Red Rail** capital flows and sensitive ops away from profiling/score systems via **data minimisation + segregation**.

### Reality check
Anything touching **fiat** at scale is regulated (KYC/AML). “Outside the system” is mostly a myth.  
Western Union/MoneyGram can be a manual fallback in niche cases but is typically high-friction for online merchant checkout:
- https://www.westernunion.com/gb/en/money-transfer-fees.html

### Recommended “rails” by tier

#### Tier A/B (conversion + convenience)
- **Cards (primary):** Stripe (inevitable for most international buyers)
- **Second card rail (dormant):** keep a backup processor configured for lawful continuity (route *new* checkouts if PSP1 is disrupted)
- **Pay-by-bank (UK/EU):** add Open Banking pay-by-bank to reduce card-network dependence  
  - TrueLayer Pay by Bank: https://truelayer.com/payments/
  - GoCardless Instant Bank Pay: https://gocardless.com/solutions/instant-bank-pay/
- **Invoice + bank transfer:** for institutional/higher-trust clients and resilience

#### Tier C/D (sovereign + resilient)
Everything above, plus:
- **Red Rail option (aligned customers):** crypto **direct-to-wallet** via self-hosted BTCPay Server (optional Lightning)  
  - BTCPay Server: https://btcpayserver.org/
  - Lightning in BTCPay: https://docs.btcpayserver.org/LightningNetwork/

**Important constraint:** Red Rail exists for resilience and optionality, **not** to evade lawful restrictions. If sanctions/AML prohibitions apply, assume you must comply.

### Finance automation rule (all tiers, especially C/D)
The kernel should not “move money” autonomously.
- Use **Finance Intents**: draft invoice/refund request/reconciliation → **approval** → execute in PSP/bank UI → audit log.
- High-impact actions (refunds over threshold, payout changes) are always **Escalate**.

---

## Obsidian “anywhere” (knowledge OS)

Obsidian can be used in all tiers **if you treat the vault as plain Markdown** and choose the sync model based on tier.

**Recommended**
- Tier A/B: Obsidian + optional Obsidian Sync (convenience)
- Tier C: Obsidian local vault + customer-owned sync (Git/Nextcloud); keep “Restricted” vault local-only
- Tier D: split vaults by classification:
  - **Internal** (sync via self-hosted Git/Nextcloud)
  - **Restricted** (no cloud sync; local-only; encrypted backups)

Rule: don’t let your “sovereign tier” depend on a vendor service for core knowledge continuity.

---

## Repo map
- `00_overview/` — one-pager, pitch deck, diagrams, roadmap
- `01_business/` — financial model, pricing/packaging, legal outlines
- `02_product/` — architecture, tier stacks, workflows, threat model, controls
- `03_market/` — competitors, partners, research notes
- `04_ops/` — meetings, decision log, risk register, runbooks
- `05_build/` — prototypes, scripts, infra

Key docs:
- `02_product/architecture/tech-stack-by-tier.md`
- `02_product/architecture/decision-tree.md`
- `02_product/architecture/control-matrix.md`
- `02_product/architecture/payments-by-tier.md` *(add/maintain here)*
- `02_product/architecture/psp-redundancy.md` *(backup rail plan)*
- `02_product/workflows/stripe-entitlements.md`
- `02_product/workflows/payments-intents.md`

---

## Current focus (IQ Mindware as the proving ground)
1) Tier A MVP (ship + measure)
2) Tier A hardening (payments/RLS/security)
3) Tier B boundary (local context + redaction)
4) Tier C appliance beta (ownable sovereign mode)
5) Tier D (only where the threat model demands)

Keep decisions and evidence in:
- `04_ops/decisions/decision-log.md`
- `04_ops/risks/risk-register.md`
