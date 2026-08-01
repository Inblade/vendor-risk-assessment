# DPA Review Checklist — Data Processing Points to Verify

A Data Processing Agreement review checklist for the security/privacy reviewer working alongside counsel. Counsel owns enforceability; this checklist covers the operational substance — the clauses that determine what actually happens when data flows, subprocessors change, or a breach hits. Written with GDPR Article 28 as the baseline; adjust for other regimes (UK GDPR, CCPA/CPRA service-provider terms) as applicable.

For every item: record **present / absent / weaker-than-required**, and where in the document. "Weaker than our downstream obligations" is the killer category — a processor's promise must never be softer than the promise you've made to your own customers and regulators.

## 1. Scope and roles

- [ ] Roles stated correctly (they process on your documented instructions; watch for vendors quietly claiming independent-controller status for "service improvement" or analytics over your data).
- [ ] Processing scope: data categories, data-subject categories, purposes, duration — filled in, not left as blank annexes. Blank Annex 1 = unreviewable DPA.
- [ ] **Use of your data for AI/model training addressed explicitly.** Silence is no longer acceptable in 2026; require an opt-out or a no-training clause for Confidential-tier data.
- [ ] Instructions mechanism: product configuration counts as instructions (matters when you enable a feature that changes processing).

## 2. Subprocessors

- [ ] Current subprocessor list referenced and actually retrievable (URL or annex), captured into your vendor register at signature.
- [ ] **Advance notice of new subprocessors** (≥ 30 days preferred) **with an objection right**, and a meaningful remedy on objection (termination with pro-rata refund at minimum).
- [ ] Flow-down: subprocessors bound by equivalent obligations; vendor remains fully liable for them.
- [ ] Notification channel is one you'll actually receive (subscription/email — assign the mailbox; "we update this page" plus nobody watching the page is how subprocessor drift happens).

## 3. Security measures

- [ ] Technical and organizational measures (TOMs) annex is specific: encryption standards, access control, logging — not "industry-standard measures."
- [ ] TOMs can only be changed without degradation ("no material reduction" language).
- [ ] Personnel: confidentiality commitments, need-to-know access; support access to your data logged.
- [ ] Right to security information: current attestations (SOC 2/ISO) provided on request, annually.

## 4. Breach notification — the clause to fight for

- [ ] Notification of a personal-data breach **without undue delay and in any case within a fixed window — target ≤ 48h, sign at ≤ 72h** from the vendor becoming aware. Your GDPR clock (72h to the supervisory authority) starts when *you* become aware; a vendor promising "prompt" notification can consume your entire window.
- [ ] Definition covers *suspected* incidents affecting your data, not only confirmed ones.
- [ ] Content specified: nature, categories and approximate volumes, likely consequences, measures taken — the Article 33 ingredients you'll need for your own notification.
- [ ] Named security contact / channel on both sides; verify the channel exists (send a test message; it's not paranoia, it's ops).
- [ ] No gag: the DPA must not prevent you from meeting your own legal notification duties.

## 5. International transfers

- [ ] Storage and processing locations stated; region-pinning honored contractually if you rely on it.
- [ ] Transfer mechanism for ex-EEA/UK flows: adequacy (incl. EU–US DPF status where relied on), or SCCs incorporated with modules matching the actual roles; UK addendum where relevant.
- [ ] Transfer-impact considerations: vendor commitment to challenge/notify on government access requests where legally possible.
- [ ] Subprocessor locations included in the transfer analysis (the vendor may be EU; their support subprocessor may not be).

## 6. Data-subject rights and assistance

- [ ] Assistance with DSARs (access, deletion, portability) at no or reasonable defined cost, within timelines that leave you room for your own 30-day duty.
- [ ] Assistance with DPIAs and supervisory-authority consultations.
- [ ] Product reality check: does the product *actually* support per-subject deletion/export, or is the clause a promise their engineering can't keep? Ask for the mechanism, not the clause.

## 7. Termination and deletion

- [ ] Return and/or deletion of all personal data at termination, at your choice; **fixed deadline** (30–90 days) and **written deletion confirmation**.
- [ ] Backup-tail handling: deletion from backups within a stated cycle, with protection-until-deletion commitments.
- [ ] Export before deletion in a usable format — align with the exit answers from the questionnaire (Q24).

## 8. Audit and liability (review with counsel)

- [ ] Audit right: attestation reports as the default satisfaction, with a genuine audit right retained for cause (post-incident, regulator demand).
- [ ] Liability for data-protection breaches not capped at a trivial multiple; ideally carved out or super-capped relative to general contract caps. *This is a negotiation about money — bring counsel and the business owner, and know your walk-away.*

## Register integration

On signature, record in the vendor register: DPA date/version, breach-notification window, subprocessor-notification channel + owner, deletion deadline, transfer mechanism, and any weaker-than-required items accepted (each one is a register watch item with an owner, re-raised at renewal — see [../docs/continuous-monitoring.md](../docs/continuous-monitoring.md)).
