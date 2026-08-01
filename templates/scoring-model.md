# Vendor Scoring Model — Weighted Criteria and Thresholds

A scoring model exists to make vendor decisions consistent, explainable, and fast — not to fake precision. This one uses six weighted criteria scored 0–4, hard-fail gates that no score can override, and different pass thresholds per tier.

## Criteria and weights

| # | Criterion | Weight | What 4 looks like | What 0 looks like |
|---|---|---|---|---|
| C1 | **Independent assurance** | 25% | Current SOC 2 Type II / ISO 27001 covering the purchased service, clean or well-remediated exceptions, report actually reviewed by us | No attestation, no pentest, refuses evidence under NDA |
| C2 | **Data protection fit** | 25% | Encryption at rest/in transit, deletion on termination demonstrated, region control, DPA terms meet [dpa-checklist.md](dpa-checklist.md) | Unclear storage, no deletion story, DPA gaps on core points |
| C3 | **Access & integration security** | 20% | SSO on our plan, least-privilege scopes, scoped/short-lived API tokens, staff access logged & visible | No SSO, broad OAuth scopes only, opaque staff access |
| C4 | **Incident & notification posture** | 15% | Contractual ≤ 72h breach notification, named contact, credible IR evidence (they can describe their last incident) | Best-effort notification only, no IR answers |
| C5 | **Resilience & continuity** | 10% | SLA fits our availability tier, DR tested with dates, backup immutability | No SLA, no DR evidence, single-region with no story |
| C6 | **Vendor viability & exit** | 5% | Established or well-funded, standard export formats, realistic migration path | Pre-revenue + proprietary lock-in + our data hostage on exit |

**Score = Σ (criterion score / 4 × weight) × 100 → 0–100.**

Scoring discipline: score only from evidence (documents, configurations, contractual text, recorded answers). Marketing pages score 0. Note the evidence next to each criterion score — the note is what makes re-assessment at renewal a 30-minute job instead of a repeat.

## Hard-fail gates (auto-reject or escalate regardless of score)

1. No TLS on data in transit, or credible evidence of plaintext password storage.
2. Refuses any independent evidence (attestation, pentest letter) under NDA — Tier 1 only.
3. Will not sign a DPA while processing personal data on our behalf.
4. Breach-notification commitment absent or weaker than our downstream obligations to customers/regulators (we cannot promise 72h to a regulator while our processor promises "reasonable efforts").
5. Sanctions/legal exposure or ownership structure we cannot legally contract with.

Gate hits at Tier 1 can only be overridden by a written risk acceptance from the exec who owns the affected data, with a compensating control and an expiry — the same exception machinery as every other policy.

## Pass thresholds

| Tier | Approve | Approve with conditions | Reject |
|---|---|---|---|
| Tier 1 (critical) | ≥ 75 | 60–74 — conditions with owner + due date written into the register (and where possible the contract) | < 60 |
| Tier 2 (standard) | ≥ 60 | 45–59 | < 45 |
| Tier 3 (low) | No scoring — register entry only | — | Gate hits only |

**Conditions must be real:** "enable SSO within 60 days of contract," "provide SOC 2 by Q3 or we get termination rights," "restrict OAuth scope to read-only." A condition without a date and an owner is a wish; the register review (see [../docs/continuous-monitoring.md](../docs/continuous-monitoring.md)) checks conditions first.

## Worked example (Tier 1: customer-data analytics platform)

| Criterion | Evidence | Score | Weighted |
|---|---|---|---|
| C1 | SOC 2 Type II, 12-mo period, 1 exception (access review timeliness) remediated | 3 | 18.75 |
| C2 | AES-256/KMS, EU region pinning, deletion attested; DPA: subprocessor objection right missing | 3 | 18.75 |
| C3 | SAML on our plan; API tokens scoped but non-expiring | 3 | 15.0 |
| C4 | 72h contractual notification, named CSIRT contact | 4 | 15.0 |
| C5 | 99.9% SLA, DR test Feb 2026 | 4 | 10.0 |
| C6 | Series C, standard export (Parquet/CSV) | 3 | 3.75 |
| **Total** | | | **81.25 → Approve** |

Register note: "Approved 81/100. Watch item: request token expiry support; re-check DPA subprocessor clause at renewal."

## What this model deliberately ignores

- **Vendor size as a proxy for security.** Large vendors fail differently, not less; score evidence, not logos.
- **Questionnaire prose quality.** Beautifully written answers with no artifacts score like no answers.
- **Security-rating-service scores** (external scan grades). Usable as a monitoring *signal* (see continuous monitoring), too noisy as an approval input.
