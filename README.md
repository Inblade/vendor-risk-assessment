# Vendor Risk Assessment Toolkit

A tiered third-party risk toolkit sized for companies that have hundreds of SaaS vendors and a security team of approximately one. The design goal is triage: spend real effort on the ~10 vendors that can hurt you, a structured hour on the next ~40, and nearly zero on the long tail — while keeping an auditable trail for all three.

Reference templates distilled from practice; adapt thresholds and questions to your risk appetite and legal obligations. Not legal advice — DPA and contract items need counsel review.

**Author:** Danylo Kochetov — Senior DevOps/SRE, security architecture track.

## Structure

```
vendor-risk-assessment/
├── README.md
├── LICENSE
├── templates/
│   ├── vendor-questionnaire.md   # Three tiers: critical / standard / low-risk
│   ├── scoring-model.md          # Weighted criteria, pass thresholds, override rules
│   └── dpa-checklist.md          # Data-processing points to verify before signature
└── docs/
    └── continuous-monitoring.md  # Renewal cadence, breach-notification clauses, drift triggers
```

## The process in one diagram

```
New vendor request
   │
   ├─ Tiering (5 questions, 2 minutes) ──────────────► Tier 3 (low): register entry + AUP rules, done
   │
   ├─ Tier 2 (standard): short questionnaire ────────► Score (scoring-model.md) ─► approve / conditions / reject
   │
   └─ Tier 1 (critical): full questionnaire + SOC2/ISO review + DPA checklist + security-owner sign-off
                                                        │
                                                        ▼
                                        Register entry with renewal date
                                                        │
                                                        ▼
                              Continuous monitoring (docs/continuous-monitoring.md)
```

## Tiering rules (the 2-minute decision)

A vendor is **Tier 1 (critical)** if any of: processes customer personal data or Restricted-tier data; has production access or an OAuth grant to core systems (VCS, IdP, cloud, mailboxes); is in the availability path of the product; or would trigger contractual/regulatory duties if breached.
**Tier 2 (standard):** processes Internal/Confidential company data without reaching Tier 1 (analytics, support tooling, HR systems).
**Tier 3 (low):** no company data beyond names/emails of the buyer, no integrations (the design tool a team pays for by card).

Tier decides everything downstream: questionnaire depth, scoring rigor, DPA scrutiny, and review cadence.

## Opinionated defaults

- **Read their SOC 2 / ISO cert instead of sending 300 questions.** The questionnaire exists to cover what reports don't say (subprocessors, your specific data flows, notification commitments) — not to re-derive their control environment.
- **OAuth scopes are vendor risk.** A free tool with `repo` read on your VCS org is a Tier 1 vendor regardless of contract value.
- **Reject silently-renewing assessments.** Every approval carries an expiry; unowned vendors drift into the highest-risk category.

## License

MIT — see [LICENSE](LICENSE).
