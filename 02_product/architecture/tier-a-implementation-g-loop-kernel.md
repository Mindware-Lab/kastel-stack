# Kastel Stack Tier-A Implementation Spec (G-Loop Kernel)

## 1) Tier-A reference architecture (where the "brains" live)

### A. Signals in (ingest layer)

Sources you listed (examples):

- Website forms / landing pages
- Inbox (Gmail)
- Calendar
- Stripe webhooks (purchase, subscription events)
- Docs/repo updates (Drive/GitHub)

Implementation (Tier A, MVP):

- Use Make/Zapier to normalise events and post them to one HTTPS endpoint:
  - `POST /api/signals/ingest`
- Stripe webhooks go direct to a dedicated endpoint:
  - `POST /api/stripe/webhook` (verified + idempotent)

Each inbound item becomes:

- an append-only Event (`ks.signal.received`)
- and (optionally) a typed Node (e.g., Source, Artefact, or a "Signal" represented as an Artefact of type signal)

### B. The brains (Kernel controller service)

A small service (serverless worker or tiny Node service) that does three jobs:

- Psi Governor: estimate `psi_state` + set budgets/caps/permissions
- G-Loop orchestrator: create/update Mission + Run, advance stages, enforce gates
- Intent planner: produce Action Intents (drafted changes) rather than immediately acting

Where it runs (Tier A):

- A Vercel/Next.js API (or similar) + background job runner pattern
- Reads/writes to Supabase Postgres only (server-truth)

### C. Safety gate (policy layer)

This is your "Auto-run / Draft-for-approval / Escalate" switch. Implement it as:

- Permissions mode: `read_only | write_limited | write_enabled`
- Risk level: `low | med | high`
- Psi state override: `OVERHEAT` can temporarily downgrade permissions even if mission asked for more

The gate decides:

- which tool calls are allowed,
- whether the kernel may execute or must only draft,
- whether a human must approve.

### D. Actions out (connectors/tool layer)

Your connectors (Make/Zapier early) become actuators that only run when the kernel allows.

Key rule: the kernel never "does" an action until it has created an Action Intent and it passes gates.

In Tier A, Action Intents can be represented as:

- a Task + Artefact (draft payload)
- and an Event (`ks.intent.created`)

### E. Audit trail (append-only)

You already have this: `events` table with canonical `ks.*` event names.

For each intent/action:

- log what, why, inputs, outputs, and rollback plan if applicable.

### F. Proof checks (banking discipline)

Before you "bank" a workflow as a Phi-Script, require:

- dry-run harness (no external writes; produce diffs/previews)
- rollback test where reversible (create -> revert in a sandbox target or using a safe test resource)
- delayed re-check (time-shifted validation: did the outcome hold?)

You already have the right mechanism: portability experiments (`wrapper_swap`, `stake_swap`, `boundary_trap`) as hard gates before banking.

## 2) Minimal Tier-A build: components and responsibilities

### 2.1 Services (3 small pieces)

- Ingest API
  - `/api/signals/ingest`
  - `/api/stripe/webhook`

- Kernel controller
  - `handleSignal(signal)` -> creates/updates Mission + Run
  - `advanceRun(run_id)` -> stage transitions, gating
  - `proposeIntents(run_id)` -> drafts actions

- Executor (tool runner)
  - `executeIntent(intent_id)` only if allowed (permissions + risk + Psi)
  - In Tier A, this may call Make/Zapier webhook(s) to perform the external action.

(Keep "planner" and "executor" separate - it prevents accidental autonomy.)

## 3) Data in Supabase (what to store)

You can keep the Nodes/Edges/Events/Snapshots pattern, and add just one extra concept for Tier A operations:

### 3.1 Core tables

- `nodes` (Workspace/Project/Mission/Run/... stored as nodes)
- `edges`
- `events` (append-only)
- `snapshots` (optional)

### 3.2 Action Intent (recommended)

Add a lightweight `action_intents` table (or store as nodes of type `ActionIntent`):

Fields:

- `id`, `workspace_id`, `mission_id`, `run_id`
- `intent_type` (e.g., draft_email, create_task, update_crm, schedule_post)
- `target_system` (gmail/notion/substack/etc.)
- `payload_json` (the proposed change)
- `rollback_json` (if reversible)
- `status` (`draft|pending_approval|approved|executed|rolled_back|rejected`)
- `risk_level`, `permissions_required`
- timestamps

This makes "AI can propose; policies decide" literal.

## 4) How the G-Loop + Psi run the business without runaway

### 4.1 Stage 1 becomes your "investment" selector

`SALience_Investment_Psi` must set:

- mission `m` + stop rule
- `investment_plan_json` (timebox, probe budget, max sources)
- initial `psi_state` and caps (`psi_budget_json`, `lens_range_cap_json`)
- permissions mode (often start `write_limited`)

This is how you prevent infinite research loops and premature automation.

### 4.2 Psi Governor prevents the classic failure modes

Runaway patterns and what your governor does:

- Spiral (more sources, more rewriting, no commit)
  - cap sources, force `COMMIT_Plan`, impose stop rule

- Thrash (flip explore/exploit repeatedly)
  - lens swing rate limit + refractory period

- Rigid lock-in (stuck exploiting while mismatch persists)
  - force `Moves_Explore` (one discriminating probe) or return to map

- Overheat (high mismatch/drift)
  - downgrade permissions, block external writes, compress + recover

### 4.3 "Expanding Psi zone" (growth without chaos)

Scaling is simply: relax caps only after repeated stable runs.

A safe Tier-A growth rule:

After N successful runs in-band with low runaway flags and at least one successful portability proof, increase one of:

- `max_sources` (+1)
- `probe_budget` (+1)
- allow `write_enabled` earlier for low-risk mission types only

So the system gets more autonomous because it has earned it.

## 5) Tier-A workflow example: Newsletter engine (end-to-end)

### Signal in

"Sunday 07:00 newsletter cycle" (scheduler) or "new topic detected" (RSS/email digest).

### Kernel response

Create Mission:

- `m = draft published by 18:00, no uncited claims, CTA included`

Stage 1 sets:

- `timebox=90`, `probe_budget=2`, `max_sources=6`, start `write_limited`, estimate `psi_state`

### G-Loop execution (with logic)

- `Meaning_Explore / ABDUCT`: generate 3 angles + claims list
- `Moves_Explore / DIAGNOSE`: probe last 10 issues' metrics + pick the angle
- `Meaning_Exploit / DEDUCT`: tighten outline, mark claims that need citation
- `Moves_Exploit / POLICY`: draft issue + repurpose posts

### Actions out (intents, not immediate autonomy)

Create Action Intents:

- `draft_newsletter_md`
- `draft_linkedin_post`
- `draft_substack_note`
- `schedule_calendar_reminder`

Because it's Tier A: default Draft-for-approval for publishing steps.

### Proof checks

- Dry-run: generate diffs and previews
- Boundary trap: "any claim with needs_citation=true and support_status=unsupported blocks publish"
- Delayed re-check: 24h metrics snapshot recorded

### Banking

Only bank "Newsletter Weekly v1" Phi-Script after:

- wrapper swap (different topic week) and boundary trap pass
- optionally stake swap (run under a tighter timebox once)

## 6) Rollout plan for Tier A (so it's safe on day one)

### Week 1: Read-only kernel

- ingest signals
- create missions/runs
- generate intents only (no execution)

### Week 2: Write-limited execution

- allow low-risk tool actions: create tasks, draft docs, schedule reminders
- still no auto-publish, no money moves

### Week 3: Controlled writes

- allow publish/send only via approval
- add rollback tests where possible
- turn on corridor expansion only for proven mission classes

## 7) What you need to implement first (Tier-A "brains" MVP)

If you build only five things, build these:

- events append-only logging (already central to your design)
- Mission + Run state machine with gates
- Psi Governor (proxy-based) that sets budgets + permissions overrides
- Action Intents (draft/approve/execute)
- Banking gate requiring proof checks (swap/boundary)

That gives you a governed kernel that can scale.

## The best open-source building blocks for a Palantir-style ontology layer

### 1) TypeDB (strong schema + native rule inference)

Good when you want a typed conceptual model (entities/relations/attributes) and a real inference/rule capability inside the DB (not just app code). TypeDB CE is open source under MPL-2.0.

Why it fits your "generative model" idea:

- you can store your ontology as a schema-first knowledge base (Mission/Run/Claim/etc.)
- you can encode lightweight logical rules for "derived facts" (e.g., "claim is publishable if supported")

### 2) TerminusDB (versioned "git-for-data" KG + logic querying)

Good when you want the ontology to be versioned and auditable by design (diffs, time-travel), which matches your "log everything" posture. TerminusDB is Apache-2.0 and emphasises revision control, time-travel queries, schema constraints, and a Datalog/logic engine.

Why it fits:

- your ontology is your Public Programme, so versioning + diffs are a feature, not overhead
- time-travel helps with proof checks ("what did the model say at the time?")

### 3) RDF/OWL stack (Apache Jena) for "semantic web" style ontologies

Good if you specifically want RDF/OWL/SPARQL and standard ontology tooling, with rule-based reasoning available in the framework.

Trade-off: more "semantic web" engineering overhead; great for formal ontologies, sometimes heavier than you need for business ops.

### 4) Property-graph DBs (Neo4j Community / JanusGraph)

Good when you mainly want graph traversal + visualisation and are happy to keep inference in your kernel layer.

Neo4j Community is GPLv3 (fine for many internal uses, but you'll want to be deliberate about distribution/embedding).

JanusGraph is open-source and built for scale; useful if you ever need very large graphs (probably overkill initially).

### 5) Open Policy Agent (OPA) for the "Safety Gate" (policy as code)

Not an ontology store, but it's the cleanest way to implement your "AI can propose; policies decide" as a first-class component. OPA is an open-source policy engine designed to externalise policy decisions via Rego + APIs.

### 6) DataHub (optional) if you want a metadata/catalog "asset graph"

DataHub is an open-source metadata platform with a schema-first model - great for cataloguing datasets/models and lineage, but it's not primarily an ops-ontology for missions/actions.

## How to integrate this into your Kastel Stack kernel (Tier A)

Your kernel already has the right separation:

`Signals -> Psi/Safety gate -> G-Loop planning -> Action intents -> Execution -> Audit -> Proof/banking`

The ontology choice mainly affects where the "world model" lives.

### The most practical Tier-A pattern (recommended)

Keep Supabase/Postgres as the system-of-record (nodes/edges/events), and add an open-source KG as a read-optimised "ontology engine":

- Postgres (Supabase) remains canonical
  - stores nodes, edges, events, action_intents
  - this is your audit trail and entitlement/security boundary

- Projector syncs events -> KG
  - a small worker reads events and updates the KG (TypeDB or TerminusDB)
  - the KG becomes the place you do multi-hop queries, "what relates to what", and (optionally) rule inference

- Kernel consults KG during G-Loop stages
  - `MAP_Model` / `Meaning_Explore`: query KG for relevant objects/relationships
  - `PROBE_Test`: run diagnostic queries (and log results as Experiments)
  - `COMMIT_Plan`: apply policy checks (OPA) + derived-fact checks (KG rules or app rules)
  - `SWAP_Bank`: verify portability evidence exists before banking scripts

- OPA enforces the Safety Gate
  - OPA decides: auto-run vs draft-for-approval vs escalate
  - your kernel passes structured input (risk, Psi state, intent type, thresholds) and receives allow/deny + reason

This gives you an "ontology-driven" system without making the KG your payment/auth/audit source of truth.

### Which should you pick for your "generative model"?

If you want the closest feel to "ontology as executable model":

- TypeDB if you want typed conceptual modelling + rule inference inside the knowledge layer.
- TerminusDB if you want versioned ontology + diffs/time-travel + logic querying as a core feature (very aligned with your proof/audit posture).
- Add OPA either way for policy gating.

## How it maps to your "Public Programme" idea

Your Phi layer becomes literally:

- ontology objects (Mission/Run/Claim/Script/etc.)
- relationships (supported_by, derived_from, executes, produces...)
- actions as intents (draft/approve/execute)
- proof checks as first-class experiment objects

That's essentially "Palantir ontology thinking", but with open-source components and your Trident-G style governors.