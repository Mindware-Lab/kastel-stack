# HRPLab Workflow Master List (Kastel Stack)

Purpose: master workflow catalog for IQ Mindware / Trident G operations, using Kastel Stack governance.

Kernel policy for all workflows:
- Safety gate mode: `Auto-run` (safe/reversible), `Draft-for-approval` (default for customer-facing), `Escalate` (money/legal/high risk).
- Every workflow must emit append-only events and include rollback/recovery notes where applicable.
- Workflows are only banked after proof checks: dry-run, rollback test (if reversible), delayed re-check.

---

## 1) Demand Generation and Lead Intake

1. Multi-site lead capture and dedupe (`iqmindware.com`, `highiqpro.com`, `i3mindware.com`).
2. Lead source attribution (site/page/campaign/content asset).
3. Quiz or intake form scoring (goal, urgency, coaching readiness, budget band).
4. ICP fit routing (self-serve vs coaching-assisted path).
5. Lead response triage (auto acknowledgement + human escalation for high-value leads).
6. Podia list sync and suppression logic (avoid duplicate sends/spam risk).
7. Re-engagement workflow for dormant leads/subscribers.

## 2) Assessment and Offer Matching

1. Baseline assessment intake (IQ proxies, resilience scale, five-axis profile).
2. Pre/post benchmark registry and consent status logging.
3. Automated plan recommendation (Standalone vs Premium vs Platinum).
4. Offer page personalization by profile/segment.
5. Objection-handling draft generation (science, expectations, fit criteria).
6. High-claim copy gate (scientific/legal review before publish).

## 3) Checkout, Entitlements, and Access Control

1. Purchase event ingest (primary payment rail).
2. Payment verification and idempotent processing.
3. Lifetime entitlement ledger update (single source of truth).
4. Product provisioning (app/course/Discord/Substack/certification flags).
5. Access mismatch reconciliation (payment succeeded but entitlement failed).
6. Refund and exception handling through Finance Intents.
7. Manual override workflow (audited, role-gated).

## 4) Onboarding and Activation

1. New customer welcome sequence with correct product path.
2. Protocol onboarding (Zone -> capacity drill -> strategy mindware -> application cue).
3. App setup and fallback guidance (primary app, Brain Workshop fallback if needed).
4. First-week activation nudges (session reminders, setup checks).
5. Early drop-off detection and intervention drafts.
6. Milestone trigger: first successful training week completion.

## 5) Coaching Operations

1. Coaching credit ledger (included sessions by tier).
2. Intake-to-booking flow (Zoom link, prep form, confirmation).
3. Session reminders and no-show policy enforcement.
4. Pre-session coach brief assembly from customer metrics/progress.
5. Post-session recap + homework delivery.
6. Follow-up cadence scheduling and accountability prompts.
7. Session completion gates for premium deliverables (e.g., LinkedIn recommendation).

## 6) Learning Adherence and Progress Intelligence

1. Training adherence monitoring (frequency, consistency, completion).
2. Performance trend tracking (n-back progression and related markers).
3. Plateau detection and protocol adjustment suggestions.
4. Progress narrative generation (before vs now summaries).
5. At-risk churn detection (inactivity, missed sessions, low engagement).
6. Recovery pathways (re-activation sequence + optional coaching prompt).
7. Milestone recognition (certificate and social proof request eligibility).

## 7) Community and Support

1. Discord onboarding (public vs private channel access by entitlement).
2. Community moderation and trust/safety escalation.
3. Support triage (billing, access, technical, protocol questions).
4. FAQ answer drafting and knowledge base updates.
5. High-risk support tickets escalation (payment/data/reputational issues).
6. Voice-of-customer tagging into product/research backlog.

## 8) Research and Thought Leadership Engine (Trident G + G-Loop)

1. Research/news signal intake:
- New papers, preprints, policy updates, AI/cognitive intervention news, platform changes.

2. Evidence triage and tagging:
- Relevance to fluid intelligence, resilience, strategic intelligence, metacognition, AI augmentation.

3. Claim extraction and support scoring:
- Build typed Claim nodes with support status and citation requirements.

4. Angle generation (Meaning_Explore / ABDUCT):
- 2-4 article angles tied to Trident G protocol or G-Loop principles.

5. Discriminating probe (Moves_Explore / DIAGNOSE):
- Select angle using VOI checks (audience fit, novelty, evidence depth, business relevance).

6. Draft composition (Moves_Exploit / POLICY):
- Draft long-form Substack article + companion LinkedIn variant + optional YouTube script outline.

7. Claim-risk gate:
- Block publish if unsupported high-impact claims remain uncited.
- Escalate controversial claims for human review.

8. Repurposing workflow:
- Article -> LinkedIn post -> email digest -> short video script -> site snippet.

9. Post-publish observe/update (EOU):
- Record opens, CTR, comments, paid conversion proxy, coaching inquiry lift.

10. Banking workflow script:
- Only bank “research-to-article” script after repeat pass of proof checks and at least one wrapper swap.

## 9) Content-to-Revenue Conversion

1. Substack content CTA orchestration (course/app/coaching routes).
2. Segmented email follow-up from content interactions.
3. Webinar/live session conversion funnel (if used).
4. Content-triggered offer workflow (reader intent -> tailored offer draft).
5. Social proof harvesting (testimonials/case snippets) with approval gate.

## 10) Finance, Reporting, and Risk Control

1. Monthly revenue and expense close workflow.
2. Net contribution snapshot by product line/tier.
3. Refund and chargeback monitoring.
4. Tax reserve reminder workflow.
5. KPI dashboard refresh (small founder-friendly KPI set).
6. Forecast update loop against speculative model.
7. Finance Intents enforcement for all money-adjacent actions.

## 11) Platform Reliability and Security Operations

1. Webhook reliability checks (delivery, retries, idempotency failures).
2. Secrets hygiene and credential rotation reminders.
3. Role/permission drift audit.
4. Backup and restore drill workflow.
5. Incident response playbook execution (payments/access/outage).
6. Post-incident review with action tracking.
7. Policy/rule change control with audit trace.

## 12) Strategic Growth and Partnerships

1. Partner prospecting and qualification workflow.
2. Affiliate/referral onboarding and compliance checks.
3. Pilot programme intake and delivery tracking.
4. Case-study production workflow (metrics + evidence pack).
5. Segment expansion experiments (new niches/offers/channels).
6. Tier progression decisions (A -> B -> C readiness gates).

---

## Priority Rollout (Recommended)

### Wave 1 (Immediate Revenue + Reliability)
1. Checkout -> entitlement sync and reconciliation.
2. Coaching credit + booking + reminders.
3. Lead scoring + offer routing.
4. At-risk retention workflow.
5. Founder KPI weekly snapshot.

### Wave 2 (Authority + Conversion Engine)
1. Research/news-to-article workflow (with claim-risk gate).
2. Content repurposing pipeline across Substack/LinkedIn/email.
3. Benchmark delta reporting for customers.
4. Testimonial/referral workflow.

### Wave 3 (Scale + Sovereignty Readiness)
1. Banking of proven workflow scripts.
2. Partner pipeline automation.
3. Advanced portability checks and staged autonomy expansion.

---

## Core KPIs Per Workflow Class

- Acquisition: qualified leads/week, lead-to-checkout conversion.
- Fulfilment: checkout-to-entitlement success rate, time-to-access.
- Coaching: session utilization rate, no-show rate, completion rate.
- Outcomes: adherence rate, benchmark delta completion, milestone completion.
- Content: publish cadence, open rate, click-through, conversion lift.
- Risk: unsupported-claim publish blocks, incident count, rollback success.
- Ops: manual interruptions/day, automation failure rate, support hours/week.

---

## Governance Defaults

- Default mode for customer-facing outputs: `Draft-for-approval`.
- `Escalate` always for money, legal, strong scientific claims, or high uncertainty.
- No autonomous money movement; Finance Intents only.
- Promotion to Auto-run requires proof checks and portability evidence.