---
name: "Deposition Prep Outline"
category: operations
tools: [claude, chatgpt]
difficulty: intermediate
time_saved: "~45 min/depo"
version: 1.2
last_eval_score: 8.10
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
- **Source A:** [citation, page:line if transcript]
- **Source B:** [citation]
- **Foundation / Confront / Commit sequence:** [questions]

### Contradiction 2
[...]

## Impeachment Plan
| Anticipated denial | Prior statement to use | Exhibit # | Foundation q. | Confront q. | Commit q. |
|--------------------|------------------------|-----------|---------------|-------------|-----------|
| ... | ... | ... | ... | ... | ... |

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

## Example Output

> [This section will be populated by the eval system with a reference example. For now, run the skill with sample documents and prior statements to see output quality.]
