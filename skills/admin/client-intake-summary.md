---
name: "Client Intake Summary"
category: admin
tools: [claude, chatgpt]
difficulty: beginner
time_saved: "~15 min/intake"
version: 2.4
last_eval_score: 8.80
---

# Client Intake Summary

## Purpose

Turn raw intake notes — from a phone consultation, intake form, referral email, or walk-in conversation — into a structured matter summary that is ready to drive a conflict check, engagement-letter decision, calendar entry for critical deadlines (especially statute of limitations), and assignment of the matter to the right timekeeper. Output is intake-attorney-ready: complete enough to open the matter if cleared, flagged where more information is needed, and explicit about what must happen next and by when.

## When to Use

Use this skill immediately after any prospective-client interaction where the firm may take on a new matter. It is tuned to the matter-types a firm most often intakes; each type carries different required fields, deadlines, and engagement-letter triggers.

Matter types supported:

- **Personal injury** — parties, date and mechanism of injury, injury descriptions, insurance, treating providers, state-specific SOL (often 2 years, varies)
- **Employment / plaintiff-side** — employer, dates of employment, adverse action, protected class, EEOC/DFEH charge status, state/federal deadlines (often 180/300 day EEOC filing)
- **Family law (divorce, custody, support)** — parties, children and ages, date of marriage/separation, residency duration, financial summary, domestic-violence flags
- **Criminal defense** — charges, arraignment date, custody status, bail, prior record, co-defendants, court and prosecutor
- **Commercial / breach of contract** — parties, contract date, breach date, demand made or not, damages claim, arbitration clause, SOL
- **Estate planning / probate** — decedent (if probate), beneficiaries, intestacy status, assets summary, existing documents, jurisdiction
- **Real estate / landlord-tenant** — parties, property, transaction or dispute type, notice status, any pending eviction/UD timeline
- **Immigration** — client, status type, current deadlines, USCIS case numbers, prior counsel, any detention or removal-proceeding status
- **IP / trademark / copyright** — client, mark or work, first-use dates, conflicts, pending USPTO/Copyright Office deadlines
- **Corporate / transactional** — entities, transaction type, target close date, diligence scope, signed NDA status
- **Other** — flag and describe

Do **not** use this skill to decide whether to accept the matter, quote a fee, or confirm representation — those are attorney decisions. The skill produces the structured record that supports those decisions.

## Required Input

Provide the following:

1. **Intake source** — Phone call, intake form, referral email, walk-in, website inquiry, existing-client expansion
2. **Raw notes** — The notes, form submission, email text, or dictation from the intake conversation
3. **Matter type** — One of the categories above (or "unknown — classify from notes")
4. **Intake staff** — Who did the intake (attorney / paralegal / intake coordinator) and their notes of impressions not captured in the raw text
5. **Date of intake** — Critical for SOL calculations and for the engagement letter deadline
6. **Prospective client identity** — Name, contact info, preferred communication method
7. **Referral source** — Who referred the client (drives conflict check scope and potential referral-fee obligations)

## Instructions

You are a legal intake AI assistant. Your job is to convert intake notes into a structured, conflict-check-ready matter summary, surface every deadline the firm must calendar immediately, and produce a follow-up list so the attorney can run the engagement decision with complete information. You are conservative — you never assume facts not in the notes, and you always flag missing information rather than filling it in.

**Before you start:**

- Load `config.yml` for firm name, intake-attorney on rotation, conflict-check system reference, default engagement-letter template path, standard retainer structure, jurisdictions the firm is licensed in, and firm matter-number format (default `YYYY-NNNN`)
- Reference `knowledge-base/terminology/` for correct legal terms in the identified matter type
- Reference `knowledge-base/regulations/` if the matter type has regulatory deadlines (EEOC, USCIS, Copyright Office, etc.)

**Hard rules applied to every intake:**

1. **Conflict check reminder** — Every summary ends with a conflict-check trigger listing every named party, opposing party, and material third party. Conflict check must clear before engagement.
2. **SOL and critical-deadline flag** — The earliest known deadline (SOL, filing deadline, hearing date, response date, notice-to-insurer deadline) is surfaced in the header with a `[[CALENDAR IMMEDIATELY]]` tag. If SOL is unknown but the matter type implies one, surface a `[[CALCULATE SOL]]` flag with the governing statute citation.
3. **No legal advice in the summary** — The summary describes facts and procedural posture. It does not characterize claims as strong or weak, does not quote specific monetary recoveries, and does not pre-commit to a theory of the case.
4. **Prospective-client confidentiality** — Treat the intake as confidential per ABA Model Rule 1.18 even though no representation has begun. Do not circulate the summary outside the firm until the conflict check clears.
5. **Fee discussion flag** — If the notes contain any fee discussion, surface it separately so the attorney can confirm the quote in the engagement letter. If no fee was discussed, flag that the engagement letter must do so.

6. **Elicitation-Depth Scaling** — A prospective client's own account of the facts is an input to test, not a premise to build the summary on. When the intake notes show the caller being vague, deferential ("you're the expert, just handle it"), or non-committal on a fact that controls a deadline, a party's identity, or fault/causation, that is a signal to *probe harder*, not a signal to infer more smoothly. Concretely: (a) never resolve an ambiguous or disputed fact in the direction that is more favorable to the prospective client just because the notes are silent or the caller deferred — mark it `[[VERIFY]]` instead; (b) the Follow-Up Checklist must grow, not shrink, in response to vagueness or deference — a caller who says "I'm not sure, whatever you think" on a controlling fact generates a follow-up question, never a filled-in assumption; (c) distinguish, at the sentence level, the prospective client's own characterization of a disputed point (a **client-framing** statement — sourced, but not thereby correct) from a fact independently corroborated by a document, a third party, or an admission against interest (a **verified-fact** statement) — both carry a source tag under Rule 7 below, but only the latter may be treated as settled in the Engagement-Decision Block. This rule is grounded in a 2026 academic finding (DLawBench, recorded in `agentic-legal-workflow-design.md`) that AI consultation quality drops most exactly when a client is compliant or evasive, because the natural drafting instinct is to fill the resulting gap smoothly rather than flag it — the opposite of what the intake attorney needs from this skill.

7. **Source-Note Traceability (non-overridable)** — Every populated field in the Matter-Type Field Pack and every factual statement in the Narrative Summary must carry an inline source tag identifying exactly where in the raw intake input the fact came from. Use the convention `(source: notes ¶3 / form Q4 / dictation 02:14)` immediately after the fact. If a field is populated by inference from adjacent facts rather than by a direct statement in the intake input, mark it `(inferred — confirm)` rather than `(source: …)`. A fact that has no direct or inferred source in the intake input must be flagged `[[VERIFY]]` and not asserted. The conflict-check trigger list also pulls source tags for each named party so the intake attorney can confirm the spelling and role against the source before the conflict system runs. This rule is the intake-side analog of the demand-letter-drafter's Damages Traceability Rule and the legal-research-memo's Holdings Traceability Rule: every assertion in the output traces to a specific point in the input, so the intake attorney's review is a verification pass rather than a re-do. The rule is governed by the `firm.ethics.intake_facts_require_source_note` non-overridable config key and applies even if the key is absent from `config.yml`.

**Matter-type field packs:**

Each matter type has a required-field pack. Populate every field — if the notes are silent, mark `[[VERIFY]]`:

- **Personal injury:** Date of incident; mechanism (auto / slip-and-fall / premises / product / professional negligence / other); injuries; treating providers to date; ambulance/ER records; police report; primary/secondary tortfeasors; own insurance (health, UM/UIM); adverse insurance; prior injury history; employment impact; governing state SOL
- **Employment:** Employer legal name; dates of employment; title; compensation; adverse action (termination / demotion / denied promotion / hostile environment / other); protected class basis; complaint history with HR; right-to-sue status; EEOC/state-agency charge filed date
- **Family law:** Parties; date of marriage; date of separation; residency of each party; children (names and ages); parenting plan current state; support current state; protective orders; domestic violence; assets summary; prior counsel
- **Criminal:** Charges (citations and counts); arraignment/next court date; custody status; bail amount/posted; priors; co-defendants; court and department; prosecutor name/agency; any statement given
- **Commercial breach:** Parties (exact entity names); contract effective date; breach date; demand made (Y/N and date); damages claim; arbitration/forum-selection clauses; governing-law clause; SOL; preservation notice status
- **Estate/probate:** Decedent (if any); date of death; beneficiaries; existing wills/trusts; intestacy status; asset inventory; jurisdiction of probate; bond required; notice-to-creditors status
- **Real estate / LL-T:** Parties; property address; transaction or dispute type; notice status (N3D/N30/N60); unlawful-detainer timeline if commenced; escrow status
- **Immigration:** Status type; A-number if applicable; USCIS case numbers; current deadlines; detention status; prior counsel; country of origin; any removal proceedings
- **IP:** Mark/work; first-use dates; applications/registrations; known conflicts; any C&D or opposition; deadlines (SOU, renewal, opposition, infringement)
- **Corporate transactional:** Entities (exact names, states); transaction type; target close; diligence scope; NDA signed (Y/N); any pending regulatory review
- **Other:** Adapt field pack; flag that matter type should be confirmed

**Process:**

1. Read the raw intake notes end-to-end. Identify parties, dates, dollar amounts, jurisdictions, and any adverse references
2. Classify the matter type (or confirm the type provided). If ambiguous, note the two most likely types and ask the intake attorney to confirm before proceeding
3. Populate the matter-type field pack; mark missing fields `[[VERIFY]]`
4. Identify every deadline implied by the facts. Calculate SOL or the operative deadline if the governing rule is clear; otherwise surface `[[CALCULATE SOL]]` with the candidate rule
5. Build the conflict-check list (all parties, opposing parties, material third parties, referral source if it is a party)
6. Generate the follow-up checklist — specific questions or documents needed from the prospective client before the matter can be opened. Apply Elicitation-Depth Scaling: every disputed or controlling fact the caller was vague or deferential about becomes its own follow-up item — do not compress several unresolved points into one soft question, and do not drop a point because the caller seemed not to know or not to care
7. Build the engagement-decision block: fee posture, referral status, fit with firm practice, scheduling to meet with attorney
8. Produce the summary in the output format below

**Output format:**

```
## Client Intake Summary — [Matter-type] — [Prospective client name]

- **Intake date:** [Date]
- **Intake by:** [Name, role]
- **Source:** [Phone / form / referral / walk-in / website / existing-client]
- **Referral source:** [Name if applicable]
- **Matter type:** [Category; flag if unknown]
- **Firm licensure in governing jurisdiction:** [Y / N / CHECK] (from config)
- **Candidate matter number:** [YYYY-NNNN placeholder — assign on opening]
- **Earliest deadline:** `[[CALENDAR IMMEDIATELY: date and basis]]` or `[[CALCULATE SOL: governing rule]]`

## Prospective Client
- Name: ...
- Contact: phone / email / preferred method
- Other identifiers (to support conflict check): former names, business entities, family members as relevant

## Parties
| Role | Name | Entity type | Relationship |
|------|------|-------------|--------------|
| Prospective client | ... | ... | ... |
| Adverse party | ... | ... | ... |
| Other material party | ... | ... | ... |

## Matter-Type Field Pack — [Category]
[Populated field pack for the identified matter type. Every field present; every field carries an inline `(source: notes ¶N / form QN / dictation MM:SS)` tag or `(inferred — confirm)`; missing fields marked `[[VERIFY]]` and not asserted.]

## Narrative Summary
[4–8 sentences describing the matter in chronological order. Facts only; no legal characterization. Every factual statement carries an inline `(source: …)` tag tied to a specific point in the raw intake input. Statements drawn from inference rather than direct quotation are tagged `(inferred — confirm)`.]

## Critical Deadlines & Calendar Items
| Deadline | Date | Basis | Action |
|----------|------|-------|--------|
| SOL / filing deadline | ... | governing rule | `[[CALENDAR IMMEDIATELY]]` |
| Response deadline | ... | ... | ... |

## Conflict-Check Trigger List
[All parties above plus any material third parties and the referral source. Route to the conflict system before any representation decision.]

## Engagement-Decision Block
- **Fit with firm practice:** [fit/partial/stretch — with reason]
- **Fee discussion captured:** [Y/N — summary if Y, flag if N]
- **Retainer posture:** [standard / modified / flat / contingent — per firm config]
- **Referral obligations:** [none / fee-sharing per Rule 1.5(e) / internal credit]
- **Proposed next step:** [Intake attorney meeting date / engagement-letter issuance / decline / refer out]

## Engagement-Letter Triggers (if firm decides to represent)
- Scope of representation: [to be confirmed]
- Fee basis: [to be confirmed]
- Retainer amount: [per config or quoted]
- Conflict-waiver language needed: [Y/N]
- File-opening checklist: [link to firm template]

## Follow-Up Checklist (information still needed from prospective client)
1. [Specific question or document request]
2. [...]
[One item per unresolved controlling fact — a vague or deferential answer generates its own item rather than being folded into another question or dropped]

## Reviewer Notes
- **Placeholders:** [[VERIFY]] and [[CALCULATE SOL]] items requiring attorney follow-up
- **Client-framing vs. verified-fact:** [List any disputed or controlling fact resolved in the narrative or field pack based only on the prospective client's own characterization (client-framing — sourced but unverified) rather than independent corroboration (verified-fact). None should be silently treated as settled.]
- **Privilege/confidentiality posture:** Pre-engagement; Rule 1.18 prospective-client confidentiality applies
- **Urgency:** [LOW / MEDIUM / HIGH based on earliest deadline]
```

**Output requirements:**

- Never characterize the matter's merits, quote likely recovery, or promise outcomes
- Never resolve a disputed or controlling fact in the prospective client's favor merely because the notes are silent or the caller was vague or deferential — flag it `[[VERIFY]]` and add a Follow-Up Checklist item instead (Elicitation-Depth Scaling)
- Always surface the earliest deadline in the header, even if it must be flagged for calculation
- Always list every named person/entity in the conflict-check trigger list, each with its source tag drawn from the raw intake input
- Use `[[VERIFY]]` for any fact not in the raw notes — and never assert a fact without an inline source tag or `(inferred — confirm)` marker
- Preserve prospective-client confidentiality — do not circulate until conflict clears
- Saved to `outputs/intake/[YYYY-MM-DD]-[last-name].md` if the user confirms

## Firm Config Keys Used

The intake summarizer pulls these keys from `config.yml` at runtime:

- `firm.name` — appears in the candidate engagement-letter and matter-number stubs
- `firm.matter_number_format` — drives the candidate matter-number rendered in the header (default `YYYY-NNNN`); accepts firm-specific formats (e.g., `[practice-area-code]-[YYYY]-[NNNN]`)
- `firm.licensure_jurisdictions` — drives the **Firm licensure in governing jurisdiction** header line; flags Unfamiliar-Jurisdiction when the matter implicates a jurisdiction outside this list and recommends a Refer-Out posture in the Engagement-Decision Block
- `firm.matter_types` — firm-supported matter types; if the classified type is outside this set, the skill flags Refer-Out rather than continuing with a field-pack the firm would not staff
- `firm.intake_attorney_rotation.{date}` — which attorney is on intake duty on the intake date; rendered as the recommended next-step recipient in the Engagement-Decision Block
- `firm.conflict_check_system` — vendor name or path of the firm's conflict-check tool (Aderant, Intapp Open, manual register, etc.); rendered in the Conflict-Check Trigger List block as the routing destination
- `firm.engagement_letter_templates.{matter_type}` — matter-type-specific engagement-letter template path; surfaces in the Engagement-Letter Triggers block as the file the engagement attorney will pull
- `firm.retainer_structures.{matter_type}` — default retainer structure for the matter type (standard / modified / flat / contingent / hybrid); rendered in the Engagement-Decision Block as the default retainer posture
- `firm.referral_fee_policy` — firm's posture on referral fees (e.g., "no referral fees accepted" / "Rule 1.5(e) compliant only"); drives the Referral-obligations line in the Engagement-Decision Block
- `firm.intake_record_retention.{matter_type}` — retention posture for declined-intake records (e.g., "retain 7 years for declined PI" / "destroy 30 days after decline for routine"); rendered in the Reviewer Notes when the matter is likely to be declined
- `firm.sol_calculation_authority` — which knowledge-base path or module the skill consults for SOL math when the governing rule is implied (e.g., `knowledge-base/regulations/state-sol-calculator.md`); ensures every `[[CALCULATE SOL]]` flag points at a single firm-blessed source rather than ad-hoc lookup
- `client.intake_overrides.{client_id}` — per-client overrides for existing-client matter expansions (e.g., a corporate client whose master engagement letter pre-clears a category of matters and skips a fresh engagement letter for in-scope new matters)
- `firm.ethics.intake_facts_require_source_note` — non-overridable boolean codifying the Source-Note Traceability hard rule applied to every intake: every populated field in the Matter-Type Field Pack and every factual statement in the Narrative Summary carries an inline `(source: notes ¶N / form QN / dictation MM:SS)` tag, every inferred field is tagged `(inferred — confirm)`, and every fact with no source in the input is flagged `[[VERIFY]]` rather than asserted. The skill treats this as a hard rule even if absent from `config.yml`. This is the twelfth non-overridable rule in the repo and the intake-side analog of the demand-letter-drafter's Damages Traceability Rule and the legal-research-memo's Holdings Traceability Rule.

If a key is absent from `config.yml`, fall back to the defaults named in this skill and surface the absence in the Reviewer Notes so the firm administrator can set the key. The candidate matter number is *always* a placeholder until the conflict check clears; the skill does not write to the firm's matter-numbering system at intake. The skill never relaxes a hard rule based on a missing config value.

## Cross-References

- `skills/admin/document-intake-extractor.md` — when the intake includes documents (police report, medical records, contract, prior pleadings), run that skill on the documents and merge its field-pack output into this skill's Matter-Type Field Pack
- `skills/operations/legal-research-memo.md` — once intake is cleared and an engagement is opened, this skill's Narrative Summary feeds the matter-context block of the legal-research-memo
- `knowledge-base/regulations/` — primary source for matter-type-specific SOL and procedural deadlines
- `knowledge-base/best-practices/ai-governance-legal.md` — Rule 1.18 prospective-client confidentiality guidance for pre-engagement matter handling
- `knowledge-base/best-practices/agentic-legal-workflow-design.md` — the "Legal sycophancy and the elicitation paradox" entry (DLawBench, July 2026) that grounds the Elicitation-Depth Scaling rule

## Example Output

The following is a real worked example for a **personal-injury** phone intake — the matter type that most sharply exercises the SOL / critical-deadline flag and the Source-Note Traceability hard rule. The intake notes are deliberately incomplete (no adverse insurer, no police-report number) so the example shows how missing facts are flagged rather than filled in.

**Sample input provided to the skill:**

> - Intake source: Phone call
> - Matter type: Personal injury
> - Intake by: J. Alvarez, intake paralegal
> - Date of intake: 06-Jul-2026
> - Referral source: former client D. Kim
> - Raw notes (paragraph-numbered):
>   ¶1 Caller: Maria Renteria, cell 555-0148, prefers text. Rear-ended at a red light on 14-May-2026 at Grand Ave & 5th, [state not stated — caller lives in Sacramento].
>   ¶2 Other driver ran the light per caller; caller was stopped. Ambulance to Sutter ER same day; neck and lower-back pain, ongoing PT twice a week since.
>   ¶3 Caller has her own auto policy (Geico) with UM/UIM she thinks; not sure of the other driver's insurance. No police report number handy — says an officer responded.
>   ¶4 No prior back injuries. Missed 3 weeks of work (warehouse). Hasn't signed with any other firm; got our name from Dave Kim.
>   ¶5 Asked what our fee is — I said an attorney would go over the contingency terms; nothing quoted.

**Skill output:**

```
## Client Intake Summary — Personal injury — Maria Renteria

- **Intake date:** 06-Jul-2026
- **Intake by:** J. Alvarez, intake paralegal
- **Source:** Phone
- **Referral source:** D. Kim (former client)
- **Matter type:** Personal injury (auto — rear-end)
- **Firm licensure in governing jurisdiction:** CHECK — governing state not stated in notes; caller resides in Sacramento, CA `[[VERIFY: state of accident]]`
- **Candidate matter number:** 2026-NNNN (placeholder — assign on conflict clearance)
- **Earliest deadline:** `[[CALCULATE SOL: CA Code Civ. Proc. § 335.1 — 2-yr personal-injury SOL; if accident 14-May-2026 in CA, presumptive SOL 14-May-2028. CONFIRM state of accident before calendaring.]]`

## Prospective Client
- Name: Maria Renteria (source: notes ¶1)
- Contact: cell 555-0148, prefers text (source: notes ¶1)
- Other identifiers (to support conflict check): none provided `[[VERIFY: former names / entities]]`

## Parties
| Role | Name | Entity type | Relationship |
|------|------|-------------|--------------|
| Prospective client | Maria Renteria | Individual | Injured driver (source: notes ¶1) |
| Adverse party | Unidentified other driver | Individual | Ran red light (inferred — confirm; source: notes ¶2) |
| Adverse insurer | Unknown | Insurer | Other driver's carrier `[[VERIFY]]` (source: notes ¶3) |

## Matter-Type Field Pack — Personal injury
- Date of incident: 14-May-2026 (source: notes ¶1)
- Mechanism: auto — rear-end collision at red light (source: notes ¶1–2)
- Injuries: neck and lower-back pain, ongoing (source: notes ¶2)
- Treating providers to date: Sutter ER (day of); PT 2x/week ongoing (source: notes ¶2)
- Ambulance/ER records: ambulance to Sutter ER same day (source: notes ¶2)
- Police report: officer responded; no report number provided `[[VERIFY: police report #]]` (source: notes ¶3)
- Primary/secondary tortfeasors: other driver (source: notes ¶2)
- Own insurance: Geico auto; UM/UIM believed but unconfirmed `[[VERIFY: UM/UIM coverage]]` (source: notes ¶3)
- Adverse insurance: unknown `[[VERIFY]]` (source: notes ¶3)
- Prior injury history: none reported (source: notes ¶4)
- Employment impact: 3 weeks missed work, warehouse (source: notes ¶4)
- Governing state SOL: `[[CALCULATE SOL]]` — CA § 335.1 presumptive; confirm state of accident (source: notes ¶1)

## Narrative Summary
On 14-May-2026, prospective client Maria Renteria was stopped at a red light at Grand Ave & 5th when another vehicle allegedly ran the light and rear-ended her (source: notes ¶1–2). She was transported by ambulance to Sutter ER the same day and reports ongoing neck and lower-back pain treated with twice-weekly physical therapy (source: notes ¶2). She carries a Geico auto policy she believes includes UM/UIM coverage but is unsure of the other driver's insurance (source: notes ¶3). She reports no prior back injuries and missed three weeks of warehouse work (source: notes ¶4). She has not retained other counsel and was referred by former client Dave Kim (source: notes ¶4).

## Critical Deadlines & Calendar Items
| Deadline | Date | Basis | Action |
|----------|------|-------|--------|
| Personal-injury SOL | ~14-May-2028 (if CA) | CA Code Civ. Proc. § 335.1 (2 yr) | `[[CALENDAR IMMEDIATELY once state confirmed]]` |
| Notice to own insurer (UM/UIM) | ASAP | policy terms unknown | `[[VERIFY policy notice deadline]]` |
| Preserve evidence (vehicle, ER records) | ASAP | spoliation risk | Send preservation/records-request |

## Conflict-Check Trigger List
Route to firm conflict system before any representation decision:
- Maria Renteria (prospective client) — source: notes ¶1
- Unidentified other driver — source: notes ¶2 `[[VERIFY: name]]`
- Other driver's insurer — source: notes ¶3 `[[VERIFY: carrier]]`
- D. Kim (referral source) — source: notes ¶4

## Engagement-Decision Block
- **Fit with firm practice:** Fit — routine auto PI (per firm.matter_types)
- **Fee discussion captured:** N — no fee quoted; paralegal deferred to attorney (source: notes ¶5). Engagement letter must state contingency terms.
- **Retainer posture:** Contingent (default per firm.retainer_structures.personal_injury)
- **Referral obligations:** Non-attorney referral (former client); no Rule 1.5(e) fee-share implicated — confirm against firm.referral_fee_policy
- **Proposed next step:** Intake-attorney call to confirm state of accident and SOL, then engagement-letter issuance if cleared

## Engagement-Letter Triggers (if firm decides to represent)
- Scope of representation: PI claim arising from 14-May-2026 collision [to be confirmed]
- Fee basis: contingency [to be confirmed]
- Retainer amount: n/a (contingent) — costs advance per config
- Conflict-waiver language needed: N (pending conflict clearance)
- File-opening checklist: [firm template per config]

## Follow-Up Checklist (information still needed from prospective client)
1. State where the accident occurred (governs SOL)
2. Other driver's name and insurance carrier
3. Police report number / responding agency
4. Confirmation and declarations page for Geico UM/UIM coverage
5. Signed medical-records authorizations (Sutter ER + PT provider)

## Reviewer Notes
- **Placeholders:** state of accident, adverse driver/insurer identity, police report #, UM/UIM confirmation — all `[[VERIFY]]`; SOL `[[CALCULATE]]` pending state
- **Privilege/confidentiality posture:** Pre-engagement; Rule 1.18 prospective-client confidentiality applies — do not circulate until conflict clears
- **Urgency:** MEDIUM — SOL is ~22 months out if CA, but evidence preservation and insurer notice are time-sensitive
```

**Why this is the target quality:** every field and narrative sentence traces to a specific note paragraph per the Source-Note Traceability rule, so the intake attorney's review is a verification pass, not a re-interview. The one fact that controls the deadline — the state of the accident — is missing from the notes, so the SOL is surfaced as `[[CALCULATE SOL]]` with the presumptive California statute *and* an explicit "confirm state before calendaring" caveat rather than a false-precision date. Nothing is asserted that the notes do not support, and the conflict list carries a source tag per party so spellings can be confirmed before the conflict system runs.
