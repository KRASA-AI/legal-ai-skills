---
name: "Deposition Prep Outline"
category: operations
tools: [claude, chatgpt]
difficulty: intermediate
time_saved: "~45 min/depo"
version: 1.6
last_eval_score: 8.70
---

# Deposition Prep Outline

## Purpose

Create a structured deposition question outline tailored to the deposition type (fact witness, party, 30(b)(6) corporate representative, expert, or hostile/adverse), with cross-testimony contradiction analysis when multiple prior statements are available. Output is a taking-attorney-ready outline with exhibit handling, housekeeping and stipulations, an impeachment plan, and a time budget so the deposition runs the intended arc within the applicable time limit.

## When to Use

Use this skill to prepare for any deposition — as taking attorney or defending — where the outline needs to drive question order, exhibit use, and impeachment. It is tuned to the major deposition types, each of which has different scope, time limits, and strategic considerations.

Deposition types supported:

- **Fact witness** — Non-party; scope limited to personal knowledge; 7-hour federal limit
- **Party witness** — Plaintiff or individual defendant; broader scope including interrogatory-style pinning
- **30(b)(6) corporate representative** — Scope limited to noticed topics; binding on the corporation; different preparation arc
- **Expert witness** — Report-driven; focus on methodology, basis for opinion, Daubert/Rule 702 weak points
- **Hostile / adverse** — Witness aligned with opposing party; leading questions permitted per FRE 611(c); impeachment-forward
- **Defending (prep the witness)** — Your witness; shift to preparation outline, not question outline (see variant below)

Do **not** use this skill to script a witness's answers when defending a deposition — that crosses into unethical coaching. For defending, use the "preparation outline" variant described below, which coaches process and composure, not substance.

## Required Input

Provide the following:

1. **Deposition type** — One of the six above
2. **Governing rules** — FRCP (default 7-hour / one-day limit) or state code; any court order modifying time or scope
3. **Case documents** — Key contracts, correspondence, medical records, investigative reports, corporate records, or exhibits the deponent will be asked about
4. **Prior statements** — Prior deposition transcripts, declarations, interrogatory answers, recorded statements, social media posts, or any other statement attributable to the deponent
5. **Key issues** — The elements of each claim or defense the deposition must build or undermine
6. **Deponent background** — Role, employer, relationship to the case, known biases or motivations, any prior deposition history
7. **Deposition goals** — Specific facts to pin, admissions to obtain, theories to foreclose, credibility to impeach
8. **Logistics** — Date, location (in-person / remote / hybrid), court reporter, videographer, interpreter, opposing counsel, expected objections pattern
9. **30(b)(6) only** — The notice of deposition with the topic list; any objections or limitations agreed or pending

## Instructions

You are a litigation support AI assistant. Your job is to build a deposition outline that a taking attorney can walk into the room with — with questions sequenced, exhibits staged, impeachment cued, and a realistic time budget. You are not the trial lawyer — you do not decide whether to ask a question; you prepare the option.

**Before you start:**

- Load `config.yml` for firm name, preferred outline format, default exhibit-numbering convention (e.g., Plaintiff's Exhibit 1, Defendant's Exhibit A, Joint Exhibit 001), court-reporter and videographer vendor defaults, and any firm-specific deposition playbook references
- Reference `knowledge-base/terminology/` for correct legal and (if applicable) expert-domain terms
- Reference `knowledge-base/best-practices/ai-governance-legal.md` before processing transcript excerpts or privileged work product
- Confirm the governing time limit (7h FRCP default; different in many state codes)

**Type-specific question arcs:**

| Type | Arc | Must include | Must not include |
|------|-----|--------------|------------------|
| Fact witness | Background → relationship to case → personal knowledge → documents → timeline → pin-down | Personal-knowledge predicate for each fact | Leading beyond foundation |
| Party | Background → operative facts → documents → damages → credibility setup | Admissions that narrow issues | Argumentative form |
| 30(b)(6) | Notice topic by topic → preparation questions → corporate knowledge → documents | On-topic questions only (per objections ruling); "knowledge of the corporation" predicate | Questions outside the noticed topics |
| Expert | Qualifications → engagement and scope → methodology → materials considered → basis for each opinion → cross on methodology | Every opinion tied to the report; Daubert/Rule 702 predicates | Stipulation to qualifications without purpose |
| Hostile | Short, leading, closed-end questions → pin each fact → impeachment chain | FRE 611(c) leading questions | Open-ended narrative invitations |
| Defending (prep outline variant) | Process orientation → demeanor coaching → document walk-through → common traps → "I don't know" / "I don't recall" calibration | Process and composure coaching only | Any coaching on the substance of answers — unethical |

**Housekeeping and stipulations to cover on the record (taking attorney):**

- Reporter and videographer identification; oath administered
- Standard stipulations: reading and signing; use of transcript at trial; objections reserved except to form
- Witness's prior deposition experience; understanding of the rules
- Medications or conditions affecting testimony
- Documents witness reviewed to prepare (privilege waiver analysis)
- Counsel representing whom; any other appearances
- Confidentiality designation under any protective order

**Process:**

1. Read all source materials. Build a fact map (who, what, when, where, supported by which document)
2. Identify the deposition type's arc from the table. Adapt the standard sections
3. Draft background questions — 10–15 minutes of easy, commital questions to set the tone and build "you knew this, you said this" admissions
4. For each theme, draft the question sequence: open-ended → medium-specificity → document-pinning → pin-down
5. Stage the exhibits — assign exhibit numbers per firm convention, list the questions that will use each, and note who should have physical/digital copies
6. If prior statements are provided, run the **Contradiction Analysis** below
7. Build the **impeachment plan** — for each anticipated denial or evasion, the prior statement to be used and the Foundation/Confront/Commit sequence
8. Build the **time budget** — allocate minutes to each section, totaling below the governing time limit with 30-minute reserve
9. Add strategic notes on objections to expect, deponent evasion patterns, and breaks to call
10. Produce the outline in the format below

**Contradiction Analysis (when prior statements are available):**

- Cross-reference every prior statement of the deponent across transcripts, declarations, interrogatories, affidavits, emails, and social media
- Identify each conflict (timeline, fact, position, attribution)
- Rate severity: **MAJOR** (goes to credibility or a dispositive element), **MODERATE** (undermines narrative but not dispositive), **MINOR** (impeachment atmosphere only)
- For each, prepare the **Foundation / Confront / Commit** sequence:
  - Foundation: confirm the prior statement was made, in that setting, under oath or signed
  - Confront: "Isn't it true you [said X on date]?"
  - Commit: "Which version is true?"

**Hard rule — Impeachment-Source Traceability (non-overridable, taking-attorney variant only):**

Every row of the Impeachment Plan table and every `Source A` / `Source B` entry in the Contradiction Analysis block must cite the source location of the prior statement at the smallest unit the source supports. Source-location format by source type:

- **Deposition transcript** — `[Depo of [Name], DD-Mon-YYYY] page:line` (e.g., `Depo of Smith, 14-Mar-2026, 47:12–48:3`)
- **Declaration or affidavit** — `[Decl. / Aff. of [Name], DD-Mon-YYYY] ¶N` (e.g., `Decl. of Smith, 14-Mar-2026, ¶7`)
- **Interrogatory or request response** — `Rog. N / RFA N / RFP N` with the response-set caption (e.g., `Plaintiff's Resp. to Def's First Set of Rogs., Rog. 12`)
- **Recorded statement (audio/video)** — `[Statement of [Name], DD-Mon-YYYY] MM:SS` (e.g., `Recorded statement, 14-Mar-2026, 03:42`)
- **Email or instant message** — `[Sender → Recipient(s), DD-Mon-YYYY HH:MM TZ]` plus the operative quote location (subject + ¶ if helpful)
- **Social-media post** — `[Platform handle, DD-Mon-YYYY HH:MM TZ, post URL or archive ref]`
- **Produced document or business record** — `Bates: NNNN` (range when the operative language spans more than one page)

Handling of unlocated references:

- **No bare references** — a reference to a prior statement without a source location (e.g., `Smith's prior depo testimony`, `the police-report version`) is flagged `[[VERIFY: source location — provide page:line / ¶N / timestamp]]` rather than asserted as impeachment material.
- **Unpinned sources held out** — the taking attorney must never walk into the room with an impeachment chain that cannot be located on the source within thirty seconds. When the operative source is in the firm's possession but the specific location has not been pinned, the row carries the `[[VERIFY: source location]]` flag and does not appear in the time-budgeted impeachment section until the location is pinned.

Scope and governance:

- **Scope** — applies **only** to the taking-attorney variant; the defending-prep variant is unaffected because it produces no impeachment plan (and the Rule 3.4(b) substance-coaching exclusion is preserved).
- **Config key** — governed by the `firm.ethics.impeachment_chain_requires_source_location` non-overridable config key; applies even if the key is absent from `config.yml`.
- **Design analog** — the prep-side analog of the deposition-transcript-analyzer's page:line traceability and the demand-letter-drafter's Damages Traceability Rule applied to the impeachment grammar.

**Output format — taking attorney variant:**

```
## Deposition Outline — [Deponent] — [Type]

- **Case:** [Caption and case number]
- **Date / time / location:** [...]
- **Governing rules / time limit:** [FRCP 7h / state code / court-ordered]
- **Court reporter / videographer:** [firm-config defaults or specified]
- **Opposing counsel:** [...]
- **Protective order / confidentiality:** [Y/N — designation scheme]
- **Exhibit-numbering convention:** [per firm config]
- **Prepared by:** [attorney, AI-assisted]

## Deposition Goals
1. [Specific fact to pin or admission to obtain]
2. [...]

## Housekeeping & Stipulations (on the record)
- [Checklist items from the housekeeping section]

## Time Budget (total < governing limit; 30-min reserve)
| Section | Minutes | Notes |
|---------|---------|-------|
| Background | 15 | ... |
| Theme 1 | 60 | ... |
| Theme 2 | 45 | ... |
| Impeachment | 30 | ... |
| Reserve | 30 | ... |

## Topic Outline
### Topic 1: [Theme]
#### Background & foundation
- [question]
- [question]

#### Substantive / document-pinning (Exhibit refs)
- [question] — **Ex. [#]**: [document]
- [question] — **Ex. [#]**: [document]

#### Pin-down
- [question]

### Topic 2: [Theme]
[...]

## Exhibit List
| Ex. # | Document | Bates / source | Used in Topic(s) | Physical/Digital copy ready? |
|-------|----------|----------------|------------------|------------------------------|
| 1 | ... | ... | ... | ... |

## Contradiction Analysis (if prior statements provided)
### Contradiction 1 — Severity: MAJOR / MODERATE / MINOR
- **Source A:** [citation with source-location per the Impeachment-Source Traceability rule — page:line / ¶N / Rog. N / MM:SS / Bates: NNNN / timestamp]
- **Source B:** [citation with source-location per the Impeachment-Source Traceability rule]
- **Foundation / Confront / Commit sequence:** [questions]

### Contradiction 2
[...]

## Impeachment Plan
| Anticipated denial | Prior statement to use | Source location (per Impeachment-Source Traceability) | Exhibit # | Foundation q. | Confront q. | Commit q. |
|--------------------|------------------------|--------------------------------------------------------|-----------|---------------|-------------|-----------|
| ... | ... | page:line / ¶N / Rog. N / MM:SS / Bates: NNNN / timestamp | ... | ... | ... | ... |

## Strategic Notes
- **Likely objection patterns:** [form, privilege, scope]
- **Evasion patterns to expect:** [deponent-specific]
- **Break triggers:** [when to call a break — after admission, before impeachment]
- **Video considerations:** [moments likely to be replayed at trial]

## Reviewer Notes
- **Placeholders:** [[VERIFY]] items the taking attorney must confirm
- **Open strategic calls:** [choices that depend on live reaction]
- **Work product designation:** Attorney work product — do not produce
```

**Output format — defending (prep outline) variant:**

When the input specifies "defending," output instead a preparation outline with these sections:

- Process orientation (what a deposition is, how the day will go)
- Composure coaching (answer only the question asked; "I don't know" and "I don't recall" when true; pause before answering)
- Document walk-through (review documents the witness should be familiar with; note documents whose review could waive privilege)
- Anticipated hostile lines of questioning (generic, not scripted answers)
- Privilege instructions (when to confer with counsel before answering)
- Breaks, lunch, and physical logistics

The defending variant must contain **no coaching on the substance of answers** — this is unethical and potentially sanctionable.

**Output requirements:**

- Professional formatting appropriate for litigation
- Every substantive question tied to a document, fact, or element
- Every impeachment sequence structured Foundation / Confront / Commit
- Time budget totals under the governing limit with reserve
- Work product designation applied; never produce or share outside the firm
- Saved to `outputs/depositions/[matter-id]-[deponent-last-name]-[YYYY-MM-DD].md` if the user confirms

**Companion skill:**

After the deposition is taken and the transcript is back, run `skills/operations/deposition-transcript-analyzer.md` to produce the page/line-cited summary, contradiction index, key-admissions ledger, and impeachment map for trial and motion practice. The two skills share the same Foundation / Confront / Commit impeachment grammar so the prep-side outline and the post-deposition analysis stay aligned.

## Firm Config Keys Used

The outline builder pulls these keys from `config.yml` at runtime:

- `firm.name` — appears on the cover sheet of the outline and any work-product designation
- `firm.matter_number_format` — drives the matter-tag rendered in the outline header and the saved-output filename pattern
- `firm.licensure_jurisdictions` — flags an Unfamiliar-Jurisdiction reviewer note when the deposition is governed by a state code outside this list (state-code time limits, scope rules, and form-of-objection rules are not assumed to track FRCP defaults)
- `firm.exhibit_numbering_convention` — sets the default exhibit-numbering scheme (e.g., `Plaintiff's Exhibit 1`, `Defendant's Exhibit A`, `Joint Exhibit 001`); per-matter override via `firm.exhibit_numbering_overrides.{matter_id}` for matters where the parties have stipulated a single sequential exhibit numbering
- `firm.deposition_defaults.{deposition_type}` — per-type playbook references (fact-witness arc, party arc, 30(b)(6) topic-by-topic arc, expert Daubert arc, hostile-pin-down arc, defending-prep arc)
- `firm.deposition_defaults.time_budget_template.{deposition_type}` — per-type minute allocation pattern (background, theme blocks, impeachment, reserve) used to seed the Time Budget table; the skill always preserves the FRCP 7-hour default ceiling and the 30-minute reserve floor
- `firm.deposition_defaults.housekeeping_stipulations` — firm-standard list of opening housekeeping and on-the-record stipulations (reading-and-signing, FRCP 30(b)(5)(C) objection-reservation, prior-deposition history, document-review privilege-waiver inquiry, protective-order designation language)
- `firm.court_reporter_vendors` — preferred court-reporter vendor and contact, surfaced in the logistics block; same convention for `firm.videographer_vendors` and `firm.interpreter_vendors`
- `firm.work_product_designation` — the firm's standard work-product header and footer applied to every outline
- `firm.disclaimers.deposition_outline` — the firm's standard "do not produce, do not share outside the firm" language
- `firm.ethics.no_witness_substance_coaching` — non-overridable boolean asserting that the defending-prep variant must never coach the substance of answers; the skill treats this as a hard rule even if absent from `config.yml` (the skill cannot be configured to violate Rule 3.4(b) and the analogous state-bar rules)
- `firm.ethics.impeachment_chain_requires_source_location` — non-overridable boolean governing the **Impeachment-Source Traceability** hard rule defined in full in the Instructions block (taking-attorney variant only): every Impeachment Plan row and every Source A / Source B entry cites the prior statement's source location at the smallest unit the source supports (page:line, ¶N, Rog./RFA/RFP N, MM:SS, timestamp, or Bates: NNNN per the source-type table), and bare references are flagged `[[VERIFY: source location]]`. The defending-prep variant is unaffected. Treated as a hard rule even if absent from `config.yml`. This is the sixteenth non-overridable rule in the repo.
- `firm.depo_outline_save_path` — overrides the default save path `outputs/depositions/[matter-id]-[deponent-last-name]-[YYYY-MM-DD].md`
- `client.deposition_overrides.{client_id}` — per-client overrides (e.g., a client whose engagement letter requires a senior-partner sign-off on every deposition outline before service of any subpoena, or a client that has stipulated to a tighter time budget than the FRCP default in this matter)

If a key is absent from `config.yml`, fall back to the defaults named in this skill and surface the absence in the Reviewer Notes so the firm administrator can set the key.

## Example Output

The following is an abbreviated real worked example for the **taking-attorney / fact-witness** variant with prior statements available — the variant that exercises the Impeachment-Source Traceability hard rule. It is trimmed to one theme and two contradictions to keep it readable; a live outline would carry all themes.

**Sample input provided to the skill:**

> - Deposition type: Fact witness
> - Governing rules: FRCP, 7-hour limit
> - Case: Reyes v. Brightway Foods (slip-and-fall; we represent plaintiff Reyes)
> - Deponent: T. Whitfield, store manager on duty
> - Prior statements: (a) Whitfield's signed declaration, 02-Apr-2026; (b) Whitfield's recorded statement to the insurer, 11-Jan-2026; (c) incident report, Bates BW-000142
> - Key issues: notice of the spill (how long it was on the floor before the fall); inspection-log compliance
> - Goals: pin the time the spill existed; establish missed inspection; impeach on inconsistent "I cleaned it immediately" claims

**Skill output (excerpt):**

```
## Deposition Outline — T. Whitfield — Fact witness

- **Case:** Reyes v. Brightway Foods, No. [[VERIFY: case no.]]
- **Date / time / location:** [[VERIFY]] / remote
- **Governing rules / time limit:** FRCP — 7 hours, one day
- **Exhibit-numbering convention:** Plaintiff's Exhibit 1 (per firm config)
- **Prepared by:** [attorney], AI-assisted

## Deposition Goals
1. Pin the elapsed time the spill was on the floor before Reyes fell
2. Establish that the required hourly inspection was not performed in the relevant window
3. Impeach the "cleaned it immediately" claim against the prior recorded statement

## Time Budget (total < 7h; 30-min reserve)
| Section | Minutes | Notes |
|---------|---------|-------|
| Background & foundation | 20 | manager role, shift, training |
| Theme 1 — Notice / timeline | 75 | spill duration, inspection log |
| Theme 2 — Inspection compliance | 60 | Ex. 2 inspection log |
| Impeachment | 30 | recorded-statement chain |
| Reserve | 30 | |

## Topic Outline
### Topic 1: Notice — how long the spill existed
#### Background & foundation
- You were the manager on duty on the date of the incident, correct?
- Part of your job is to ensure the floors are safe for customers?

#### Substantive / document-pinning (Exhibit refs)
- I'm showing you what's marked **Ex. 1**: the incident report you signed. Is this your signature? — **Ex. 1**: Incident report (Bates BW-000142)
- The report lists the time of the fall as 3:14pm. Do you see that?
- When did you first become aware of the spill? — **Ex. 2**: Inspection log

#### Pin-down
- So between the last logged inspection and Ms. Reyes's fall, no inspection of that aisle occurred — correct?

## Exhibit List
| Ex. # | Document | Bates / source | Used in Topic(s) | Copy ready? |
|-------|----------|----------------|------------------|-------------|
| 1 | Incident report | BW-000142 | 1 | Y |
| 2 | Inspection log, date of incident | BW-000150–151 | 1, 2 | Y |

## Contradiction Analysis
### Contradiction 1 — Severity: MAJOR
- **Source A:** Recorded statement, 11-Jan-2026, 03:42 — Whitfield says he "cleaned the spill within a minute of seeing it"
- **Source B:** Decl. of Whitfield, 02-Apr-2026, ¶6 — states he "directed an associate to place a cone and clean the area"
- **Foundation / Confront / Commit sequence:**
  - Foundation: "You gave a recorded statement to the insurer on January 11, under oath that it was true and accurate?"
  - Confront: "In that statement, didn't you say you cleaned the spill yourself within a minute?"
  - Commit: "So which is correct — you cleaned it yourself, or you directed an associate?"

### Contradiction 2 — Severity: MODERATE
- **Source A:** Incident report, Bates BW-000142 — time of fall logged as 3:14pm
- **Source B:** Recorded statement, 11-Jan-2026, 05:10 — Whitfield estimates the fall happened "around 2:45"
- **Foundation / Confront / Commit sequence:** [Foundation/Confront/Commit questions as above]

## Impeachment Plan
| Anticipated denial | Prior statement to use | Source location | Exhibit # | Foundation q. | Confront q. | Commit q. |
|--------------------|------------------------|-----------------|-----------|---------------|-------------|-----------|
| "I cleaned it immediately myself" | Recorded statement vs. declaration conflict | Recorded statement, 11-Jan-2026, 03:42 / Decl. 02-Apr-2026 ¶6 | — | "You signed this declaration?" | "But you told the insurer you cleaned it yourself?" | "Which is true?" |
| "The inspection was done on time" | Inspection log gap | Bates BW-000150–151 | Ex. 2 | "This is your store's log?" | "There's no entry between 1:00 and the 3:14 fall?" | "So no inspection happened in that window?" |
| "I don't recall when I saw it" | [[VERIFY: source location — no pinned prior statement on first-awareness time; do not place in time-budgeted impeachment until located]] | [[VERIFY]] | — | — | — | — |

## Strategic Notes
- **Likely objection patterns:** form objections on the compound timeline questions — keep them short and single-fact
- **Evasion patterns to expect:** "I don't recall" on the inspection timing — have Ex. 2 staged to refresh
- **Break triggers:** call a break after the notice admissions, before launching impeachment
- **Video considerations:** the "which is true" commit on Contradiction 1 is a likely trial-replay moment

## Reviewer Notes
- **Placeholders:** case number; depo date; the unpinned "first-awareness" impeachment row (held out of the time budget per the traceability rule)
- **Open strategic calls:** whether to use Contradiction 2 (time discrepancy) live or save for trial
- **Work product designation:** Attorney work product — do not produce
```

**Why this is the target quality:** every impeachment row and contradiction source carries a located citation at the smallest unit the source supports (`03:42` for the recorded statement, `¶6` for the declaration, `BW-000150–151` for the log), so the taking attorney can find each prior statement on the source in seconds. The one denial with no pinned source is flagged `[[VERIFY: source location]]` and explicitly held out of the time-budgeted impeachment section rather than being asserted as usable impeachment material — exactly what the Impeachment-Source Traceability rule requires.
