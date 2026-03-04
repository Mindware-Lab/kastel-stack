# Kastel Stack Horizontal x Vertical Scaling Architecture (v0.1)

Purpose: define a domain-based architecture for HRPLab products/services where each domain has explicit input/output contracts, and can scale independently.

Design constraints:
- Uneven growth is expected (some domains scale faster than others).
- G-Loop + Psi Governor remain the control plane across all domains.
- Safety gate and audit requirements apply to every cross-domain action.

---

## 1) MVP Domain Set (Start Here)

Use 4 domains first, then add 2 expansion domains when needed.

### D1) Audience and Demand Domain
Scope:
- Multi-site lead capture, attribution, segmentation, outreach triggers.
- Content-to-lead ingestion signals (Substack, LinkedIn, site forms).

Primary outputs:
- `LeadQualified.v1`
- `SegmentAssigned.v1`
- `EngagementSignal.v1`

### D2) Commerce and Entitlements Domain
Scope:
- Offer routing, checkout events, payment verification, lifetime entitlement ledger.
- Access provisioning and reconciliation.

Primary outputs:
- `PurchaseVerified.v1`
- `EntitlementsGranted.v1`
- `AccessMismatchDetected.v1`

### D3) Delivery and Coaching Domain
Scope:
- Onboarding, app/course activation, coaching credits, booking, session prep/recap.
- Support and community routing.

Primary outputs:
- `CustomerActivated.v1`
- `SessionCompleted.v1`
- `RetentionRiskDetected.v1`

### D4) Outcomes and Evidence Domain
Scope:
- Progress tracking, benchmark deltas, experiments, evidence synthesis.
- Research/news -> Trident-G article -> publication evidence chain.

Primary outputs:
- `OutcomeSnapshotRecorded.v1`
- `ExperimentResultRecorded.v1`
- `EvidencePackReady.v1`

### Expansion domains (add later)
- D5) Partnerships and Talent Domain.
- D6) Platform Reliability and Governance Domain.

---

## 2) Contract-First Domain Boundaries

Each domain exchange must be contract-driven. No hidden shared-state coupling.

### 2.1 Canonical Contract Envelope

All events/messages between domains follow this envelope:

```json
{
  "contract_name": "LeadQualified",
  "contract_version": "v1",
  "event_id": "uuid",
  "idempotency_key": "string",
  "producer_domain": "D1_AudienceDemand",
  "consumer_domain": "D2_CommerceEntitlements",
  "workspace_id": "uuid",
  "mission_id": "uuid-or-null",
  "run_id": "uuid-or-null",
  "occurred_at": "2026-03-03T10:12:00Z",
  "risk_level": "low|med|high",
  "psi_state": "FLAT|IN_BAND|OVERHEAT",
  "trace_refs": {
    "event_ids": ["uuid"],
    "node_ids": ["uuid"]
  },
  "payload": {}
}
```

Required behavior:
- Consumer must be idempotent on `idempotency_key`.
- Unknown contract version is rejected to dead-letter queue.
- Every accept/reject emits an append-only `ks.contract.*` event.

### 2.2 Domain Contract Registry (MVP)

Store in repo + DB:
- `contract_name`
- `version`
- `schema`
- `producer`
- `consumer`
- `validation_rules`
- `rollout_status` (`draft|active|deprecated`)

---

## 3) Inter-Domain Contracts (MVP Critical Paths)

### D1 -> D2 (Demand to Commerce)
1. `LeadQualified.v1`
   - Input: lead profile, segment, intent score.
   - Output effect: offer routing and checkout path selection.

2. `OfferIntentRequested.v1`
   - Input: recommendation context + risk tags.
   - Output effect: create proposal/offer intent with gate status.

### D2 -> D3 (Commerce to Delivery)
1. `EntitlementsGranted.v1`
   - Input: product access flags and coaching credit allocation.
   - Output effect: onboarding flow start and coaching readiness setup.

2. `AccessMismatchDetected.v1`
   - Input: payment/access inconsistency details.
   - Output effect: support escalation and reconciliation workflow.

### D3 -> D4 (Delivery to Outcomes)
1. `SessionCompleted.v1`
   - Input: session metadata, completion artifacts, follow-up tasks.
   - Output effect: update progress model and evidence timeline.

2. `RetentionRiskDetected.v1`
   - Input: inactivity and adherence signals.
   - Output effect: causal test or intervention recommendation.

### D4 -> D1 (Outcomes to Demand)
1. `EvidencePackReady.v1`
   - Input: validated findings + approved claims.
   - Output effect: content and campaign generation cues.

2. `ClaimRiskFlagged.v1`
   - Input: unsupported/high-risk claim markers.
   - Output effect: publish block or escalation in demand/content workflows.

---

## 4) Vertical Layers (Per Domain)

Apply the same vertical stack to each domain so scaling is uniform.

### L1) Channel and Connector Layer
- Web forms, Stripe, Podia, Discord, Zoom, Substack, social APIs.
- Converts external signals into typed ingest events.

### L2) Contract and Ingest Layer
- Validates schema + version.
- Writes canonical event to append-only log.
- Routes to domain queue/stream.

### L3) Kernel Orchestration Layer
- G-Loop stage transitions.
- Mission/Run lifecycle updates.
- Operator execution trace (Y-op trace where relevant).

### L4) Governance Layer
- Psi Governor: budgets, caps, permission overrides.
- Safety gate: auto-run, draft-for-approval, escalate.
- Policy checks (risk/legal/claim/money controls).

### L5) Intent and Execution Layer
- Creates Action Intents.
- Executes only approved intents via connectors.
- MVP default: n8n is the primary execution/orchestration adapter.
- Handles retries, rollback, and compensation actions.

### L6) Truth and Evidence Layer
- Supabase/Postgres system of record.
- Nodes/edges/events/action_intents.
- KPI views and evidence artifacts.

### L7) Observability and Reliability Layer
- SLOs, queue lag, error rates, drift alerts.
- Dead-letter queue processing.
- Incident and runbook triggers.

---

## 5) Uneven Scaling Model (Key Requirement)

Scale domains independently, not as one monolith.

### D1 Audience and Demand (likely first to spike)
Scaling knobs:
- High-throughput ingest workers.
- Queue partitions by source/channel.
- Burst autoscaling for campaign/content spikes.

### D2 Commerce and Entitlements (high integrity first)
Scaling knobs:
- Strong idempotency guarantees.
- Strict transaction boundaries.
- Priority queues for payment/access reconciliation.

### D3 Delivery and Coaching (human latency mixed)
Scaling knobs:
- Scheduler workers for reminders/follow-ups.
- SLA queues for support and coaching operations.
- Manual override tooling with audit.

### D4 Outcomes and Evidence (compute-heavy bursts)
Scaling knobs:
- Batch jobs for analytics/evidence packs.
- Separate experiment compute pool.
- Cached read models for dashboards.

---

## 6) Domain Autonomy Levels

Use staged autonomy by domain and risk class.

Level 0: Observe-only
- Ingest + classify + suggest.

Level 1: Draft-only
- Create intents/artifacts, no external writes.

Level 2: Limited execute
- Low-risk reversible actions auto-run.

Level 3: Controlled execute
- Broader action set with approvals and rollback tests.

Default recommendation:
- D2 money-adjacent workflows stay at Level 1-2 until long-run reliability is proven.
- D1 and D4 can move faster where claim-risk gates are enforced.

---

## 7) Contract Governance Rules

1. Every domain boundary requires an explicit versioned contract.
2. Contract changes are backward compatible for one active version window.
3. Breaking changes require:
- new version,
- migration note,
- replay test,
- rollback plan.
4. No direct cross-domain DB writes; all cross-domain state changes via contracts/events.
5. Every domain decision emits rationale in `events`.

---

## 8) Suggested MVP Build Sequence

Phase A:
1. Implement D1 -> D2 -> D3 core contracts.
2. Stand up L1-L6 for these three domains.
3. Keep D4 read-mostly (basic outcomes snapshots).

Phase B:
1. Expand D4 experimentation + evidence pipeline.
2. Close D4 -> D1 loop for research-content-growth flywheel.
3. Introduce domain-specific SLOs and queue partitions.

Phase C:
1. Add D5 (Partnerships/Talent).
2. Add D6 (Platform Reliability/Governance) as a dedicated domain.

---

## 9) Architecture Definition of Done (MVP)

1. Domain contracts documented and versioned for all critical paths.
2. All cross-domain actions are idempotent and auditable.
3. Safety gate and Psi overrides active in every domain.
4. Domain-specific scaling controls in place (queues, workers, SLOs).
5. At least one end-to-end flow proven:
- `LeadQualified -> PurchaseVerified -> EntitlementsGranted -> CustomerActivated -> OutcomeSnapshotRecorded -> EvidencePackReady`.
