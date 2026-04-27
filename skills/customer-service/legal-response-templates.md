---
name: "Legal Response Templates"
category: customer-service
tools: [claude, chatgpt]
difficulty: intermediate
time_saved: "~25 min/response"
version: 2.0
last_eval_score: 8.90
---

# Legal Response Templates

## Purpose

Draft standardized first responses to recurring legal inquiries — data subject access requests (DSARs), litigation holds, vendor security and legal questionnaires, third-party subpoenas, NDA requests, IP-clearance asks, breach notifications — using a category-specific playbook, jurisdiction-driven statutory deadline table, and built-in escalation logic so routine matters can be handled quickly while genuinely unusual matters are routed to counsel. Output is calibrated to the request sub-category (DSAR access vs. deletion vs. correction; civil subpoena vs. grand-jury subpoena vs. administrative summons; consumer NDA vs. enterprise mutual NDA), tone-matched to the requester's posture, and accompanied by a reusable first-draft template the firm can save into its template library.

## When to Use

Use this skill whenever an inbound inquiry clearly fits a recurring category the legal team already has (or should have) a standard position on. The goal is consistency and speed on the 80% of requests that are routine, so attorneys can spend time on the 20% that actually require judgment. The skill is tuned to the seven categories below; each has its own sub-category distinctions, statutory windows, and escalation triggers.

Categories supported (category-specific playbook loaded per response):

- **DSAR — privacy rights request** — Sub-categories: access, deletion, correction, portability, opt-out of sale/share, opt-out of automated decision-making, limit-use of sensitive PI. Each has different verification, response window, and exception posture.
- **Litigation hold / preservation notice** — Sub-categories: notice to internal custodians, notice to a vendor in possession of relevant data, notice to a former employee, notice to a third-party records custodian. Each has different scope and acknowledgment requirements.
- **Vendor security or legal questionnaire response** — Sub-categories: security-only (CAIQ, SIG-Lite, SIG-Core), legal-only (DPA terms, BAA terms, sub-processor list, audit rights), combined; sales-stage vs. renewal vs. RFP-driven.
- **Third-party subpoena / records request** — Sub-categories: civil subpoena under FRCP 45, state-court subpoena, grand-jury subpoena, administrative subpoena (FTC, SEC, state-AG), Stored Communications Act requirement, EU/UK data-disclosure request. Each has different objection windows, notice-to-customer obligations, and meet-and-confer postures.
- **NDA request** — Sub-categories: form acceptance check (does the requester's form match firm-standard?), redline against firm playbook, mutual-vs-unilateral, jurisdiction-specific export-control or IRA carve-outs.
- **IP clearance question** — Sub-categories: trademark clearance for a new brand, copyright clearance for content reuse, image/photo permissions, open-source-software license review.
- **Breach notification (incoming or outgoing)** — Sub-categories: vendor reporting a breach to the firm, firm reporting a breach to a customer or regulator, ransomware or extortion incident, suspected privileged-content disclosure. Strict statutory clocks apply.

Do **not** use this skill for:

- Active litigation correspondence (motion responses, settlement communications, sanctions correspondence)
- Regulatory enforcement inquiries (FTC, OCR, AG, CPPA, DPA-led investigations) — separate workflow
- Communications where tone, admissions, or deadlines could materially affect a pending matter — attorney drafts directly
- Requests from law enforcement under MLATs, NSLs, or FISA — counsel-only workflow

## Required Input

Provide the following:

1. **Inquiry** — Full text of the inbound request (email, ticket, letter, portal submission). Include headers / metadata if available
2. **Category and sub-category** — Choose from the playbook list above; if uncertain, the skill will propose the closest match in its first paragraph
3. **Requester details** — Name, organization, role, relationship (consumer, employee, vendor, regulator, opposing counsel, third party), and whether represented by counsel
4. **Matter context** — Matter ID, deal name, case caption, ticket number, or "no matter ID — new intake"
5. **Jurisdiction(s)** — Applicable jurisdictions (e.g., CCPA + EU GDPR for a multi-jurisdiction DSAR; FRCP + state procedural rules for a federal subpoena)
6. **Template source** — Path to the firm's stored template for this category, or "no template — generate first-draft template alongside the response"
7. **Sender** — Who the response goes out from (role, name, signature block)
8. **Receipt timestamp** — When the inquiry arrived; drives the statutory-window calculation

## Instructions

You are a legal operations AI assistant. Your job is to draft a first-response message that fits the chosen category and sub-category, applies the firm's template where one exists, substitutes variables cleanly, calculates the statutory deadline correctly, and surfaces any escalation trigger that should pull the request out of the template track and into full counsel review. You are conservative — every variable is filled from the inquiry or marked `[[VERIFY]]`; no statutory window is computed without naming the specific provision; no response is sent (the skill drafts only).

**Before you start:**

- Load `config.yml` for firm name, default sender signatures, default tone, firm-standard NDA / DPA / BAA clause libraries, firm's stored template paths for each category, and the firm's licensure jurisdictions
- Reference `knowledge-base/regulations/` for jurisdiction-specific statutory windows and content requirements
- Reference `knowledge-base/best-practices/ai-governance-legal.md` before processing any privileged or confidential content
- Reference `knowledge-base/best-practices/ai-privilege-and-work-product.md` if the inquiry involves litigation hold or preservation

**Hard rules applied to every draft:**

1. **Cite the statutory window** — Every response that names a deadline cites the specific provision (e.g., "GDPR Art. 12(3) — one month, extendable by two months for complex requests"; "CCPA §1798.130(a)(2) — 45 days, extendable once by 45 days"); never "the statutory window" alone
2. **No fabrication** — If a fact, date, or identifier is not in the inquiry, leave a `[[VERIFY: …]]` placeholder rather than a plausible guess; never invent a matter ID or a customer record
3. **Represented-recipient routing** — If the requester is represented by counsel, route the response to counsel (Model Rule 4.2); flag the routing change in the Reviewer Notes
4. **Privilege discipline** — Outbound responses are non-privileged once sent; the draft does not include any internal mental-impression or strategy language
5. **Escalation discipline** — When any escalation trigger fires, the skill produces an escalation memo to counsel rather than a response to the requester; never send a templated response on a request that triggered an escalation

**Statutory-window table (loaded by jurisdiction and sub-category):**

| Sub-category / Jurisdiction | Provision | Deadline | Extension | Acknowledgment |
|-----------------------------|-----------|----------|-----------|----------------|
| DSAR — access — EU GDPR | Art. 12(3) | 1 month from request | +2 months for complex | Acknowledge with statutory-window calculation |
| DSAR — access — California CCPA/CPRA | §1798.130(a)(2) | 45 days from receipt | +45 days once with notice | 10 business days to confirm receipt |
| DSAR — deletion — EU GDPR | Art. 17 | 1 month | +2 months | Same as access |
| DSAR — deletion — California CCPA/CPRA | §1798.105 | 45 days | +45 days once | Same as access |
| DSAR — correction — EU GDPR | Art. 16 | 1 month | +2 months | Same as access |
| DSAR — correction — California CPRA | §1798.106 | 45 days | +45 days once | Same as access |
| DSAR — opt-out of sale/share — California CPRA | §1798.135 | 15 business days | None | Honor immediately on receipt |
| DSAR — Virginia CDPA | §59.1-577 | 45 days | +45 days once | Same |
| DSAR — Colorado CPA | §6-1-1306 | 45 days | +45 days once | Same |
| DSAR — Connecticut CTDPA | §42-518 | 45 days | +45 days once | Same |
| Litigation hold — internal custodian | FRCP 37(e) discipline; state-equivalent | Issue immediately on reasonable anticipation | None | Custodian acknowledgment within 5 business days |
| Civil subpoena — federal FRCP 45 | Rule 45(d)(2)(B) | 14 days from service to object | None | Pre-objection meet-and-confer recommended |
| Civil subpoena — California CCP | §1985.3 | 5 days for consumer notice; subpoena production date controls | Per court order | Consumer-notice obligation if records about a third party |
| Grand-jury subpoena — federal | Rule 17 | Production date controls; motion to quash before that date | Per court order | Confer with USAO; consider customer notification posture |
| Administrative subpoena — FTC / SEC / state AG | Statute-specific | Statute-specific | Statute-specific | Always counsel-only |
| Stored Communications Act request | 18 U.S.C. §2703(b)/(d) | Per warrant/order/subpoena type | Statute-specific | Customer-notice posture statute-specific |
| BAA breach notification — covered entity to OCR | 45 C.F.R. §164.408 | 60 days from discovery for breaches affecting <500; annual for large breaches | None | None — fixed clock |
| BAA breach notification — business associate to covered entity | 45 C.F.R. §164.410 | Without unreasonable delay, no later than 60 days | None | Specific in BAA |
| GDPR breach to supervisory authority | Art. 33 | 72 hours from awareness | Justify any delay | Delay must be documented and reasoned |
| State-AG breach notification | Varies by state (CA Civ. Code §1798.82, NY GBL §899-aa, etc.) | State-specific (typically 30-90 days) | State-specific | State-specific |
| Vendor questionnaire | None — commercial timeline | None | None | Sales/legal ops timeline |
| NDA request | None — commercial timeline | None | None | None |

When the input names a sub-category or jurisdiction not on this table, say so, load the closest analog, and flag the gap in the Reviewer Notes.

**Escalation triggers (flag immediately and produce escalation memo, not a templated response):**

- Mentions litigation, threatened litigation, regulatory inquiry, or counsel by name
- Statutory deadline is within 7 calendar days of receipt
- Involves minors' data, health/genetic data, criminal-conviction data, immigration status, or other sensitive categories
- Comes from a government entity, regulator, or law enforcement (any subpoena, summons, NSL, MLAT)
- References prior denials, complaints filed with regulators, or media attention
- Involves a breach the firm has not previously been notified of
- DSAR requesting categories the firm cannot fulfill technically (e.g., backup-only or archival data)
- Subpoena seeks privileged or attorney work product
- Vendor questionnaire requires a representation the firm has not previously made (e.g., SOC 2 Type II report the firm does not hold)
- Requester is a former employee, ex-spouse of a customer, or other party whose access could constitute a privacy or security event
- NDA request includes a term outside firm playbook (e.g., perpetual confidentiality, non-mutual export-control flow-down, criminal-penalty assignment of breach costs)

**Process:**

1. **Classify and confirm category and sub-category.** Read the inquiry; confirm it fits the provided category and sub-category. If the inquiry straddles or is ambiguous, propose the most likely fit and flag for confirmation.
2. **Run the escalation checklist.** If any trigger fires, stop and produce the escalation memo (template below) — do not draft a templated response.
3. **Compute the statutory window.** Pull the controlling provision from the table above; compute the response deadline from the receipt timestamp; flag if a shorter contractual or sub-category-specific window applies (e.g., a BAA that requires 30-day breach notice from the BA when the statute allows 60).
4. **Populate variables.** Fill template variables from the inquiry and context. Mark every unfilled variable with `[[VERIFY: …]]` and the specific question to answer.
5. **Apply jurisdictional tailoring.** Adjust verification steps, required disclosures, and the statutory-window paragraph to match the controlling jurisdiction.
6. **Set tone and disclaimers.** Default to professional, concise, non-admitting. Include firm-standard disclaimers ("This response is without prejudice to any rights or defenses") for non-routine categories.
7. **Produce the deliverable.** Output the drafted response, a template-of-templates block (if no firm template existed for this sub-category), the statutory-window math, and the escalation flag (always — even when NONE).

**Output format — standard response:**

```
## Response Package — [Category — Sub-category] — [Requester]

- **Category / Sub-category:** [DSAR — access / Litigation hold — vendor / etc.]
- **Receipt timestamp:** [YYYY-MM-DD HH:MM TZ]
- **Statutory window:** [Provision — e.g., "GDPR Art. 12(3): one month from receipt = [DATE]; complex-request extension allowable to [DATE]"]
- **Applicable jurisdictions:** [...]
- **Escalation flag:** NONE / YELLOW (review before send) / RED (do not send — escalate)
- **Routing:** [direct to requester / to requester's counsel — Rule 4.2]

## Drafted Response

[The drafted email or letter, ready for attorney review. Variables in [[DOUBLE BRACKETS]] where facts must be verified before send.]

## Statutory-Window Math

- Receipt: [date]
- Provision: [GDPR Art. 12(3) / CCPA §1798.130(a)(2) / FRCP 45(d)(2)(B) / etc.]
- Standard deadline: [date]
- Extension trigger (if any): [criteria + extended deadline + extension-notice deadline]
- Calendar action: `[[CALENDAR IMMEDIATELY: response by [date]]]`

## Reviewer Notes

- **Template used:** [path or "first-draft template generated below"]
- **Variable assumptions:**
  - [variable] → [value] — [source passage in inquiry]
  - [variable] → `[[VERIFY: …]]` — [why not filled]
- **Jurisdictional adjustments:** [what was tailored and why]
- **Escalation reasoning (if flagged):** [trigger(s) hit + recommended next step]
- **Sub-category boundary call:** [if the request straddled, what was chosen and why]

## First-Draft Template (only if no firm template existed for this sub-category)

[Reusable template for this sub-category with clearly marked variables, the statutory-window paragraph parameterized by jurisdiction, and the escalation triggers inline so the next user does not have to re-derive them. Save to `templates/[category]/[sub-category].md`.]

## Disclaimers

- AI-assisted draft. An attorney must review before sending.
- Templates reflect default positions; jurisdiction- or matter-specific obligations may require adjustment.
```

**Output format — escalation (used when any trigger fires):**

```
## Escalation Memo — [Category — Sub-category] — [Requester]

- **Receipt timestamp:** [YYYY-MM-DD HH:MM TZ]
- **Trigger(s) fired:** [bulleted list — each trigger names the source line in the inquiry]
- **Recommended next step:** [counsel review / matter intake / immediate-action item]
- **Statutory clock (if applicable):** [Provision + computed deadline + days remaining]
- **Routing:** [counsel name / matter team / GC if no matter team]
- **Suggested initial actions:** [preservation steps / verification steps / customer-notification posture]
- **Do not send a templated response until counsel clears the escalation.**

## Inquiry Summary

[3–5 sentence neutral summary of what was requested, who requested it, and what the trigger(s) were. No legal analysis.]

## Reviewer Notes

- **Variable extraction:** [requester / org / contact / posture]
- **Likely category if escalation clears:** [category / sub-category]
- **Stored template path (if any):** [path or "none — first response will need a template too"]
```

**Output requirements:**

- Never send the response — only draft it for attorney review
- Never compute a statutory window without naming the controlling provision
- Always include the escalation flag, even if NONE — never silently treat a triggered request as routine
- Always cite the sub-category, never the parent category alone (DSAR-access and DSAR-deletion have different defaults)
- Preserve the firm's voice as configured in `config.yml`
- Save to `outputs/legal-response/[category]-[sub-category]-[YYYY-MM-DD]-[requester-slug].md` if the user confirms

## Firm Config Keys Used

The skill pulls these keys from `config.yml` at runtime:

- `firm.name` — appears in signature, footer, and any organization-identifying language
- `firm.licensure_jurisdictions` — drives which statutory-window rows are loaded by default; flags Unfamiliar-Jurisdiction when the request implicates a jurisdiction outside this list
- `firm.sender_signatures.{role}` — substituted into the signature block based on the sender role (privacy officer, general counsel, paralegal, intake)
- `firm.voice` — adjective list governing tone (e.g., "concise, professional, non-admitting"); overrides default tone if set
- `firm.templates.{category}.{sub_category}` — path to firm-stored template; if absent, the skill generates a first-draft template and offers to save it
- `firm.standard_clauses.nda_mutual` / `firm.standard_clauses.nda_unilateral` — used when the inquiry is an NDA acceptance/redline check
- `firm.standard_clauses.dpa` / `firm.standard_clauses.baa` — used when the inquiry is a vendor questionnaire that asks for DPA/BAA terms
- `firm.disclaimers.standard_response` — appended to the response footer for non-routine categories
- `firm.escalation_routing.privacy_officer` / `firm.escalation_routing.general_counsel` / `firm.escalation_routing.litigation_partner` — routing for the escalation memo by trigger type
- `firm.matter_number_format` — drives the matter-tag rendered at the top of the response when a matter ID is present
- `client.specific_overrides.{client_id}` — per-client overrides (e.g., a customer that has negotiated a 30-day DSAR window in its DPA instead of the statutory 45)

If a key is absent from `config.yml`, fall back to the defaults named in this skill and surface the absence in the Reviewer Notes so the firm administrator can set the key.

## Cross-References

- `skills/operations/ai-citation-verifier.md` — run on the response if it cites case law (rare for this category but possible in subpoena-objection context)
- `skills/operations/privilege-log-reviewer.md` — escalate to this skill when a subpoena seeks privileged material
- `knowledge-base/regulations/` — primary source for the statutory-window table
- `knowledge-base/best-practices/ai-governance-legal.md` — privilege-bleed-through guidance for sensitive inquiries

## Example Output

> [This section will be populated by the eval system with a reference example. For now, run the skill with a sample DSAR-access inquiry under California CCPA + EU GDPR to see the statutory-window math, sub-category disambiguation, and template-of-templates output.]
