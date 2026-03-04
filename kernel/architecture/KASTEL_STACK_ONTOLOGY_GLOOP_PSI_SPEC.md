# Kastel Stack Ontology MVP + G-Loop Logic + Ψ Self-Regulation Spec (v1.1)

This is a single consolidated spec you can paste into your repo as:
`KASTEL_STACK_ONTOLOGY_GLOOP_PSI_SPEC.md`

It defines a small, executable **Public Programme (Φ)** for an agentic workspace by combining:

1) a **typed ontology** (objects + links + actions + governance)
2) an enforced **G-Loop controller** (8 stages + Meaning/Moves × Explore/Exploit lenses)
3) explicit **logic/inference states** (abduct, deduce, diagnose, policy, update, invariance test) that are logged and queryable
4) a **Ψ Governor** for allostatic self-regulation (prevents runaway behaviours while allowing growth via corridor expansion)

## Design rule

Everything the agent touches must be: **typed**, **linked**, **permissioned**, and **logged**.

## Non-goals (MVP)

- Not a full enterprise ontology project.
- No heavy formal logic engine. We track inferential states and constraints, but keep storage light (JSON where useful).
- No “autonomous publish everywhere” without gating and audit trails.

---

## 0) Core concepts

### Public Programme (Φ)

Externalised cognition that can be repeated and improved: templates, checklists, scripts, evidence logs, decision records, dashboards.

### Ontology (Palantir-style, but lean)

Φ made machine-readable with:

- **Objects** (stable handles)
- **Links** (explicit relationships)
- **Actions** (operations that change state)
- **Governance** (permissions + audit trail)

### G-Loop

A controller/policy that determines:

- what to model
- what to probe
- when to commit
- how to update
- what may be banked as a reusable Φ-Script

### Logic / inference pack

A lightweight way to track “what kind of thinking is happening”:

- **Meaning vs Moves**: model inference vs policy inference
- **Explore vs Exploit**: option generation / uncertainty reduction vs compression / commitment
- **Inference modes**: abduct, deduce, diagnose, policy, update, invariance testing

### Ψ Governor (allostatic self-regulation)

A top-level regulator that constrains:

- **investment** (rigour budget, timeboxes, probe budgets)
- **mode range** (how far explore↔exploit can swing now)
- **permissions** (read-only vs write-limited vs write-enabled)
- **rate limits** (anti-spiral, anti-thrash)

Goal: maintain operation inside an **allostatic Ψ corridor** (non-zero load), avoid runaway behaviours, and allow gradual **corridor expansion** via validated success.

---

## A) Enumerations (constants)

### A1) G-Loop stage (8-stage state machine)

Used on **Mission** and **Run**:

1. `SALience_Investment_Psi`
2. `LENS_Select`
3. `MAP_Model`
4. `EXPLORE_Options`
5. `PROBE_Test`
6. `COMMIT_Plan`
7. `EOU_Execute`
8. `SWAP_Bank`

### A2) Lens mode (2×2 controller)

Used on **Run**:

- `Meaning_Explore`
- `Meaning_Exploit`
- `Moves_Explore`
- `Moves_Exploit`

### A3) Inference mode (logic tag)

Used on **Run**, and optionally on **Decision/Claim**:

- `ABDUCT` (generate candidate models/hypotheses)
- `DEDUCT` (derive implications, tighten constraints)
- `DIAGNOSE` (design/interpret discriminating probes)
- `POLICY` (select and execute a plan)
- `UPDATE` (revise model/weights given evidence)
- `INVARIANCE_TEST` (swap/boundary portability proof)

### A4) Risk levels

- `low`, `med`, `high`

### A5) Rigour budgets

- `low`, `med`, `high`

### A6) Permissions modes (workspace guardrails)

- `read_only` (agents may read; no writes to external systems)
- `write_limited` (agents may write only to safe targets, e.g., drafts/tasks)
- `write_enabled` (agents may publish/commit changes, but only after stage gates)

### A7) Claim types

- `descriptive`, `causal`, `forecast`, `normative`

### A8) Claim states

- `hypothesis`, `candidate`, `committed`, `published`

### A9) Support status

- `unsupported`, `partial`, `supported`

### A10) Experiment test types

- `diagnostic_probe`
- `wrapper_swap`
- `stake_swap`
- `boundary_trap`

### A11) Ψ state (zone state)

Used on **Run**:

- `FLAT` (underloaded / low novelty / low surprise; risk of stagnation)
- `IN_BAND` (viable corridor; full dynamic range permitted)
- `OVERHEAT` (overloaded / high surprise / high mismatch; risk of thrash)

---

## 1) Ontology object types (13)

All objects are stored as **Nodes** (or typed tables later). Each node has:

- `id`, `workspace_id`, `type`, `title`, `status` (optional), `owner_actor_id` (optional), `data_json`, timestamps.

### 1) Workspace

Container for a brand/business unit.

Key fields:
- `name`
- `owner_id`
- `default_policies` (JSON)
- `created_at`

### 2) Actor

Human user or agent identity.

Key fields:
- `actor_type` (`human|agent`)
- `display_name`
- `auth_user_id` (if human)
- `capabilities` (JSON: allowed actions, read/write scopes)

### 3) Project

Ongoing initiative (Newsletter, Product sprint, Research brief series).

Key fields:
- `status`
- `focus`
- `cadence`
- `success_signals_default` (JSON)

### 4) Mission

Single unit of work with explicit success signal **m** and stop rule.

Key fields:
- `salience_score` (0-5)
- `salience_rationale` (string)
- `m_success_signal` (string)
- `stop_rule` (string)
- `risk_level` (`low|med|high`)
- `deadline` (datetime)
- `rigour_budget` (`low|med|high`)
- `investment_plan_json` (JSON; see section 4.2 Gate 1)
- `permissions_mode` (`read_only|write_limited|write_enabled`) (recommended)
- `g_loop_stage` (enum A1)

Optional Ψ corridor parameters (for growth/scaling):
- `psi_corridor_params` (JSON; see section 9)

### 5) Task

Atomic actionable step.

Key fields:
- `status` (todo/doing/done etc.)
- `assignee_actor_id`
- `due_at`
- `depends_on` (array of task ids) (or via edges)

### 6) Artefact

Persistent output (outline, draft, brief, graphic, table, slide, dataset extract).

Key fields:
- `artefact_type` (string enum you define per workspace)
- `storage_ref` (URL/path)
- `version`
- `checksum` (optional)
- `created_by_run_id` (optional)

### 7) Source

Evidence input (web page, paper, email, transcript, dataset, interview note).

Key fields:
- `source_type`
- `locator` (URL / file id)
- `retrieved_at`
- `reliability_grade` (`A|B|C`) (simple)
- `snippet_refs` (array of pointers/quote locations)

### 8) Claim

A statement you might publish or act on, explicitly supported or not.

Key fields:
- `claim_text`
- `claim_type` (A7)
- `confidence` (0-1 or low/med/high)
- `needs_citation` (bool)
- `claim_state` (A8)
- `support_status` (A9)
- `logic_notes` (string; constraints/conditions)

### 9) Decision

A commitment point.

Key fields:
- `decision_text`
- `rationale`
- `alternatives_considered`
- `reversible` (bool)
- `inference_mode` (optional A3)

### 10) Experiment

Probe/test (A/B, diagnostic check, pilot, wrapper swap, stake swap, boundary trap).

Key fields:
- `test_type` (A10)
- `hypothesis` (string)
- `design` (string or JSON)
- `diagnostic_power` (`low|med|high`)
- `result_summary`
- `next_action`
- `deep_demand_tag` (string: invariant under test)
- `passed` (bool)
- `swap_cost` (`low|med|high`) (qualitative)
- `failure_mode` (string; empty if passed)

### 11) Metric Snapshot

Recorded outcomes at a timepoint.

Key fields:
- `metric_name`
- `value` (number/string)
- `time_window`
- `source_system`
- `notes`

### 12) Public Programme Script (Φ-Script)

Reusable workflow template (your “mindware script” for work).

Key fields:
- `trigger` (string)
- `steps` (ordered list; JSON)
- `checks` (ordered list; JSON)
- `boundary_traps` (list; JSON)
- `swap_tests` (plan; JSON)
- `stop_rule` (string)
- `outputs_expected` (list; JSON)

G-Loop additions:
- `g_loop_template` (the 8-stage checklist; JSON)
- `required_proofs` (e.g., `{wrapper_swap:1, stake_swap:0, boundary_trap:1}`)
- `stage_gates` (override gates; JSON, optional)

Ψ additions (script-level defaults):
- `psi_policy_defaults` (JSON; caps/timeboxes/rate limits)

### 13) Run

One execution of a Mission or Φ-Script (human+agent collaboration) with trace.

Base fields:
- `run_status` (queued|running|blocked|done|failed)
- `inputs_json`
- `outputs_json`
- `started_at`
- `ended_at`
- `cost_estimate` (optional)

G-Loop + logic fields:
- `g_loop_stage` (A1)
- `lens_mode` (A2)
- `inference_mode` (A3)

Inferential state payloads (JSON):
- `hypothesis_set_json` (array of {id, statement, weight/confidence})
- `working_model_json` (current model map: nodes/links/constraints)
- `probe_plan_json` (what observations discriminate what)
- `policy_json` (steps + thresholds + stop rule)
- `mismatch_signals_json` (what triggers return to Map/Probe)
- `portability_evidence_json` (swap outcomes summary)

Ψ Governor payloads (JSON):
- `psi_state` (A11)
- `psi_signals_json` (see section 8.2)
- `psi_budget_json` (current caps: timebox/probes/sources/writes)
- `lens_range_cap_json` (allowed lens states + max swing rate)
- `reentry_metrics_json` (optional; recovery dynamics)
- `runaway_flags_json` (thrash, spiral, rigidity)

---

## 2) Relationship types (edges)

Use a small, consistent verb set. Each edge has:
- `id`, `workspace_id`, `from_id`, `to_id`, `type`, `data_json`, timestamps.

Starter set:
- `workspace_has` -> (Project, Actor)
- `project_has` -> (Mission, Φ-Script)
- `mission_has` -> (Task, Artefact, Decision, Experiment, MetricSnapshot, Claim, Source)
- `artefact_cites` -> Source
- `artefact_contains` -> Claim
- `claim_supported_by` -> Source
- `decision_based_on` -> (Claim, Experiment, MetricSnapshot)
- `experiment_tests` -> (Claim or Decision)
- `run_executes` -> (Mission or Φ-Script)
- `run_produces` -> Artefact
- `run_records` -> (Decision, Experiment, MetricSnapshot) (recommended)
- `task_depends_on` -> Task

Optional but helpful:
- `script_derived_from` -> Run (when banking)

---

## 3) Action vocabulary (agent operations)

Every action produces an **Event** (append-only) and must respect:
- stage gates (G-Loop)
- permissions mode
- Ψ Governor caps (budgets, rate limits, lens restrictions)

Read / analyse:
- `FetchSources(query, constraints)`
- `SummariseSources(source_ids)`
- `ExtractClaims(source_ids)`
- `ScoreEvidence(claim_id)` (simple rubric)
- `PullMetrics(project_id, window)`
- `FindDuplicates(artefact_id, corpus_scope)` (optional)

Create / write (permission-gated):
- `CreateMissionCard(project_id, fields...)`
- `GenerateOutline(mission_id, angle)`
- `DraftArtefact(artefact_type, outline_id)`
- `CreateTasksFromOutline(mission_id)`
- `Repurpose(draft_id, channel)`
- `SchedulePublish(artefact_id, datetime)`
- `Publish(artefact_id)`

Test / prove (portability gate):
- `RunProbe(experiment_spec)` -> creates `Experiment(test_type=diagnostic_probe)`
- `RunWrapperSwap(script_id, new_wrapper)` -> `Experiment(wrapper_swap)`
- `RunStakeSwap(script_id, constraint)` -> `Experiment(stake_swap)`
- `RunBoundaryTrap(script_id, trap_case)` -> `Experiment(boundary_trap)`

Bank / reuse:
- `CreatePhiScriptFromRun(run_id)`
- `UpdatePhiScript(script_id, patch)`
- `StartRun(script_id, inputs)`

Ψ Governor actions (internal controller operations):
- `EstimatePsiState(run_id)`
- `SetPsiBudget(run_id, budget_json)`
- `SetLensCaps(run_id, lens_range_cap_json)`
- `TripRunawayGuard(run_id, flag, action)` (e.g., freeze writes, force probe, force stop)

---

## 4) The G-Loop controller (enforced behaviour)

### 4.1 State machine

Default forward path:
`SALience_Investment_Psi -> LENS_Select -> MAP_Model -> EXPLORE_Options -> PROBE_Test -> COMMIT_Plan -> EOU_Execute -> SWAP_Bank`

Allowed back edges (when mismatch/uncertainty demands it):
- `PROBE_Test -> MAP_Model`
- `EOU_Execute -> MAP_Model`
- `EOU_Execute -> PROBE_Test`
- `SWAP_Bank -> MAP_Model` (swap failure reveals brittleness)

### 4.2 Stage requirements (gates)

#### Gate 1: enter `LENS_Select` (Stage 1 must be complete)

Mission must have:
- `salience_score` (0-5) and `salience_rationale`
- `m_success_signal`
- `stop_rule`
- `rigour_budget`
- `investment_plan_json`
- `risk_level`
- `permissions_mode`

Minimum `investment_plan_json` schema:
- `timebox_minutes` (number)
- `probe_budget` (0-3 in MVP)
- `max_sources` (number)
- `evidence_threshold` (string, e.g., “2 sources support key claims”)
- `write_permissions_unlocked_at_stage` (default: `COMMIT_Plan`)

Stage 1 also triggers Ψ Governor initialisation:
- `EstimatePsiState(run_id)`
- `SetPsiBudget(run_id, psi_budget_json)`
- `SetLensCaps(run_id, lens_range_cap_json)`

#### Gate 2: enter `MAP_Model`
Run must set:
- `lens_mode` (A2)

#### Gate 3: enter `EXPLORE_Options`
Must exist:
- at least one map artefact (`Artefact.artefact_type` in {map, brief, model_note})
- `working_model_json` must be non-empty

#### Gate 4: enter `PROBE_Test`
Must exist:
- >= 2 options/hypotheses (in `hypothesis_set_json` or as Claims with state `hypothesis|candidate`)

#### Gate 5: enter `COMMIT_Plan`
Must exist:
- >= 1 diagnostic probe `Experiment(test_type=diagnostic_probe)` with `result_summary`
  OR an explicit `Decision` that timeboxed probing (allowed in MVP)

#### Gate 6: enter `EOU_Execute`
Must exist:
- a commitment `Decision` (chosen angle/plan)
- `policy_json` set (steps + thresholds + stop rule)

#### Gate 7: enter `SWAP_Bank`
Must exist:
- >= 1 produced artefact (draft/output)
- >= 1 metric snapshot OR explicit Decision “no metrics captured due to X”

#### Hard gate: bank Φ-Script (CreatePhiScriptFromRun)
Must exist in this Run/Mission:
- >= 1 portability proof `Experiment(test_type in {wrapper_swap, stake_swap, boundary_trap})`
- and at least one of:
  - a `boundary_trap` present, or
  - a Decision explicitly deferring boundary traps (not recommended)

### 4.3 Action permissions by stage (with Ψ overlay)

Baseline rules (cannot be loosened for high risk):

- Stages 1-5: external writes disabled (draft internally only)
- Stage 6: allow internal writes (docs, tasks), no publishing
- Stage 7: allow publish actions only if:
  - `permissions_mode = write_enabled`
  - `risk_level != high`
  - key Claims are `support_status != unsupported` (unless clearly labelled opinion)
  - Ψ Governor does not block writes (see section 8.4)
- Stage 8: allow banking scripts and patching scripts

---

## 5) Logic/inference placement (where “reasoning” lives)

### 5.1 Lens <-> inference mapping

- `Meaning_Explore` -> `ABDUCT`
- `Meaning_Exploit` -> `DEDUCT`
- `Moves_Explore` -> `DIAGNOSE`
- `Moves_Exploit` -> `POLICY`

### 5.2 Inferential state variables (tracked on Run)

Minimum tracked state:
- `hypothesis_set_json`
- `working_model_json`
- `probe_plan_json`
- `policy_json`
- `mismatch_signals_json`
- `portability_evidence_json`

### 5.3 Claims discipline

For any claim you might publish:
- Must be stored as a `Claim`
- Must have `claim_state` and `support_status`
- If `needs_citation=true`, then either:
  - there is >=1 `claim_supported_by` edge, or
  - it remains uncommitted (cannot progress to publish in high-risk contexts)

---

## 6) Event log (append-only audit trail)

### 6.1 Event table

`events` table (append-only):
- `id`
- `workspace_id`
- `ts`
- `actor_id`
- `app` (e.g., kastel_stack)
- `event_type`
- `entity_type` (Mission/Run/etc.)
- `entity_id`
- `payload_json`

### 6.2 Canonical event names

Stage + lens:
- `ks.gloop.stage_set`
- `ks.gloop.lens_set`
- `ks.logic.inference_mode_set`

Salience + investment:
- `ks.salience.set`
- `ks.investment.set`

Model + options:
- `ks.model.map_saved`
- `ks.model.hypotheses_updated`

Evidence:
- `ks.evidence.source_added`
- `ks.evidence.claim_extracted`
- `ks.evidence.claim_supported`

Probes:
- `ks.probe.planned`
- `ks.probe.ran`

Commit + execution:
- `ks.commit.decision_recorded`
- `ks.execute.task_created`
- `ks.execute.artefact_drafted`
- `ks.execute.published`
- `ks.observe.metrics_recorded`
- `ks.update.model_revised`

Portability + banking:
- `ks.swap.wrapper_ran`
- `ks.swap.stake_ran`
- `ks.swap.boundary_ran`
- `ks.bank.script_created`
- `ks.bank.script_patched`

Ψ Governor:
- `ks.psi.state_estimated`
- `ks.psi.budget_set`
- `ks.psi.lens_caps_set`
- `ks.psi.permission_set`
- `ks.psi.runaway_guard_tripped`

---

## 7) Minimal storage design

Option A (recommended MVP): Nodes + Edges + Events + Snapshots
- `nodes`: `id`, `workspace_id`, `type`, `title`, `status`, `owner_id`, `data_json`, timestamps
- `edges`: `id`, `workspace_id`, `from_id`, `to_id`, `type`, `data_json`, timestamps
- `events`: append-only action log
- `snapshots` (optional): per Mission/Project `latest_state_json` for fast load

Option B (later): Typed tables
Promote stable types (Mission, Run, Script) into first-class tables for performance and stricter constraints.

---

## 8) Ψ Governor (self-regulation + anti-runaway)

### 8.1 Overview

The Ψ Governor runs at:
- stage transitions, and
- any “costly” action (heavy research, many writes, publish/send/delete).

It outputs:
1) `psi_state` (FLAT / IN_BAND / OVERHEAT)
2) `psi_budget_json` (caps on investment)
3) `lens_range_cap_json` (allowed lens states and swing rate)
4) permission overrides (may temporarily downgrade `permissions_mode`)

### 8.2 Ψ signals (stored on Run as `psi_signals_json`)

MVP signals are simple and can be refined later:

- `surprise_proxy` (0-1): based on novel sources, contradiction rate, repeated revisions, user corrections
- `uncertainty_proxy` (0-1): number of live hypotheses, low support_status, model disagreement
- `mismatch_proxy` (0-1): repeated EOU mismatch, failed probes, failed swaps
- `drift_proxy` (0-1): time overruns, task churn, oscillating edits, aborted actions

Optional numeric fields if you want closer alignment later:
- `F_est`, `G_est`, `epsilon_est`, `pe_epsilon_est`

### 8.3 Ψ state classification (with hysteresis)

Use two thresholds for each transition to prevent oscillation:

- Enter `OVERHEAT` if:
  - `mismatch_proxy` high OR `drift_proxy` high OR `surprise_proxy` very high
- Exit `OVERHEAT` only after:
  - proxies fall below lower thresholds for a sustained window (refractory period)

Similarly for `FLAT`:
- Enter `FLAT` if:
  - `surprise_proxy` low AND `uncertainty_proxy` low for sustained window
- Exit `FLAT` only after:
  - a controlled novelty dose (small explore) completes

### 8.4 Default caps per Ψ state (anti-runaway)

#### `OVERHEAT` caps
- `probe_budget` <= 1
- `max_sources` reduced
- `max_hypotheses` reduced (force compression)
- `lens_mode` allowed:
  - allow `Moves_Exploit` (micro-commit) and `Meaning_Exploit` (simplify)
  - restrict `Meaning_Explore` to short, timeboxed bursts
- external writes: blocked unless explicitly confirmed by human + risk != high
- stage transitions rate-limited (no rapid cycling)

#### `IN_BAND` caps
- normal budgets from Mission investment plan
- full lens range allowed
- normal gating applies

#### `FLAT` caps (growth stimulus without chaos)
- allow brief `Meaning_Explore` with strict timebox
- require fast transition into `PROBE_Test` or `COMMIT_Plan` (avoid wandering)
- small novelty dose: cap hypotheses and sources; prefer “one new angle + one quick probe”

### 8.5 Runaway guards (detect & stop failure modes)

Track flags in `runaway_flags_json`:

- `thrash`: frequent lens toggles without progress
- `spiral`: source count / draft rewrites increasing without commit
- `rigidity`: stuck in exploit with persistent mismatch
- `premature_lockin`: commit with unsupported claims or without probe when rigour=med/high

Guard actions (`TripRunawayGuard`):
- freeze external writes
- force a single discriminating probe
- force compression (Meaning_Exploit) and a stop rule
- force stop / defer mission (if sustained OVERHEAT)

### 8.6 Growth/scaling: expanding the Ψ corridor safely

Corridor expansion is not “more intensity by default”. It is **validated capacity**.

Implement `psi_corridor_params` (Mission-level or Workspace-level) as:
- `target_band_version`
- `band_width` (initial)
- `growth_policy` (JSON)

Suggested MVP growth policy:
- If the system completes N runs in `IN_BAND` with:
  - stable outcomes (m met directionally),
  - low runaway flags,
  - and at least one successful swap proof,
  then:
  - slightly relax caps (e.g., +1 max_sources or +1 probe_budget) for similar mission types.

This gives you “expanding Ψ zone” through evidence, not through uncontrolled escalation.

---

## 9) Optional Ψ corridor parameters (for later fidelity)

If you want tighter alignment with your theory later, store (as JSON):

`psi_corridor_params`:
- `F_star_minus`, `F_star_plus` (corridor bounds)
- `F_bar_star` (centre)
- `quantiles_version`
- `measurement_model` (which proxies/metrics feed F_est)

MVP can run purely on proxies (section 8.2).

---

## 10) MVP UI (two screens)

### 10.1 Mission Board

Must show:
- Mission cards (salience + rationale, m, stop rule, deadline, risk)
- Investment plan (timebox, probe budget, max sources, evidence threshold)
- Current `g_loop_stage`
- Current `psi_state` (FLAT/IN_BAND/OVERHEAT) + active caps
- Current `lens_mode` (if active run exists)
- Tasks + latest artefacts
- Evidence status (count of unsupported committed claims)
- Buttons: Run probe, Commit, Publish (enabled only when gates + Ψ allow)

### 10.2 Script Library (Φ)

Must show:
- List of Φ-Scripts
- Start Run
- Swap tests history (wrapper/stake/boundary outcomes)
- Bank improvements (patch from run outcomes)
- Required proofs policy per script
- Script-level Ψ defaults (caps/timeboxes)

---

## 11) Implementation checklist (ship-ready MVP)

1) Implement `nodes`, `edges`, `events`, optional `snapshots`.
2) Implement Mission creation with required fields:
   - salience + rationale
   - m + stop rule
   - rigour + investment plan
   - risk + permissions
3) Implement Run creation and stage transitions with gates.
4) Implement lens modes + inference mode tagging.
5) Implement Experiment types incl. diagnostic + swap/boundary.
6) Implement bank Φ-Script hard gate requiring portability proof.
7) Implement Ψ Governor:
   - psi_state estimation (proxy-based)
   - caps/budgets + lens caps + hysteresis
   - runaway guards
   - growth policy (optional initial version)
8) Build Mission Board + Script Library UI with stage-aware and Ψ-aware buttons.
9) Ensure every action emits an Event (including Ψ decisions).

---