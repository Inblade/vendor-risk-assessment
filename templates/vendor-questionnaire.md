# Vendor Security Questionnaire — Tiered

Three questionnaires, sized to vendor tier (see README for tiering rules). Principles: every question must be *decision-relevant* — if no answer would change your approval, delete the question; prefer verifiable artifacts over prose answers; accept the vendor's existing attestations in lieu of overlapping questions.

Header block for all tiers:

| | |
|---|---|
| Vendor / product | |
| Internal requester & business purpose | |
| Data shared (types + classification tier) | |
| Integration surface (OAuth scopes, API keys, SSO, network) | |
| Tier assigned / assessed by / date | |

---

## Tier 3 — Low-risk (no questionnaire)

No vendor contact. Internal checks only, recorded in the register:

1. Confirm tiering is right (the most common error is a "low-risk" tool that requests an OAuth grant — that's a re-tier).
2. Vendor is a real company with a published privacy policy and security page.
3. Payment via approved procurement path; owner named in the register.

---

## Tier 2 — Standard (12 questions, target: vendor answers in one sitting)

**Attestations & posture**
1. Do you hold a current SOC 2 Type II or ISO/IEC 27001 certificate? Provide the report/certificate (under NDA if needed). *If yes, questions 4–7 may be answered by reference to the report.*
2. Have you had a security breach requiring customer or regulator notification in the last 3 years? Describe scope and remediation.
3. Who is accountable for security in your organization (role, not name)?

**Product security**
4. Does your product support SSO (SAML/OIDC) for our users, and is it available on our plan? Is MFA enforced for your own staff?
5. How is customer data segregated from other tenants?
6. Is data encrypted in transit (TLS ≥ 1.2) and at rest? Who manages the keys?
7. Describe your vulnerability management: scanning, patch SLAs, and whether you run a pentest at least annually (share the latest summary/attestation letter).

**Data handling**
8. Where is our data stored and processed (regions)? Can region be constrained?
9. What is your data retention on termination, and how is deletion performed and confirmed?
10. List subprocessors that would touch our data.

**Operations**
11. What is your breach-notification commitment to customers (hours/days, contractual)?
12. What uptime SLA applies to our plan, and where is your status page?

**Pass expectations:** see [scoring-model.md](scoring-model.md). Hard expectations at this tier: TLS everywhere, SSO available, a real answer to Q9 and Q11.

---

## Tier 1 — Critical (Tier 2 questions plus the following; expect a call, not just a form)

**Assurance depth**
13. Provide the full SOC 2 Type II report (not the badge). We will review: opinion, period, exceptions, complementary user-entity controls (CUECs), and carve-outs. *Internally: record a 5-line review note — period covered, qualified?, exceptions relevant to our use, CUECs we must implement, subservice orgs.*
14. Scope check: does the attestation cover the product/service *we* are buying and the region we'll use? (Reports scoped to a different product line are a classic false comfort.)
15. Latest pentest: date, firm, scope, summary of criticals/highs and their remediation status.

**Access & integration (answer for our specific integration)**
16. Exactly which permissions/scopes does the integration require, and what is the least-privilege configuration you support? Do you support scoped/short-lived tokens or IP allowlisting for API access?
17. Which of your staff can access our data, under what controls (SSO, MFA, JIT, logging)? Is support access to customer data logged and visible to us?
18. Do you support customer-managed encryption keys or private connectivity (PrivateLink/PSC) for our data path?

**Resilience**
19. RTO/RPO for the service we're buying; date and result of your last DR test.
20. Describe your ransomware resilience for customer data (backup isolation/immutability).

**Incident & legal**
21. Contractual breach-notification window for confirmed incidents affecting our data — we require ≤ 72 hours to sign, ≤ 24–48 preferred (see [dpa-checklist.md](dpa-checklist.md)).
22. Will you notify us of new subprocessors in advance with an objection right?
23. Cyber insurance: carrier and coverage limit.
24. If we terminate: data export format, assisted-migration options, deletion attestation.

**Internal-only additions for Tier 1** (not sent to the vendor)
- Concentration risk: what breaks for us if this vendor is down for 5 days? Is there a credible substitute?
- Exit cost honestly estimated (lock-in is a risk score input, see scoring model).
- For vendors with OAuth grants to core systems: review the exact scopes against least privilege before approval, and record them in the register — scope creep at re-auth is a monitored event.

---

## Handling refusals and non-answers

- "We can't share our SOC 2" → accept under NDA; refusal with NDA offered is a scoring red flag at Tier 1.
- "Security questionnaire fee" or answers pointing solely at a marketing trust page → treat as non-answer.
- Startups without attestations can pass Tier 2 with strong answers + a committed audit date; they cannot pass Tier 1 for Restricted data without compensating controls (e.g. customer-managed keys, restricted scopes) and a documented risk acceptance by the data owner.
