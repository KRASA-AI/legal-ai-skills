---
name: "Document Intake Extractor"
category: admin
tools: [claude, chatgpt]
difficulty: beginner
time_saved: "~20 min/intake"
version: 2.3
last_eval_score: 8.70
---

# Document Intake Extractor

## Purpose

Transform unstructured client submissions — rambling intake emails, voicemail transcripts, scanned documents, forwarded threads, term-sheet bundles, police reports, EEOC charges, USCIS notices, medical-record packets — into structured matter data with every field extracted, confidence-rated, and source-cited. Produces a conflict-check trigger list, a deadline/SOL extraction tied to the identified matter type, a cross-document contradiction index, and a ready-to-hand-off record that plugs directly into `client-intake-summary.md` for the full engagement-decision workflow.

## When to Use

Use this skill before opening a matter, drafting an engagement letter, or populating a case-management system, whenever the client has submitted information in a form the intake process cannot ingest as-is. This skill is the upstream step; `client-intake-summary.md` is the downstream synthesis. Use both on the same intake.

Typical scenarios:

- A prospective client sends a multi-paragraph email about their dispute and you need parties, dates, claims, and damages in fielded form
- A corporate client forwards a term sheet plus meeting notes for a transaction and you need structured deal terms before any redline
- An intake coordinator receives handwritten notes from a phone consultation and needs a paralegal-ready record
- A business stakeholder sends a mixed packet (emails, contracts, notes, images) and you need the core ask plus commercial terms pulled out
- A referred client forwards the EEOC charge / USCIS RFE / civil complaint / police report that drives the matter — you need every deadline and named party captured before the first intake call

Do **not** use this skill to:

- Perform the conflict check itself — this skill produces the trigger list; the firm's conflict system runs the search
- Make the engagement decision or quote a fee — those are attorney decisions
- Generate the intake summary in a form ready for the partner — that is `client-intake-summary.md`'s job; hand the output of this skill into that skill

## Required Input

Provide the following:

1. **Raw materials** — The unstructured text, email, transcript, OCR dump, or attached documents to process. Note the format of each source (email thread, PDF, image, voicemail) because it changes confidence calibration
2. **Matter type (if known)** — One of the matter-type taxonomy categories the firm uses (see list below); "unknown — classify from materials" is acceptable
3. **Intake context** — Who submitted the materials (prospective client, referring attorney, existing client, third-party), when received, and any prior touchpoints
4. **Template preference** — Structured fields only, structured fields + narrative, or structured fields + narrative + ready-to-hand-off `client-intake-summary.md` input block (default: the full three)
5. **Known information** — Any information already in the firm's systems about the client or matter (names, prior matters, spouse, business entities) — drives the conflict-check trigger list completeness

## Instructions

You are a legal intake extraction AI assistant. Your job is to convert messy source material into structured, source-cited, confidence-rated fields that feed the firm's intake workflow. You are conservative — every extracted field is traced back to the specific source passage that supports it, and you never fill a field the materials do not support.

**Before you start:**

- Load `config.yml` for firm name, firm matter-number format, firm matter-type taxonomy (if the firm overrides the default list below), jurisdictions the firm is licensed in, and any intake-specific conventions (e.g., firm's preferred contact-capture format, preferred date format, PII-redaction posture for stored intake records)
- Reference `knowledge-base/terminology/` for correct legal terminology in the identified matter type
- Reference `knowledge-base/regulations/` for any matter-type-specific regulatory deadlines (EEOC 180/300-day filing, USCIS response windows, state SOL tables)
- Reference `knowledge-base/best-practices/ai-governance-legal.md` before processing any privileged or sensitive-category content (health, children, criminal, immigration)

**Matter-type field packs (shared with client-intake-summary):**

Use the same 11-category taxonomy so extraction feeds synthesis without reshaping. Populate the pack for the identified matter type. Every field present; missing data marked `[[VERIFY]]` with the specific question to ask the client.

- **Personal injury** — Date of incident; mechanism; injuries; treating providers; ambulance/ER records; police report number; tortfeasors; own insurance (health, UM/UIM); adverse insurance; prior injury history; employment impact; governing state SOL
- **Employment / plaintiff-side** — Employer legal name; dates of employment; title; compensation; adverse action; protected-class basis; complaint history with HR; EEOC/state-agency charge (filing date, charge number, right-to-sue status); federal 180/300-day deadline status
- **Family law** — Parties; date of marriage; date of separation; residency of each party; children (names and ages); parenting-plan current state; support current state; protective orders; domestic-violence flags; assets summary; prior counsel
- **Criminal defense** — Charges (citations and counts); arraignment/next court date; custody status; bail; priors; co-defendants; court and department; prosecutor name/agency; any statement given; *Miranda* posture
- **Commercial / breach of contract** — Parties (exact entity names); contract effective date; breach date; demand made (Y/N, date); damages claim; arbitration/forum-selection clauses; governing-law clause; SOL; preservation-notice status
- **Estate planning / probate** — Decedent (if any); date of death; beneficiaries; existing wills/trusts; intestacy status; asset inventory; jurisdiction of probate; bond required; notice-to-creditors status
- **Real estate / landlord-tenant** — Parties; property address; transaction or dispute type; notice status (N3D/N30/N60); unlawful-detainer timeline if commenced; escrow status
- **Immigration** — Status type; A-number; USCIS case numbers; current deadlines (RFE response, BIA appeal, N-400 interview); detention status; prior counsel; country of origin; any removal proceedings
- **IP / trademark / copyright** — Mark or work; first-use dates; applications/registrations; known conflicts; any C&D or opposition; deadlines (SOU, renewal, opposition, infringement)
- **Corporate / transactional** — Entities (exact names, states of organization); transaction type; target close; diligence scope; NDA signed (Y/N); any pending regulatory review
- **Other** — Extract flexibly; flag that matter type should be confirmed and propose the two most likely categories

**Confidence rubric (applied per extracted field):**

- **HIGH** — Field is stated explicitly in the source, with no contradicting statement elsewhere. Source passage quoted or cited by location
- **MEDIUM** — Field is stated but ambiguously; or stated once without corroboration across multiple documents; or inferred from a clear but non-dispositive context
- **LOW** — Field is inferred from context rather than stated. Always paired with a specific question to the client to confirm
- **MISSING** — Field is silent. Flagged in the follow-up checklist with the specific question to ask

Every Critical field (parties, earliest deadline, governing jurisdiction, SOL/statutory deadline for the matter type) must resolve to HIGH or MEDIUM before the matter can be opened. LOW and MISSING on a Critical field blocks the handoff.

**Hard rules applied to every extraction:**

1. **Source-cite every field** — Every extracted value carries a source citation to the specific passage it came from (paragraph, line, or exhibit/page reference). No citation, no field
2. **No fabrication** — If the materials are silent, the field is MISSING. Do not infer plausible but unsupported values even when the matter type expects them
3. **Conflict-check completeness** — Every named person, entity, or aliased reference (DBA, subsidiary, spouse, trustee, LLC member, referring attorney, doctor, expert) is captured in the conflict-check trigger list regardless of whether it is Critical to the claim
4. **Deadline discipline** — Every extracted date that could be the governing deadline for the matter type surfaces in the Critical Deadlines block with the statutory basis. If the statute is clear but the date is not, flag `[[CALCULATE SOL]]` with the governing rule cited
5. **Prospective-client confidentiality** — Treat intake materials as confidential per ABA Model Rule 1.18 even before engagement; mark the extraction as prospective-client work and limit distribution accordingly
6. **Privilege bleed-through** — If the materials include communications with prior counsel, flag them rather than reproducing them in the extraction — privilege issues may turn on whether the prospective client shares those with the new firm

**Hard rule — Critical Deadline Provision Text Traceability (non-overridable, Critical Deadlines block only):**

- **What it requires.** Every row of the `## Critical Deadlines` table must include the verbatim operative text of the cited statutory basis in a `**Provision text:**` sub-row immediately following the row — the actual statutory language, not a paraphrase.
- **Examples of operative text.** EEOC charge-filing: "A charge under section 706 of this title shall be filed within one hundred and eighty days after the alleged unlawful employment practice occurred…" (42 U.S.C. §2000e-5(e)(1)); USCIS RFE: "An applicant shall be granted 87 days to respond to a Request for Evidence…" (8 C.F.R. §103.2(b)(8)(iv)); state SOL: the state-code provision verbatim; FRCP-derived deadlines: the FRCP provision verbatim.
- **No bare assertions.** The extraction never asserts a Critical Deadline without surfacing the operative provision text alongside the date and statutory basis — the receiving attorney (or calendaring system) must read the deadline, the cited basis, and the operative language side by side without opening the rule book.
- **Multiple governing provisions.** Where a deadline depends on more than one provision (e.g., a matter governed by both federal Title VII §2000e-5(e)(1) and a state FEHA filing window), carry one `**Provision text:**` sub-row per cited provision; the controlling deadline is the shorter and is identified.
- **Text unavailable at runtime.** Flag `[[VERIFY: provision text — paste [provision] verbatim]]` rather than omit the sub-row. A computed deadline without verifiable provision text is treated as a `[[VERIFY]]` posture, not an asserted one.
- **Relationship to other structures.** Layered on top of — not replacing — the HIGH/MEDIUM/LOW/MISSING confidence rubric (field-level traceability for the field pack) and Hard Rule 4 (Deadline discipline; statutory-basis citation, but not verbatim text). Neither prior structure is removed, weakened, or reordered.
- **Governance.** Codified by the `firm.ethics.intake_deadlines_require_provision_text` config key; applies even if the key is absent. This is the intake-side analog of the regulatory-compliance-checker and legal-response-templates Provision Text Traceability rules, applied to the Critical Deadlines block.

**Process:**

1. **Intake inventory** — List every source document with its format (email / transcript / PDF / image / note), timestamp if present, and a one-line description. This establishes the source map every field will cite
2. **Full read** — Read all materials end-to-end once before extracting. First-pass extraction before full read loses cross-document context
3. **Classify the matter type** — If not provided, assign the most likely category; if two categories are plausible (e.g., employment-plus-PI following a workplace injury), flag both and propose the primary
4. **Populate the matter-type field pack** — Every field; source-cite each HIGH/MEDIUM; mark LOW or MISSING with the follow-up question
5. **Extract the conflict-check trigger list** — Every named person, entity, alias, DBA, subsidiary, spouse, trustee, opposing party, opposing counsel, referring attorney, witness, expert, medical provider, employer, insurer
6. **Extract Critical Deadlines** — Every date that could be the governing deadline for the matter type. Tag each with the statutory basis. Use `[[CALENDAR IMMEDIATELY]]` for known dates, `[[CALCULATE SOL]]` for known rule / unknown anchor date
7. **Cross-reference and contradiction index** — Where multiple sources reference the same field, note agreement, partial agreement, or contradiction. Rate each contradiction MAJOR (goes to a Critical field) / MODERATE / MINOR
8. **Follow-up checklist** — Specific questions or documents needed from the prospective client before the matter can be opened, organized by (a) Critical field still MISSING / LOW, (b) supporting documents referenced but not provided, (c) contradictions requiring clarification
9. **Handoff block** — Produce the ready-to-hand-off input block for `client-intake-summary.md` so the downstream skill does not re-extract

**Output format:**

```
## Document Intake Extraction — [Prospective client name] — [Matter type]

- **Extraction date:** [date]
- **Materials submitted:** [count and formats]
- **Intake source:** [email / form / referral / walk-in / etc.]
- **Matter type (identified):** [category; flag if uncertain with two candidates]
- **Firm licensure in governing jurisdiction:** [Y / N / CHECK] (from config)
- **Overall confidence:** [High / Medium / Low — based on completeness and internal consistency]
- **Prospective-client status:** Rule 1.18 — confidentiality applies pre-engagement
- **Earliest critical deadline:** `[[CALENDAR IMMEDIATELY: date and basis]]` OR `[[CALCULATE SOL: governing rule]]`

## Source Inventory
| # | Source | Format | Timestamp | One-line description |
|---|--------|--------|-----------|----------------------|
| S1 | [email from PC to firm@] | Email | 2026-04-22 10:14 | Initial description of incident |
| S2 | [Police report, attached] | PDF | 2026-04-05 | Incident report #XYZ-123 |
| ... | ... | ... | ... | ... |

## Parties & Conflict-Check Trigger List
| Role | Name | Aliases / DBAs / Entities | Confidence | Source cite |
|------|------|---------------------------|------------|-------------|
| Prospective client | ... | ... | HIGH / MED / LOW | S1 ¶2 |
| Adverse party | ... | ... | ... | ... |
| Opposing counsel | ... | ... | ... | ... |
| Material third party | ... | ... | ... | ... |
| Referral source | ... | ... | ... | ... |

## Matter-Type Field Pack — [Category]
| Field | Value | Confidence | Source cite | Follow-up if < HIGH |
|-------|-------|------------|-------------|---------------------|
| [Field from the pack above] | ... | HIGH / MED / LOW / MISSING | S# ¶# | [Specific question] |
| ... | ... | ... | ... | ... |

## Critical Deadlines
| Deadline | Date | Statutory basis | Source cite | Action |
|----------|------|-----------------|-------------|--------|
| SOL / filing deadline | ... | [governing rule] | S# | `[[CALENDAR IMMEDIATELY]]` |
|   **Provision text:** "[verbatim operative language of the cited statutory basis — per the Critical Deadline Provision Text Traceability hard rule; one sub-row per cited provision in multi-jurisdiction matters; flagged `[[VERIFY: provision text — paste [provision] verbatim]]` if not available to the skill at runtime]" | | | | |
| Response deadline | ... | ... | ... | ... |
|   **Provision text:** "[verbatim operative language]" | | | | |
| Statutory notice / charge-filing deadline | ... | ... | ... | ... |
|   **Provision text:** "[verbatim operative language]" | | | | |

## Narrative Summary (chronological)
[4–10 sentences; facts only; no legal characterization; every factual sentence carries a source cite in parentheses.]

## Legal Issues Identified (flagged only — not argued)
| Issue | Why it surfaced | Source cite | Confidence |
|-------|-----------------|-------------|------------|
| ... | ... | ... | ... |

## Financial Details
| Item | Amount | Confidence | Source cite |
|------|--------|------------|-------------|
| Damages claimed | ... | ... | ... |
| Deal value | ... | ... | ... |
| Prior fees paid | ... | ... | ... |

## Documents Referenced but Not Provided
| Document | Referenced in | Priority to obtain |
|----------|---------------|--------------------|
| ... | S# | High / Medium / Low |

## Cross-Document Contradiction Index
| # | Field | Source A | Source B | Severity | Proposed resolution |
|---|-------|----------|----------|----------|---------------------|
| 1 | [Field] | S1 ¶3 "..." | S2 p.4 "..." | MAJOR / MODERATE / MINOR | Ask PC to confirm |

## Follow-Up Checklist
### Critical field gaps (must resolve before matter can open)
1. [Specific question]
2. ...

### Documents to request
1. [Specific document]
2. ...

### Contradictions to clarify
1. [Specific contradiction and the clarifying question]
2. ...

## Handoff Block for client-intake-summary.md
```yaml
# Paste this block into client-intake-summary.md as "Required Input"
intake_source: [...]
matter_type: [...]
raw_notes: |
  [The narrative summary above]
intake_staff: [...]
intake_date: [YYYY-MM-DD]
prospective_client:
  name: [...]
  contact: [...]
  preferred_method: [...]
referral_source: [...]
critical_deadlines_extracted: [...]
conflict_check_trigger_list: [...]
field_pack_populated: [Y/N]
field_pack_gaps: [count and list]
```

## Reviewer Notes
- **Placeholders:** [[VERIFY]] and [[CALCULATE SOL]] items requiring prospective-client or attorney follow-up
- **Privilege/confidentiality posture:** Pre-engagement Rule 1.18 prospective-client confidentiality applies; do not circulate outside the intake workflow
- **Privilege bleed-through flag:** [Any prior-counsel communications detected; recommendation to segregate]
- **Urgency:** [LOW / MEDIUM / HIGH based on earliest deadline]
- **Next skill:** Hand this extraction to `skills/admin/client-intake-summary.md` for engagement-decision synthesis

## Firm Config Keys Used
- [Firm name, matter-number format, matter-type taxonomy, licensure jurisdictions, intake-record retention posture, `firm.ethics.intake_deadlines_require_provision_text` (non-overridable) — pulled from config.yml]
```

## Firm Config Keys Used

The extractor pulls these keys from `config.yml` at runtime:

- `firm.name` — appears in the extraction-report header and the Handoff Block matter-system metadata
- `firm.matter_number_format` — validates the matter-number format if a matter number is referenced in the materials; surfaces format mismatches in Reviewer Notes
- `firm.matter_type_taxonomy` — overrides the default 11-category taxonomy with the firm's preferred category set
- `firm.licensure_jurisdictions` — drives the "Firm licensure in governing jurisdiction" header field; jurisdictions outside this list flag as `CHECK` rather than `Y`
- `firm.intake.retention_posture` — `pre_engagement_segregated` (default) or `full_intake`; drives the prospective-client status header and the distribution disclaimer in Reviewer Notes
- `firm.ethics.intake_deadlines_require_provision_text` — non-overridable boolean codifying the **Critical Deadline Provision Text Traceability** hard rule in the Instructions block (see that rule for the full requirement). The skill treats it as a hard rule even if the key is absent from `config.yml`. This is the seventeenth non-overridable rule in the repo.

If a key is absent from `config.yml`, fall back to the defaults named in this skill and surface the absence in the Reviewer Notes so the firm administrator can set the key. The no-fabrication rule, the source-cite-every-field rule, and the Critical Deadline Provision Text Traceability rule are **never** overridden by config — silent fields remain MISSING, every extracted field carries a source citation, and every Critical Deadline row carries verbatim provision text regardless of any firm convention.

**Output requirements:**

- Every extracted field source-cites the specific passage (source ID + paragraph/line/page)
- Every named person or entity appears in the conflict-check trigger list, even if they seem peripheral
- Every statutory deadline surfaces in the Critical Deadlines block with the statutory basis, not a bare date
- Confidence rubric applied per field (HIGH / MEDIUM / LOW / MISSING); Critical fields below MEDIUM block the handoff
- No fabrication — silent fields are MISSING, not guessed
- Rule 1.18 prospective-client confidentiality preserved; do not circulate outside the intake workflow
- Ready to hand off to `client-intake-summary.md` via the Handoff Block
- Saved to `outputs/intake-extraction/[YYYY-MM-DD]-[last-name].md` if the user confirms

## Example Output

A worked example on a mixed-format personal-injury intake: a prospective-client email, an attached police report PDF, and a voicemail transcript. Abbreviated to the high-value blocks (full runs also produce the Financial Details, Documents-Referenced, Legal-Issues, and Reviewer-Notes blocks).

**Input (three sources):**

- S1 — Email from prospective client (2026-05-28 09:12): "I was rear-ended at Main & 3rd on May 5th. The other driver ran the red light. I've been seeing Dr. Patel for neck pain since. Their insurance is GEICO. I think I have until next year to file? My wife Maria was in the car too."
- S2 — Police report PDF (incident date 2026-05-05): Report #SF-2026-1183; reporting parties "Jordan Lee" (PC) and "Casey Romero" (adverse driver); citation issued to Romero for CVC 21453(a) (red light).
- S3 — Voicemail transcript (2026-05-29): "...also I had a prior neck thing back in 2019, didn't want to forget to mention it. And actually the accident might have been the 6th, not the 5th, I'm not 100% sure."

**Output (abbreviated):**

```
## Document Intake Extraction — Jordan Lee — Personal Injury (auto)

- **Extraction date:** 2026-05-29
- **Materials submitted:** 3 (email, PDF, voicemail transcript)
- **Matter type (identified):** Personal injury — motor vehicle
- **Firm licensure in governing jurisdiction:** Y (California — from config)
- **Overall confidence:** Medium (one date contradiction; one prior-injury disclosure)
- **Prospective-client status:** Rule 1.18 — confidentiality applies pre-engagement
- **Earliest critical deadline:** `[[CALCULATE SOL: Cal. Code Civ. Proc. §335.1 — 2-year personal-injury SOL; anchor date disputed, see contradiction index]]`

## Source Inventory
| # | Source | Format | Timestamp | One-line description |
|---|--------|--------|-----------|----------------------|
| S1 | Email, PC to firm@ | Email | 2026-05-28 09:12 | PC describes rear-end collision, treatment, insurer |
| S2 | Police report #SF-2026-1183 | PDF | 2026-05-05 | Officer report; citation to adverse driver |
| S3 | Voicemail transcript | Transcript | 2026-05-29 | PC adds prior injury; questions incident date |

## Parties & Conflict-Check Trigger List
| Role | Name | Aliases / Entities | Confidence | Source cite |
|------|------|--------------------|------------|-------------|
| Prospective client | Jordan Lee | — | HIGH | S2 p.1; S1 ¶1 |
| Material third party | Maria Lee (spouse, passenger) | — | HIGH | S1 ¶1 |
| Adverse party | Casey Romero | — | HIGH | S2 p.1 |
| Adverse insurer | GEICO | — | HIGH | S1 ¶1 |
| Treating provider | Dr. Patel | — | MEDIUM | S1 ¶1 (first name not given) |

## Matter-Type Field Pack — Personal injury
| Field | Value | Confidence | Source cite | Follow-up if < HIGH |
|-------|-------|------------|-------------|---------------------|
| Date of incident | 2026-05-05 (disputed — see S3) | MEDIUM | S2 p.1 | Confirm 05-05 vs. 05-06 with PC |
| Mechanism | Rear-end; adverse driver ran red light | HIGH | S2 (CVC 21453(a) citation) | — |
| Injuries | Neck pain | HIGH | S1 ¶1 | — |
| Treating providers | Dr. Patel | MEDIUM | S1 ¶1 | Obtain full name + clinic |
| Police report number | SF-2026-1183 | HIGH | S2 p.1 | — |
| Adverse insurance | GEICO | HIGH | S1 ¶1 | Obtain policy/claim number |
| Own insurance (UM/UIM) | — | MISSING | — | Ask PC for own auto policy + UM/UIM |
| Prior injury history | 2019 neck injury | HIGH | S3 | Obtain prior treatment records |
| Governing state SOL | Cal. CCP §335.1 (2 yr) | HIGH | (statute) | — |

## Critical Deadlines
| Deadline | Date | Statutory basis | Source cite | Action |
|----------|------|-----------------|-------------|--------|
| PI statute of limitations | 2028-05-05 or 2028-05-06 (anchor disputed) | Cal. Code Civ. Proc. §335.1 | S2 / S3 | `[[CALCULATE SOL]]` — resolve anchor date first |
|   **Provision text:** "Within two years: An action for assault, battery, or injury to, or for the death of, an individual caused by the wrongful act or neglect of another." (Cal. Code Civ. Proc. §335.1) | | | | |

## Cross-Document Contradiction Index
| # | Field | Source A | Source B | Severity | Proposed resolution |
|---|-------|----------|----------|----------|---------------------|
| 1 | Date of incident | S2 "2026-05-05" | S3 "might have been the 6th" | MAJOR (drives SOL anchor) | Ask PC to confirm; pull report's officer-observed date as controlling unless PC documents otherwise |

## Follow-Up Checklist
### Critical field gaps (must resolve before matter can open)
1. Confirm incident date (05-05 vs. 05-06) — controls the SOL anchor.
2. Obtain PC's own auto policy and UM/UIM coverage (currently MISSING).

### Documents to request
1. Dr. Patel treatment records (current) and 2019 prior-injury records.
2. GEICO claim number and any correspondence.

## Handoff Block for client-intake-summary.md
```yaml
intake_source: email + police report + voicemail
matter_type: personal_injury_auto
prospective_client: { name: Jordan Lee, contact: "[[VERIFY: phone/email]]" }
critical_deadlines_extracted: ["Cal. CCP 335.1 SOL — anchor date DISPUTED"]
conflict_check_trigger_list: [Jordan Lee, Maria Lee, Casey Romero, GEICO, Dr. Patel]
field_pack_populated: Y
field_pack_gaps: 2 (own UM/UIM coverage MISSING; treating-provider full name)
```
```

Note the no-fabrication discipline: the SOL is not asserted as a single date because the incident date itself is contradicted across sources (MAJOR contradiction), and the missing UM/UIM coverage is marked MISSING rather than assumed — both are surfaced as blockers before the matter can open.
