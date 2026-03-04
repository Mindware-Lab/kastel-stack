# Appendix: Y-Operator Wiring for Kastel Stack G-Loop Kernel (Tier-A Core)

This appendix specifies how the Trident-G "syntax of thought" (Y-operators) plugs into the Kastel Stack **G-Loop + Logic + Psi Governor** kernel.

Goal: make reasoning **typed, auditable, and gateable**:
- the **G-Loop** selects stage + lens regime
- the **Y-operators** execute the micro-moves inside each stage
- the **Psi Governor** constrains which operators are allowed, plus budgets and swing limits

---

## 1) Data model additions (minimal)

### 1.1 Run fields (add)
- `y_ops_trace_json`: ordered list of operator executions

**y_ops_trace_json item schema (MVP)**
```json
{
  "seq": 12,
  "y_op": "Y10",
  "stage": "PROBE_Test",
  "lens_mode": "Moves_Explore",
  "inference_mode": "DIAGNOSE",
  "psi_state": "IN_BAND",
  "inputs_ref": ["node:Claim:abc", "node:Source:def"],
  "outputs_ref": ["node:Experiment:xyz"],
  "rationale": "Need to discriminate Model A vs B",
  "ts": "2026-03-03T10:12:00Z"
}
```

### 1.2 Mission fields (already in spec, confirm)

- `salience_score`, `salience_rationale`
- `rigour_budget`
- `investment_plan_json`
- `permissions_mode`

### 1.3 Experiment fields (confirm)

- `test_type` (diagnostic_probe / wrapper_swap / stake_swap / boundary_trap)
- `deep_demand_tag`, `passed`, `swap_cost`, `failure_mode`

### 1.4 Phi-Script fields (add)

- `y_chain_template_json`: default Y-operator chain per stage
- `required_proofs`: extend to include **proof harness** subtests (see section 5)

---

## 2) Kernel principle: operators are executable units with typed IO

Every Y-operator invocation MUST:

1. append a `y_ops_trace_json` entry
2. emit an Event `ks.yop.ran`
3. create/update nodes that represent its outputs (Claim/Experiment/Decision/Artefact/etc.)
4. respect Psi caps and permissions (see section 6)

---

## 3) Stage <-> operator mapping (canonical)

| G-Loop stage              | Required operator(s) | Typical optional operators                   |
| ------------------------- | -------------------- | -------------------------------------------- |
| `SALience_Investment_Psi` | **Y0**               | Y13/Y14/Y15 (if corridor issue)              |
| `LENS_Select`             | (output of **Y0**)   | -                                            |
| `MAP_Model`               | **Y1**               | Y2, Y3, Y8                                   |
| `EXPLORE_Options`         | (Y1 iterative)       | Y4, Y5, Y12                                  |
| `PROBE_Test`              | **Y10**              | Y7                                           |
| `COMMIT_Plan`             | **Y9**               | Y7                                           |
| `EOU_Execute`             | **EOU macro**        | Y7, Y10, Y14, Y15                            |
| `SWAP_Bank`               | **Y11**              | Y7 (failure diagnosis), Y1/Y8 (revise model) |

**Hard gates**

- No stage advance past `SALience_Investment_Psi` without a Y0 record.
- No entry to `COMMIT_Plan` without Y10, unless timebox override Decision exists.
- No banking without Y11 portability evidence + proof harness requirements.

---

## 4) Tier-A "core" operator set

**Always available in Tier-A**

- Y0 (Orient / salience / investment / permissions / Psi init)
- Y1 (Map)
- Y7 (Evaluate)
- Y9 (Satisfice / commit)
- Y10 (Discriminate / probe)
- Y11 (Stabilise portability / swaps / boundary)
- Y13 (Loop prevent / anti-runaway)
- Y14 (Recover / restore band)
- Y15 (Externalise / degrade safely)

**Often useful**

- Y2 (Chunk)
- Y3 (Unpack)
- Y5 (Abstract)

**Optional**

- Y4 (Analogy)
- Y6 (Classify)
- Y8 (Re-represent)
- Y12 (Meta-map)

---

## 5) Operator specs (required inputs/outputs, events, gates)

### Y0 - Orient (salience + investment + Psi initialisation)

**Purpose:** set relevance variable (**m**), stop rule, rigour budget, and initialise Psi caps/lens constraints.

**Inputs**

- Mission draft fields (goal, deadline, risk)
- current workspace context (recent events/metrics)
- (optional) prior similar runs

**Outputs (must update Mission + Run)**

- Mission:
  - `salience_score`, `salience_rationale`
  - `m_success_signal`, `stop_rule`
  - `rigour_budget`, `investment_plan_json`
  - `permissions_mode`
- Run:
  - `psi_state` (initial)
  - `psi_budget_json` (derived from investment plan + Psi state)
  - `lens_range_cap_json` (allowed lens states + swing rate)
  - `lens_mode` initial (see below)
  - `inference_mode` set consistent with lens

**Y0 must explicitly output `lens_mode_initial`**

- If uncertainty is high -> start `Meaning_Explore`
- If mismatch/urgency is high -> start `Moves_Explore` (probe first)
- If risk is high -> constrain to `Meaning_Exploit` + `Moves_Exploit` with write blocked

**Events**

- `ks.salience.set`
- `ks.investment.set`
- `ks.psi.state_estimated`
- `ks.psi.budget_set`
- `ks.psi.lens_caps_set`
- `ks.yop.ran` (payload includes Y0 outputs)

**Gate created**

- passing Y0 allows transition to `LENS_Select` (Stage 2)

---

### Y1 - Map (generative model / structure)

**Purpose:** create the minimal working model for the mission (claims, constraints, causal story, operator chain candidates).

**Inputs**

- Mission m + constraints
- relevant artefacts/sources/metrics
- current hypothesis set (if any)

**Outputs**

- Artefact: `map|brief|model_note` (required for Gate 3)
- Run: `working_model_json` (non-empty)
- Claims created/updated:
  - new `Claim` nodes in `hypothesis` or `candidate` state
  - `logic_notes` for key constraints ("only if...", "depends on...")

**Events**

- `ks.model.map_saved`
- `ks.yop.ran`

**Gate created**

- enables `EXPLORE_Options` once map exists and model JSON is non-empty

---

### Y2 - Chunk (compress representation)

**Purpose:** reduce complexity; create compact representations that are stable under load.

**Inputs**

- working model + artefacts

**Outputs**

- updated `working_model_json` (fewer nodes/links)
- optional "chunked" artefact version

**Events**

- `ks.model.map_saved` (if artefact updated)
- `ks.yop.ran`

---

### Y3 - Unpack (expand representation)

**Purpose:** add detail to resolve ambiguity or support claims.

**Inputs**

- working model + uncertainty/mismatch signals

**Outputs**

- updated `working_model_json` (new links/variables)
- may add new Claims in `hypothesis` state

**Events**

- `ks.model.map_saved`
- `ks.yop.ran`

---

### Y4 - Analogy (transfer candidate structure)

**Purpose:** generate candidate frames based on similar cases.

**Inputs**

- target mission + archive of prior runs

**Outputs**

- hypotheses added to `hypothesis_set_json`
- optional claims tagged "analogy-derived"

**Events**

- `ks.model.hypotheses_updated`
- `ks.yop.ran`

**Psi rule**

- restricted or timeboxed in `OVERHEAT` (avoid wandering)

---

### Y5 - Abstract (derive invariants)

**Purpose:** produce higher-level operator templates and invariants for transfer.

**Inputs**

- multiple cases/runs or multiple hypotheses

**Outputs**

- updates `working_model_json` with invariant tags
- adds `deep_demand_tag` candidates for later Y11 tests

**Events**

- `ks.model.map_saved`
- `ks.yop.ran`

---

### Y6 - Classify (slot into known categories)

**Purpose:** reduce uncertainty by classification.

**Outputs**

- optional labels on Mission/Claims (e.g., "pricing research", "content pipeline", "entitlement ops")

**Events**

- `ks.yop.ran`

---

### Y7 - Evaluate (logic hygiene: confounds, support, directionality)

**Purpose:** enforce evidence discipline and causal caution; update claim support states.

**Inputs**

- Claims and Sources
- existing experiments/metrics

**Outputs (must update Claim nodes)**

- `Claim.support_status`: unsupported/partial/supported
- `Claim.logic_notes`: conditions, confounds, "what would falsify"
- (optional) Decision: "timebox override" or "avoid claim"

**Events**

- `ks.evidence.claim_supported` (on status changes)
- `ks.yop.ran`

**Gate effect**

- can block publish if committed claims remain unsupported

---

### Y8 - Re-represent (change problem representation)

**Purpose:** when stuck, change the representational scheme (new variables, new partition, new wrapper).

**Inputs**

- mismatch signals, failed probes, failed swaps

**Outputs**

- revised `working_model_json`
- often triggers return to `MAP_Model`

**Events**

- `ks.update.model_revised`
- `ks.yop.ran`

**Psi rule**

- disallow in `OVERHEAT` unless explicitly escalated, because it can trigger over-work.

---

### Y9 - Satisfice / Commit (MI-tighten + stop rule)

**Purpose:** lock a plan under rigour budget; commit to one model/angle and stop expanding.

**Inputs**

- hypotheses + probe results (or timebox override)
- current Psi caps

**Outputs**

- Decision node (required):
  - chosen plan/angle
  - rationale
  - stop rule confirmation
- Run:
  - `policy_json` (steps + thresholds)
  - lens set to `Moves_Exploit` for execution phase
- Claims:
  - set key claims to `committed` only if support plan exists

**Events**

- `ks.commit.decision_recorded`
- `ks.yop.ran`

**Gate created**

- enables `EOU_Execute`

---

### Y10 - Discriminate / Probe (VOI)

**Purpose:** run a discriminating test that separates hypotheses.

**Inputs**

- at least 2 competing hypotheses/models
- probe budget and max sources from Psi caps

**Outputs (must create Experiment)**

- `Experiment(test_type=diagnostic_probe)` with:
  - hypothesis A vs B (explicit)
  - observation chosen (discriminating)
  - result_summary
  - next_action (which model survives)
- Run:
  - update `probe_plan_json` and optionally belief weights in `hypothesis_set_json`

**Events**

- `ks.probe.planned` (optional)
- `ks.probe.ran`
- `ks.yop.ran`

**Gate created**

- enables `COMMIT_Plan` (unless timebox override)

---

### EOU - Execute / Observe / Update (macro)

**Purpose:** enact the plan and update model based on outcomes.

**Inputs**

- policy_json
- permissions mode
- Psi caps

**Outputs (OBSERVE is mandatory)**

- Artefact(s) created/updated (drafts, tasks, etc.)
- MetricSnapshot OR explicit Decision "no metrics captured"
- Run:
  - update `psi_signals_json`
  - update `mismatch_signals_json`
  - route decision: return to Y7/Y10/Y14/Y15 if mismatch persists

**Events**

- `ks.execute.artefact_drafted`
- `ks.execute.task_created`
- `ks.execute.published` (only if allowed)
- `ks.observe.metrics_recorded`
- `ks.update.model_revised` (when update occurs)
- `ks.yop.ran` (record EOU as a macro step)

---

### Y11 - Stabilise portability (swap + boundary + proof harness)

**Purpose:** verify invariance under transformation before banking.

**Inputs**

- deep_demand_tag (invariant under test)
- swap plans + boundary traps
- proof requirements from Phi-Script / workspace policy

**Outputs (must create at least one portability Experiment)**

- `Experiment(test_type in {wrapper_swap, stake_swap, boundary_trap})`
  - deep_demand_tag
  - passed/failure_mode
  - swap_cost
- Update Run portability evidence:
  - `portability_evidence_json`

**Proof harness subtests (Tier-A banking discipline)**
Record as either:

- additional Experiment nodes, OR
- fields inside Y11 trace payload

Required subtests (configurable by script):

1. `dry_run` (diff/preview, no external writes)
2. `rollback_test` (where reversible, in safe target)
3. `delayed_recheck` (scheduled verification)

**Events**

- `ks.swap.wrapper_ran`
- `ks.swap.stake_ran`
- `ks.swap.boundary_ran`
- `ks.yop.ran`

**Gate created**

- only after Y11 proofs may `CreatePhiScriptFromRun` succeed

---

### Y13 - Loop prevent (anti-runaway)

**Purpose:** detect thrash/spiral/rigidity and apply guardrails.

**Inputs**

- runaway_flags_json + stage transition rate + source counts + rewrite counts

**Outputs**

- may set:
  - freeze external writes
  - force one probe (Y10)
  - force compression (Y2/Y9)
  - force stop/defer mission

**Events**

- `ks.psi.runaway_guard_tripped`
- `ks.yop.ran`

---

### Y14 - Recover (restore Psi band)

**Purpose:** corridor protection: reduce load, recover stability, then re-enter.

**Outputs**

- Run:
  - psi_state moves toward `IN_BAND`
  - permissions downgraded temporarily
  - action = "recover protocol" note (Artefact)
- may end current run as "paused"

**Events**

- `ks.psi.permission_set`
- `ks.yop.ran`

---

### Y15 - Externalise / degrade safely

**Purpose:** when band is violated or risk is high, externalise to Phi and escalate to human.

**Outputs**

- Artefact: "escalation brief" (what/why/next steps)
- Action Intent: `pending_approval` only
- Run: mark `blocked` awaiting human

**Events**

- `ks.intent.created` (if intent created)
- `ks.yop.ran`

---

## 6) Psi Governor constraints on operators (Tier-A defaults)

### `OVERHEAT`

Allowed operators:

- Y0, Y2, Y7, Y9, Y10 (single probe), Y13, Y14, Y15, EOU (limited)
- Restricted:
  - Y4, Y8, Y12 (timeboxed or blocked)
- Caps:
  - `probe_budget` <= 1
  - reduce `max_sources`, reduce `max_hypotheses`
  - block publish unless explicit human confirmation + risk != high

### `IN_BAND`

Allowed:

- all Tier-A core operators
- Caps:
  - use Mission investment plan

### `FLAT`

Allowed:

- brief Y4/Y5 (timeboxed), then force Y10 or Y9
- Caps:
  - small novelty dose, avoid wandering

---

## 7) Gate rules expressed in operator terms (summary)

- **Gate into LENS_Select**: Y0 executed and recorded (Mission + Run fields present)
- **Gate into EXPLORE_Options**: Y1 produced map artefact + working_model_json non-empty
- **Gate into COMMIT_Plan**: Y10 executed OR timebox override Decision exists
- **Gate into EOU_Execute**: Y9 executed and policy_json set
- **Gate into banking**: Y11 executed with required proofs (swap/boundary + proof harness)

---

## 8) Event additions (optional but recommended)

Add canonical operator events:

- `ks.yop.ran` (payload includes y_op, stage, lens, inference, inputs/outputs refs)
- `ks.yop.blocked` (when Psi/policy disallows an operator)

---

## 9) Minimal `y_chain_template_json` examples

### Newsletter Phi-Script (Tier-A)

```json
{
  "SALience_Investment_Psi": ["Y0"],
  "MAP_Model": ["Y1", "Y7"],
  "EXPLORE_Options": ["Y1", "Y5"],
  "PROBE_Test": ["Y10"],
  "COMMIT_Plan": ["Y9", "Y7"],
  "EOU_Execute": ["EOU"],
  "SWAP_Bank": ["Y11"]
}
```

### Payments/entitlements incident Phi-Script (Tier-A)

```json
{
  "SALience_Investment_Psi": ["Y0"],
  "MAP_Model": ["Y1"],
  "PROBE_Test": ["Y10"],
  "COMMIT_Plan": ["Y9"],
  "EOU_Execute": ["EOU"],
  "SWAP_Bank": ["Y11"]
}
```

---

## 10) Implementation note (keep it lean)

- Start by implementing only the Tier-A core set (Y0/Y1/Y7/Y9/Y10/Y11 + EOU + Y13/Y14/Y15).
- Treat the rest as "advanced operators" enabled later.
- Your kernel remains: stage gates + Psi caps + policy approvals + audit log.