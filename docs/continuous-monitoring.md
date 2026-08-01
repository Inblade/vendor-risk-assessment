# Continuous Vendor Monitoring — Cadence, Triggers, and Clauses That Matter Later

Point-in-time vendor assessment answers "should we sign?" Continuous monitoring answers the question that actually causes incidents: "is the assessment still true?" Vendors drift — they get acquired, add subprocessors, ship AI features trained on customer data, weaken plans, get breached. This document defines a monitoring regime an approximately-one-person security function can sustain.

## The register is the instrument

Everything monitors *against the register entry*. Minimum columns for it to work:

`Vendor · Tier · Owner (internal) · Data shared · Integration scopes · Assessment score + date · Conditions (open/closed) · Attestation expiry · DPA: notification window / deletion deadline / subprocessor channel · Renewal date · Watch items`

If the register lives in the compliance platform, fine; a well-kept sheet also works. What doesn't work is the register nobody owns — assign a named owner per vendor (the internal business owner, not security; security owns the *process*).

## Scheduled cadence

| Activity | Tier 1 | Tier 2 | Tier 3 |
|---|---|---|---|
| Full re-assessment (re-score against current evidence) | Annual | Every 2 years or at renewal | Never (tier check only) |
| Attestation refresh (new SOC 2 period / ISO cert) + 5-line review note | Annual — chase 60 days before expiry | At re-assessment | — |
| Condition & watch-item check | Quarterly | At re-assessment | — |
| OAuth/API scope review (compare live grants vs register) | Quarterly, automated where the platform exposes grant APIs | Annual | — |
| Register hygiene sweep (orphaned vendors, departed owners, unused tools) | Quarterly, org-wide | | |
| Tier revalidation ("does this vendor still touch what we recorded?") | At renewal | At renewal | Annual batch |

**Effort budget honesty:** with 10 Tier 1 and 40 Tier 2 vendors, this cadence costs roughly 2–3 days per quarter. If your list makes it cost more, your tiering is too generous — demote vendors, don't skip cycles.

## Event-driven triggers (re-assess out of cycle)

- **Vendor breach or security incident disclosure** — run the playbook below.
- **Acquisition or major funding/ownership change** — data-handling posture and subprocessor stack often change within two quarters.
- **New subprocessor notice** — check against the transfer analysis and objection window; this is why the DPA notification channel needed a named mailbox owner.
- **Material product change**: new AI features processing your data, new data-residency options (or their removal), authentication changes.
- **Scope creep on integration**: the app re-requests broader OAuth scopes — treat as a new assessment of C3, not a click-through.
- **Attestation lapse**: SOC 2 period ends with no successor report; a coverage gap is information.
- **Our side changes**: we start sending a new data category to an existing vendor — the most commonly missed trigger, because it looks like a product decision, not a vendor decision. Countermeasure: data-flow changes to Tier 1 vendors go through design review.
- **External signals**: security-rating-service alerts and vendor status/incident pages — useful as tripwires, never as verdicts.

## Renewal as the enforcement point

Renewal is the only moment you have leverage without escalation, so the renewal workflow front-loads the accountability:

1. 90 days out: owner confirms the vendor is still needed (usage data beats opinions — unused seats and stale integrations are both spend and attack surface).
2. 60 days out: security checks open conditions and watch items. **Unmet conditions from last cycle are renewal blockers by default** — the vendor promised SSO "within 60 days" 300 days ago; now is when that promise has teeth.
3. Re-assessment (per cadence table) with the previous evidence notes as the baseline — score the deltas, not the whole vendor from scratch.
4. Contract check: attestation-refresh obligation, breach-notification window, deletion terms survive the renewal paperwork (auto-renewals have a way of dropping negotiated DPA riders — diff the documents).

## Vendor breach playbook (have it before you need it)

1. **Clock check:** note when the vendor's notice arrived vs their contractual window; when *we* became aware starts our own regulatory/customer clocks — hand to legal immediately if personal data is in scope.
2. **Scope from the register**, not from memory: what data, what scopes, what integrations. (This is the moment the register earns its keep.)
3. **Contain our side:** rotate shared credentials/API keys, revoke OAuth grants if credible compromise of the vendor's platform, raise monitoring on data the vendor held.
4. **Demand the Article-33 ingredients** the DPA promised: categories, volumes, consequences, measures.
5. **Decide:** continue / conditions / emergency exit. Run the exit answers from questionnaire Q24 — this is why exit was assessed at onboarding, when it was hypothetical.
6. **Record** the incident in your own incident process (it's a security incident of yours, category "third party") and as a register event; it feeds the vendor's next score.

## Offboarding — the forgotten half

Vendor risk ends only when the data does: on termination, trigger the DPA deletion clause, calendar the deadline, obtain the written deletion confirmation, revoke SSO/OAuth/API access and network allowances, and close the register entry with a date. An offboarded vendor with a live OAuth grant is the third-party version of the un-offboarded employee — and it appears in real incident write-ups just as often.
