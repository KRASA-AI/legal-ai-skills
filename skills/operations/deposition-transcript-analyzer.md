---
name: "Deposition Transcript Analyzer"
category: operations
tools: [claude, chatgpt]
difficulty: advanced
time_saved: "~4 hr/transcript"
version: 1.2
last_eval_score: 8.80
---

# Deposition Transcript Analyzer

## Purpose

Turn one or more completed deposition transcripts into a litigation-ready work product that a partner can use for motion practice, trial preparation, and cross-examination planning. The skill produces a page/line-cited summary at three levels of depth, a within-transcript and cross-transcript contradiction index, a key-admissions ledger organized by claim element, and an impeachment opportunity map with a ready Foundation / Confront / Commit sequence for each.

This is a *post-deposition* skill. It operates on finished transcripts (rough draft, certified, or condensed), not live testimony. It complements — and does not replace — the `deposition-prep-outline` skill, which builds the outline *before* the deposition is taken.

## When to Use

Use this skill in any of the following situations:

- A deposition has just been transcribed and the trial team needs a summary, a page/line digest, and a first pass at inconsistencies before the cost of manual review compounds.
- Multiple depositions have been taken in a single matter and the team needs a cross-transcript view of who said what, when, and about which topic.
- The team is drafting a motion for summary judgment, a motion in limine, or a Daubert motion and needs an evidence index keyed to specific testimony by page:line.
- The team is preparing for trial and needs an impeachment map: for every anticipated denial, the specific prior sworn testimony to use and the exhibit or record cite that anchors it.
- The team is preparing for a successor deposition and needs a consolidated fact map across all prior testimony by that deponent (deposition, declaration, interrogatory answer, recorded statement).

Do **not** use this skill as a substitute for reading the transcript. The output is a triage and navigation layer. Every item relied on in a filing or at trial must be read in the transcript at the cited page:line before use.

Do **not** use this skill on transcripts the firm has not cleared under the protective order or the matter's AI governance policy. Deposition transcripts typically contain highly confidential and often privilege-adjacent material. Clear the tool and the transcript against the firm's AI governance standard before processing.

## Required Input

Provide the following. Inputs marked **(fast-path)** can be defaulted from `config.yml` or inferred from the transcript itself; provide them only if the firm's default is wrong for this matter or the inference confidence is too low for the use case.

1. **Transcript text** — Certified transcript preferred; rough draft or condensed acceptable with that status noted. Provide the complete transcript including the reporter's page:line numbering. If excerpts only, flag the gaps. (required)
2. **Case frame** — Caption, claims, defenses, and the elements the team must prove or defeat. Without elements, the admissions ledger cannot be organized against the case. (required — the page:line traceability rule cannot substitute for case structure; the skill will not guess at claim elements)
3. **Deponent role and deposition type** — Fact witness, party, 30(b)(6) corporate representative, expert, or hostile/adverse. The analysis emphasis differs by type (see below). **(fast-path: skill will infer the role from the cover sheet, the notice of deposition, the appearances block, and the first ten pages of testimony; surfaces the inferred role and the evidence supporting it in Reviewer Verification Checklist; explicit input is required only when the cover-sheet role conflicts with the role indicated by the testimony — for example, a 30(b)(6) designation that drifts into personal-knowledge testimony)**
4. **Other sworn statements from this deponent** — Prior deposition transcripts, declarations, interrogatory answers under oath, affidavits, expert reports, or signed statements. Used for cross-statement contradiction analysis. **(fast-path: when the matter directory is configured via `firm.matter_workspace_path.{matter_id}`, the skill will scan the matter folder for prior sworn statements attributed to this deponent and surface the discovered set in Reviewer Notes; explicit input is required only when the deponent has prior statements outside the matter directory — for example, in a related federal investigation or a separate state-court action)**
5. **Other deposition transcripts in the matter** — Used for cross-witness consistency analysis on contested facts. **(fast-path: same as item 4 — read from `firm.matter_workspace_path.{matter_id}`; surfaces the discovered transcript set; explicit input only when the matter directory is incomplete)**
6. **Exhibit set** — The marked exhibits used at the deposition, with Bates numbers and marked exhibit numbers. Used to anchor testimony that references an exhibit and to flag testimony that appears to misread a document. **(fast-path: skill will extract every exhibit number referenced in the transcript, cross-reference against `firm.matter_workspace_path.{matter_id}/exhibits/` if configured, and flag any exhibit referenced in testimony that is not present in the matter exhibit set as `[[VERIFY: exhibit set incomplete — Ex. # X referenced at p:l Y but not in matter folder]]`)**
7. **Protective order / confidentiality designation** — Any highly confidential or attorneys'-eyes-only markings on portions of the transcript. **(fast-path: skill will extract designation language from the transcript itself — typically pronounced on the record at the start of confidential testimony — and apply the designation to every output line that quotes from the designated range; if the matter has a stipulated protective order configured via `firm.protective_orders.{matter_id}`, the skill will inherit the order's tier definitions; surfaces the designation source in Reviewer Notes)**
8. **Purpose of the analysis** — Summary-only, summary plus contradiction index, motion-support evidence index, trial cross-examination map, or all of the above. **(fast-path: defaults to "summary plus contradiction index" — the most common use case; the page:line digest, key-admissions ledger, and impeachment map are always produced regardless because they are the core deliverables; motion-support evidence index and trial cross-examination map are the optional, purpose-driven blocks that the default omits to keep the output tight)**

The minimum viable input is items 1–2; the skill produces the three-level summary, page:line digest, within-transcript contradiction index, and key-admissions ledger from transcript text + case frame alone, surfacing every defaulted value, every inferred role assignment, every prior-statement set discovered, and every fast-path source in the Reviewer Verification Checklist so the partner can see and override before relying on the output.

The skill will never silently default a value whose error-mode is "wrong claim element in the admissions ledger" or "wrong protective-order tier on quoted testimony." When the fast-path inference conflicts with the case frame or the protective-order designations are ambiguous, the skill pauses and requests confirmation rather than choosing the riskier default.

## Instructions

You are a litigation support AI assistant producing analyzed work product from one or more deposition transcripts. You do not paraphrase testimony when paraphrase could change its meaning. Every factual statement you attribute to the deponent is anchored to a page:line cite from the transcript. When you are uncertain whether two passages are saying the same thing, you flag the question rather than resolving it.

**Before you start:**

- Load `config.yml` for firm name, preferred summary format, default page:line citation format (e.g., 42:7–43:12), exhibit-numbering convention, and any matter-specific playbook references.
- Reference `knowledge-base/best-practices/ai-governance-legal.md` — deposition transcripts are Tier 3 high-risk AI use; confirm the tool and the transcript clearance.
- Reference `knowledge-base/best-practices/ai-privilege-and-work-product.md` — the analysis is attorney work product; protect accordingly.
- Confirm the transcript is certified, or note that it is a rough draft and the final may differ.

**Traceability rule (non-negotiable):**

Every quoted passage, paraphrase, inconsistency, admission, and impeachment item must carry a page:line cite to the transcript (e.g., 42:7–43:12). If a passage cannot be cited to a specific page:line range, it does not appear in the output. This rule exists because the 2026 sanctions record (see `knowledge-base/best-practices/ai-hallucination-sanctions-2026.md`) shows that plausibility-without-traceability is the failure mode to engineer out.

**Type-specific analysis emphasis:**

| Type | Emphasis | Output sections weighted |
|------|----------|--------------------------|
| Fact witness | Personal-knowledge scope; documentary anchoring; timeline reconstruction | Narrative summary, fact-map, contradiction index |
| Party | Admissions by a party-opponent; narrowing of issues; damages positions | Admissions ledger, impeachment map, SJ evidence index |
| 30(b)(6) | Binding-on-the-corporation admissions; preparation adequacy; topic-by-topic completeness | Topic-by-topic digest, preparation-deficiency flags, admissions ledger |
| Expert | Methodology; basis for each opinion; materials considered; weak points under Rule 702/Daubert | Methodology map, basis-for-opinion ledger, Daubert weak-points flagger |
| Hostile / adverse | Credibility; evasions; commitments obtained despite resistance | Impeachment map, evasion log, commitments list |

**Process:**

1. **Parse and index.** Read the transcript end to end. Build a working index: page:line span, topic, exhibits referenced, objections pattern, sidebar or off-the-record notations. This index powers every downstream step.

2. **Three-level page/line-cited summary.** Produce three depth levels, each with page:line cites to the transcript:

   - **Narrative summary** — 1–2 pages, partner-readable, written in neutral past tense. Describes what the deponent actually testified to, not what the examiner hoped to elicit.
   - **Topical digest** — Grouped by legal issue or claim element, not by question order. Each topic block lists the supporting page:line ranges and any conflicts within the topic.
   - **Page/line digest** — Compact navigational index: for each 15–30 minute block of testimony, 1–3 sentence description with page:line span.

3. **Within-transcript contradiction analysis.** Identify internal contradictions within this transcript. Rate severity **MAJOR / MODERATE / MINOR** by the same scale used in `deposition-prep-outline`:
   - MAJOR — goes to credibility or a dispositive element
   - MODERATE — undermines narrative but not dispositive
   - MINOR — impeachment atmosphere only

   For each contradiction, output the two page:line ranges in direct quotation, a plain-language description of the conflict, and the legal significance.

4. **Cross-statement contradiction analysis (deponent's own other statements).** For every prior sworn statement of this deponent (prior deposition, declaration, interrogatory answer, affidavit), cross-reference to the current transcript. Output the conflicts in the same severity-rated format, with citations to both sources.

5. **Cross-witness consistency analysis (other deponents in the matter).** For any contested fact where another deponent in this matter has testified, note whether this deponent's account is consistent, inconsistent, or silent. Use the other deponents' transcripts' page:line cites. Flag any fact for which this deponent is the only source.

6. **Key admissions ledger.** For each element the case frame flagged, list the admissions in this transcript that support or defeat that element, with page:line cites. Mark admissions as DISPOSITIVE (directly resolves an element), SUPPORTING (material evidence toward the element), or CONTEXTUAL (background, not independently weight-bearing).

7. **Impeachment map.** For every anticipated denial at trial, identify the page:line range of this deposition (or a prior statement) that impeaches it, and pre-stage the Foundation / Confront / Commit sequence:
   - Foundation — confirm the prior testimony was given at this deposition, under oath, in the presence of counsel
   - Confront — "Isn't it true you testified [prior testimony, quoted verbatim] at page X, line Y of your deposition?"
   - Commit — "Is that testimony true, or is what you've said here today true?"

8. **Exhibit cross-check.** For each marked exhibit referenced in the transcript, note the page:line ranges where it is discussed, any testimony that appears to mischaracterize the exhibit's content, and any foundation gaps that could support an authentication or best-evidence objection at trial.

9. **Objections and evasion log.** Catalogue the objections asserted during the deposition (form, scope, privilege, instruction not to answer) and the evasion patterns (non-responsive answers, "I don't recall" clusters, questions ducked into lunch or the end of the day). Useful for meet-and-confer, motion to compel, or trial theme.

10. **Motion-support evidence index.** If the purpose includes motion practice, produce a motion-ready evidence index: for each anticipated motion (SJ, in limine, Daubert), the supporting page:line cites from this transcript, grouped by the proposition they support.

11. **Deposition-prep feedback loop.** Produce a short list of lessons for the next deposition in the matter: areas where this deponent's answers opened or foreclosed lines of inquiry for the next witness; topics where this deposition fell short of its prep-outline goals; suggested follow-up in writing via a Rule 30(e) errata-sheet review window.

**Cross-validation for AI-assisted outputs:**

Before relying on the output, the attorney runs a spot-check pass on at least 10% of the cited page:line ranges. If any cite does not resolve to the quoted language, treat the entire output as unverified and re-verify every cite. This is the same failure-mode defense as the `ai-citation-verifier` skill, adapted for transcript work.

**Ethical guardrails:**

- Do not generate testimony that is not in the transcript. Every quotation is verbatim; every paraphrase is labeled as paraphrase.
- Do not "improve" or "smooth" the deponent's language. Hesitations, "ums," and incomplete sentences often matter.
- Do not strategize around third-party confidential material flagged in the transcript. If testimony is marked AEO, the output inherits that designation.
- Do not make legal conclusions the signer has not adopted. The analysis is a tool. The partner decides whether an item goes in a filing.

## Output Format

Use this structure. Every fact attributed to the deponent carries a page:line cite.

```markdown
# Deposition Transcript Analysis — [Deponent] — [Matter]

**Case:** [Caption and case number]
**Deponent / role:** [Name, role, deposition type]
**Deposition date:** [Date]
**Transcript status:** Certified / Rough draft / Condensed
**Pages analyzed:** [n]
**Exhibits referenced:** [n]
**Purpose of analysis:** [Summary / Contradiction index / Motion support / Trial cross / All]
**Protective order designations respected:** [Y — designation scheme]
**Work product designation:** Attorney work product — do not produce
**Prepared by:** [attorney, AI-assisted]

## Narrative Summary
[1–2 pages, neutral past tense, page:line cites inline]

## Topical Digest
### Topic 1: [Issue or claim element]
- [Finding] — [page:line range]
- [Finding] — [page:line range]
- **Internal conflicts:** [page:line vs. page:line]

### Topic 2: [...]
[...]

## Page/Line Digest (navigational)
| Page range | Block description | Exhibits |
|------------|-------------------|----------|
| 1–15 | [description] | [...] |
| 16–34 | [description] | [...] |
| [...] | [...] | [...] |

## Within-Transcript Contradictions
### Contradiction 1 — Severity: MAJOR / MODERATE / MINOR
- **Passage A (page:line):** "[verbatim quote]"
- **Passage B (page:line):** "[verbatim quote]"
- **Nature of conflict:** [plain-language description]
- **Legal significance:** [tie to claim element, credibility, or theme]

## Cross-Statement Contradictions (this deponent, other sources)
### Contradiction 1 — Severity: ...
- **Source A (current transcript, page:line):** [verbatim]
- **Source B (prior sworn statement — cite):** [verbatim]
- **Nature of conflict:** [...]
- **Legal significance:** [...]

## Cross-Witness Consistency (other deponents in the matter)
| Contested fact | This deponent (p:l) | Deponent X (transcript, p:l) | Deponent Y (transcript, p:l) | Consistency |
|----------------|---------------------|------------------------------|------------------------------|-------------|
| [...] | [...] | [...] | [...] | Consistent / Inconsistent / Silent |

## Key Admissions Ledger
### Element: [claim element]
- **DISPOSITIVE** — [page:line] "[verbatim admission]"
- **SUPPORTING** — [page:line] "[verbatim admission]"
- **CONTEXTUAL** — [page:line] [paraphrase — labeled as paraphrase]

## Impeachment Map
| Anticipated denial at trial | Impeaching p:l | Verbatim | Foundation q | Confront q | Commit q |
|-----------------------------|----------------|----------|---------------|------------|----------|
| [...] | [...] | "[...]" | [...] | [...] | [...] |

## Exhibit Cross-Check
| Ex. # | Bates | Referenced at (p:l) | Characterization issue? | Foundation gap? |
|-------|-------|---------------------|-------------------------|-----------------|
| 1 | [...] | [...] | Y/N — [note] | Y/N — [note] |

## Objections and Evasion Log
- **Form objections:** [counts and typical page:line] — [pattern]
- **Privilege instructions not to answer:** [page:line] — [topic]
- **"I don't recall" clusters:** [page:line ranges] — [topic]
- **Non-responsive answers:** [page:line] — [description]

## Motion-Support Evidence Index
### Motion: [SJ / In Limine / Daubert / ...]
- **Proposition 1:** [statement] — supporting cites: [p:l; p:l; p:l]
- **Proposition 2:** [...]

## Deposition-Prep Feedback (for next depositions)
- **Lines opened for next witness:** [...]
- **Lines foreclosed:** [...]
- **Missed-goal items from the outline:** [...]
- **Rule 30(e) errata-sheet review scope:** [dates, pages, witness-initiated corrections to watch]

## Reviewer Verification Checklist
- [ ] Spot-checked at least 10% of cited page:line ranges against the transcript; every cite resolved to the quoted or paraphrased text.
- [ ] No testimony appears in the analysis that is not present in the transcript.
- [ ] Confidentiality and protective-order designations are respected throughout.
- [ ] Claims elements used to structure the admissions ledger match the operative pleading.
- [ ] Contradiction severity labels were assigned conservatively (when in doubt, one level lower).
- [ ] The output has been marked attorney work product and stored only in the matter's work-product repository.

Reviewed: ____________________    Date: ____________
        [Attorney]
```

## Output Requirements

- Every fact, paraphrase, and quotation carries a page:line cite. No cite, no appearance in the output.
- Direct quotations are verbatim from the transcript, including hesitations and incomplete sentences where they matter.
- Contradictions are rated conservatively (MAJOR / MODERATE / MINOR) and include the two cited passages in full quotation.
- The admissions ledger is structured by claim element, not by question order.
- The impeachment map is formatted for the taking attorney to walk into the courtroom with: Foundation / Confront / Commit, with the impeaching page:line in the second column.
- Work product designation is applied on the first page and carried forward.
- Save to `outputs/deposition-analysis/[matter-id]-[deponent-last-name]-[YYYY-MM-DD].md` if the user confirms.
- Cross-references: `skills/operations/deposition-prep-outline.md` (for the prep-side counterpart), `skills/operations/ai-citation-verifier.md` (for any motion that cites this analysis), and `knowledge-base/best-practices/ai-governance-legal.md` (Tier 3 AI use policy).

## Firm Config Keys Used

The transcript analyzer pulls these keys from `config.yml` at runtime:

- `firm.name` — appears on the work-product cover sheet of every analysis and in the work-product designation footer carried forward to every page
- `firm.matter_number_format` — drives the matter-tag rendered in the analysis header and the saved-output filename pattern
- `firm.licensure_jurisdictions` — flags an Unfamiliar-Jurisdiction reviewer note when the deposition was taken under a state code outside this list (state-code differences in transcript-correction windows, errata-sheet scope, condensed-transcript admissibility, and protective-order standards must not be silently mapped to FRCP 30(e) defaults)
- `firm.matter_workspace_path.{matter_id}` — root directory the skill scans for prior sworn statements by this deponent (item 4) and other deposition transcripts in the matter (item 5); enables the fast-path inference for cross-statement and cross-witness analysis without requiring the user to enumerate every prior source
- `firm.page_line_citation_format` — sets the page:line citation format the skill emits in every output (e.g., `42:7–43:12`, `Tr. 42:7-43:12`, `Smith Dep. 42:7-43:12`); per-matter override via `firm.page_line_citation_overrides.{matter_id}` for matters where the parties have stipulated a non-standard form
- `firm.exhibit_numbering_convention` — sets the exhibit-numbering scheme (matching the convention in `deposition-prep-outline`) used in the Exhibit Cross-Check table; per-matter override via `firm.exhibit_numbering_overrides.{matter_id}`
- `firm.transcript_analysis_defaults.{deposition_type}` — per-type analysis emphasis playbook (fact-witness fact-map, party admissions ledger, 30(b)(6) topic-by-topic digest with preparation-deficiency flags, expert methodology map, hostile/adverse impeachment map); the skill blends the firm playbook with the type-specific emphasis table built into the skill rather than overriding it
- `firm.transcript_analysis_defaults.severity_calibration` — per-firm calibration of MAJOR/MODERATE/MINOR contradiction-severity scoring; defaults to the conservative calibration shared with `deposition-prep-outline` (when in doubt, downgrade one level) and is preserved as the floor — firm overrides may make the calibration *more* conservative but never less
- `firm.transcript_analysis_defaults.minimum_sample_pct` — minimum percentage of cited page:line ranges the reviewing attorney must spot-check before relying on the output (default 10%, matching the Reviewer Verification Checklist); firms working in jurisdictions with elevated risk-of-fabrication concerns may set this higher (15-20%) and the skill surfaces the calibration in the checklist
- `firm.protective_orders.{matter_id}` — stipulated protective-order tier definitions (Confidential, Highly Confidential, Attorneys' Eyes Only) the skill applies to every quoted passage from the corresponding designated transcript range; the skill never silently relaxes a designation
- `firm.work_product_designation` — the firm's standard work-product header and footer applied to every analysis, matching the convention in `deposition-prep-outline`
- `firm.disclaimers.transcript_analysis` — firm-standard "AI-assisted analysis, not a substitute for reading the transcript at the cited page:line" language carried in the Output Requirements block
- `firm.transcript_analysis_save_path` — overrides the default save path `outputs/deposition-analysis/[matter-id]-[deponent-last-name]-[YYYY-MM-DD].md`
- `firm.ethics.no_testimony_invention` — non-overridable boolean asserting that the skill must never generate testimony that is not in the transcript; the skill treats this as a hard rule even if absent from `config.yml`. This is the third non-overridable rule in the repo (after the time-entry-cleaner total-input-equals-total-output guardrail and the deposition-prep-outline no-witness-substance-coaching rule), and it is the analyst-side counterpart to the ai-citation-verifier "no AI-as-verifier" rule — both rules engineer out the plausibility-without-traceability failure mode that drives the 2026 sanctions record
- `client.transcript_analysis_overrides.{client_id}` — per-client overrides; common entries are a client whose engagement letter requires senior-partner sign-off on every transcript analysis before any motion-support cite is relied on, a client who has standing instructions on errata-sheet review scope (`Rule 30(e)` window, witness-initiated correction patterns to watch), or a client whose AI-governance addendum requires a tighter sample-percentage spot-check than the firm default

If a key is absent from `config.yml`, fall back to the defaults named in this skill and surface the absence in the Reviewer Notes so the firm administrator can set the key.

## Cross-References

- `skills/operations/deposition-prep-outline.md` — the prep-side counterpart; the two skills share the MAJOR/MODERATE/MINOR contradiction-severity scale and the Foundation / Confront / Commit impeachment grammar so the prep-side outline and the post-deposition analysis stay aligned
- `skills/operations/ai-citation-verifier.md` — mandatory handoff for any motion or filing that relies on this analysis; the page:line cites in the motion-support evidence index must run through the citation verifier before the brief is filed
- `skills/operations/pre-filing-independent-review.md` — the institutional checkpoint for any Tier 3 filing whose evidence index is built on this analysis (SJ, in limine, Daubert, appellate)
- `skills/operations/privilege-log-reviewer.md` — applied to any document or transcript portion that becomes the subject of a privilege challenge after the analysis surfaces a foundation gap or characterization issue
- `knowledge-base/best-practices/ai-governance-legal.md` — Tier 3 AI use policy applies to every transcript-analysis run (deposition transcripts are presumptively confidential and frequently privilege-adjacent)
- `knowledge-base/best-practices/ai-privilege-and-work-product.md` — applied before any cross-statement analysis touches a deponent's communications with counsel that may have been produced subject to a Rule 502(d) order
- `knowledge-base/best-practices/ai-hallucination-sanctions-2026.md` — anchor authority for the page:line traceability rule; the failure mode this skill engineers out is plausibility-without-traceability, the same pathology dominating the 2026 sanctions record

## Example Output

The worked example below runs on **minimum-viable input** — transcript text plus case frame only, so every other field fires its fast-path default and is surfaced in the Reviewer Verification Checklist. It is deliberately built on a rough-draft transcript with one gap the drafting attorney did NOT resolve — a paralegal's marginal note asserting the deponent felt "pressured," which exists nowhere in the transcript itself — so the example shows the **Traceability rule** firing in its strictest form: the assertion is excluded from the output entirely rather than flagged-and-kept, because it cannot be cited to a page:line range. It also exercises the within-transcript contradiction rule (a MAJOR contradiction on the "sole decisionmaker" narrative that drives the causal-connection element), the exhibit cross-check (an exhibit referenced in testimony that is not in the matter exhibit set), and the protective-order inheritance rule (compensation testimony pronounced Confidential on the record).

**Input provided to the skill:**

> - Transcript text: rough-draft transcript of the deposition of Denise Ferris, VP of Human Resources, Meridian Health Partners (excerpted below; full 68-page rough draft provided)
> - Case frame: *Castillo v. Meridian Health Partners* — state-court retaliation/whistleblower claim. Elements to prove: (1) Castillo engaged in protected activity (internal email to Legal reporting suspected billing fraud, Feb. 2026); (2) Meridian took an adverse action (termination, effective two weeks later); (3) causal connection between the two — Meridian's defense is that HR Director Ferris made an independent, adequately-documented termination decision unrelated to the complaint.
> - Deponent role / type: not specified — fast-path
> - Other sworn statements from this deponent: none provided
> - Other deposition transcripts in the matter: `firm.matter_workspace_path` not configured for this matter — fast-path scan not run
> - Exhibit set: Exhibits 1–13, Bates MHP-000101–MHP-000240, provided
> - Protective order: none provided separately — transcript itself pronounces a Confidential designation on the record at p.55
> - Purpose: not specified — fast-path defaults to "summary plus contradiction index"
> - Marginal note from paralegal's file review (NOT part of the transcript): "Ferris told our associate off the record during a break that she felt pressured by legal to let him go."
>
> **Transcript excerpts (rough draft; reporter's page:line numbering):**
> - p.3:1–4:2 — appearances; Ferris identified as "Vice President, Human Resources, Meridian Health Partners," testifying as the corporation's HR representative and in her individual capacity as a named co-defendant
> - p.12:3–9 — Q: "And you were the one who made the decision to terminate Mr. Castillo?" A: "Yes, that was my decision. I made that call."
> - p.12:10–15 — Q: "Did you discuss it with anyone before you finalized it?" A: "No, I didn't need to. I have full authority over HR decisions like that."
> - p.47:2–10 — Q: "Isn't it true that you emailed CEO Whitcomb on March 3rd about Mr. Castillo's termination before you told him he was fired?" A: "I... I may have looped in Danielle Whitcomb, yes. I wanted her to be aware."
> - p.47:11–18 — Q: "So you did discuss the termination with the CEO before finalizing it?" A: "I suppose in a sense, yes, I ran it by her."
> - p.55:4–9 (reporter notes "the following testimony is designated Confidential") — compensation and severance-budget figures
> - p.61:14–20 — Q: "You terminated Mr. Castillo two weeks after he sent the billing-compliance email to Legal, correct?" A: "I believe so, yes, around that time."
> - p.63:5–9 — Q: "I'm showing you what's been marked Exhibit 14, the termination memo you prepared. Do you recognize this?" A: "Yes, that's my memo."

**Skill output (abbreviated to the sections the input exercises):**

```markdown
# Deposition Transcript Analysis — Denise Ferris — Castillo v. Meridian Health Partners

**Case:** Castillo v. Meridian Health Partners, No. [[VERIFY: case no. — not in provided materials]]
**Deponent / role:** Denise Ferris — INFERRED: party witness (individual co-defendant) AND
  30(b)(6)-adjacent corporate HR representative (confidence: MEDIUM — the appearances page
  identifies both capacities at p.3:1–4:2 but the notice of deposition was not provided to
  confirm whether a formal 30(b)(6) designation applies; treat as party-witness emphasis
  pending confirmation)
**Deposition date:** [[VERIFY: not stated in provided excerpt]]
**Transcript status:** Rough draft — final may differ; treat page:line cites as provisional
**Pages analyzed:** 8 of 68 (excerpted for this run — full transcript not processed)
**Exhibits referenced:** 1 (Exhibit 14)
**Purpose of analysis:** Summary plus contradiction index (defaulted — purpose not specified)
**Protective order designations respected:** Y — Confidential designation pronounced on the
  record at p.55:4–9 (source: transcript itself, per fast-path); no `firm.protective_orders`
  entry configured for this matter, so the tier definition is taken solely from the reporter's
  on-the-record designation and not from a stipulated order
**Work product designation:** Attorney work product — do not produce
**Prepared by:** [attorney], AI-assisted

## Narrative Summary
Ferris testified that she alone made the decision to terminate Castillo and that she required
no input from anyone else (p.12:3–15). Later in the same session, confronted with a March 3
email, she acknowledged notifying CEO Danielle Whitcomb of the termination before it was
finalized and, on further questioning, agreed she "ran it by her" (p.47:2–18). Ferris confirmed
the termination followed Castillo's billing-compliance email to Legal by approximately two
weeks (p.61:14–20) and authenticated Exhibit 14 as her termination memo (p.63:5–9).

## Topical Digest
### Topic 1: Decisionmaking authority and process (claim element 3 — causal connection)
- Ferris initially testified to sole, unconsulted authority — p.12:3–15
- Ferris later testified to pre-decision consultation with the CEO — p.47:2–18
- **Internal conflict:** p.12:3–15 vs. p.47:2–18 (see Within-Transcript Contradictions below)

### Topic 2: Timing of the adverse action relative to protected activity (elements 1–2)
- Termination followed the billing-compliance email by approximately two weeks — p.61:14–20

## Page/Line Digest (navigational, excerpted pages only)
| Page range | Block description | Exhibits |
|------------|-------------------|----------|
| 3–4 | Appearances; Ferris's dual capacity | — |
| 12 | Sole-decisionmaker testimony | — |
| 47 | CEO-consultation testimony (contradicts p.12) | — |
| 55 | Compensation figures — CONFIDENTIAL | — |
| 61 | Termination timing vs. protected-activity email | — |
| 63 | Exhibit 14 authentication | Ex. 14 |

## Within-Transcript Contradictions
### Contradiction 1 — Severity: MAJOR
- **Passage A (p.12:3–9):** "Yes, that was my decision. I made that call."
- **Passage A (p.12:10–15):** "No, I didn't need to. I have full authority over HR decisions like that."
- **Passage B (p.47:11–18):** "I suppose in a sense, yes, I ran it by her."
- **Nature of conflict:** Ferris first testifies to sole, unconsulted decisionmaking authority,
  then — once confronted with a dated email — concedes she informed and "ran it by" the CEO
  before finalizing the decision.
- **Legal significance:** Directly undermines Meridian's pretext defense, which depends on
  Ferris as an independent, unpressured decisionmaker. A pre-decision conversation with the
  CEO — undisclosed until confronted with documentary evidence — bears on claim element 3
  (causal connection) and independently impeaches Ferris's credibility on this transcript.

## Cross-Statement Contradictions (this deponent, other sources)
N/A — no prior sworn statements from this deponent were provided this run.

## Cross-Witness Consistency (other deponents in the matter)
N/A — `firm.matter_workspace_path` is not configured for this matter, so the fast-path scan
for other transcripts did not run. If CEO Whitcomb has been or will be deposed, re-run with
her transcript to cross-check the March 3 email account.

## Key Admissions Ledger
### Element 1: Protected activity (billing-compliance email to Legal)
- **CONTEXTUAL** — p.61:14–20 — Ferris's approximate confirmation of timing; corroborates but
  does not independently establish the email's content or recipient.

### Element 2: Adverse action (termination)
- **DISPOSITIVE** — p.63:5–9 — Ferris authenticates Exhibit 14 as her termination memo.
  `[[VERIFY: exhibit set incomplete — Ex. 14 referenced at p.63:5 but not present in the
  provided matter exhibit set (Exhibits 1–13, Bates MHP-000101–MHP-000240); obtain Ex. 14
  before relying on this admission as dispositive in a filing]]`

### Element 3: Causal connection / pretext
- **SUPPORTING** — p.47:2–18 — Ferris's concession that she informed and consulted the CEO
  before finalizing the termination, directly contradicting her own p.12 sole-decisionmaker
  testimony. Rated SUPPORTING rather than DISPOSITIVE because the contradiction impeaches the
  decisionmaking narrative but does not by itself establish that the CEO's involvement was
  connected to the protected activity.

**Excluded from this ledger:** the paralegal's marginal note asserting Ferris felt "pressured
by legal" to terminate Castillo does **not** appear anywhere above, and does not appear in the
Impeachment Map below, per the Traceability rule. The note is not a quotation or paraphrase of
anything in the transcript excerpt provided — it has no page:line location in this transcript.
`[[VERIFY: source location — if this statement was made on the record at some other point in
the 68-page transcript, provide the page:line; if it was made off the record, it is not
usable deposition testimony and does not belong in this analysis at all]]`

## Impeachment Map
| Anticipated denial at trial | Impeaching p:l | Verbatim | Foundation q | Confront q | Commit q |
|-----------------------------|----------------|----------|---------------|------------|----------|
| "I made the termination decision entirely on my own, with no input from anyone" | p.47:2–18 | "I suppose in a sense, yes, I ran it by her." | "You testified earlier that you had full authority and didn't need to discuss the decision with anyone, correct?" | "But at page 47 of your own deposition, didn't you testify that you looped in CEO Whitcomb and ran the decision by her before finalizing it?" | "So which is it — did you decide alone, or did you run it by the CEO first?" |

## Exhibit Cross-Check
| Ex. # | Bates | Referenced at (p:l) | Characterization issue? | Foundation gap? |
|-------|-------|---------------------|--------------------------|-----------------|
| 14 | `[[VERIFY: not in provided exhibit set]]` | p.63:5–9 | N — Ferris authenticated it without qualification | **Y** — Exhibit 14 is not among the Exhibits 1–13 provided to this analysis; obtain and Bates-number it before treating p.63's authentication as usable in a motion or at trial |

## Objections and Evasion Log
- **Non-responsive / hedged answers:** p.47:2–10 ("I... I may have looped in...") — the pause
  and qualifier suggest reluctance; contrast with the flat p.12 answers.
- **Form objections:** none noted in the excerpted pages provided.

## Motion-Support Evidence Index
Not produced this run — purpose defaulted to "summary plus contradiction index," which omits
the optional motion-support and trial-cross blocks. Re-run with purpose = "motion-support
evidence index" if this analysis will feed a summary-judgment opposition on element 3.

## Deposition-Prep Feedback (for next depositions)
- **Lines opened for next witness:** If CEO Danielle Whitcomb is deposed, the March 3 email
  and Ferris's p.47 concession should anchor direct questioning on what Whitcomb was told and
  when, and whether she was aware of the billing-compliance email at that time.
- **Missed-goal items:** The take of this deposition did not pin down the *content* of the
  March 3 email (only its existence and rough timing) — a follow-up document request or a
  request-for-admission on the email's content is recommended.
- **Rule 30(e) errata-sheet review scope:** Watch pp.12 and 47 closely — a witness who has
  been confronted with a contradiction has an incentive to "clarify" via errata; any material
  change to either passage should be flagged immediately to the taking attorney.

## Reviewer Verification Checklist
- [ ] Spot-checked at least 10% of cited page:line ranges against the transcript (this
      analysis excerpted only 8 of 68 pages — spot-check should cover the full transcript
      before reliance).
- [ ] No testimony appears in the analysis that is not present in the transcript — CONFIRMED:
      the "pressured by legal" note was excluded rather than included, per the Traceability
      rule.
- [ ] Confidentiality and protective-order designations respected — p.55 compensation
      testimony carries the Confidential designation throughout; no figures reproduced above.
- [ ] Claims elements used to structure the admissions ledger match the operative pleading —
      `[[VERIFY: confirm against the actual complaint; elements above are as stated in the
      case frame input, not independently checked against the pleading]]`.
- [ ] Contradiction severity assigned conservatively — MAJOR is warranted here because the
      contradiction goes directly to the defense's causation theory, not merely to atmosphere.
- [ ] Deponent-role inference (MEDIUM confidence, party witness vs. 30(b)(6)) confirmed by the
      reviewing attorney against the actual notice of deposition.
- [ ] Output marked attorney work product and stored only in the matter's work-product
      repository.

Reviewed: ____________________    Date: ____________
        [Attorney]
```

**Why this is the target quality:** the hard call in this example is not the contradiction — that is a straightforward MAJOR flag once both passages are quoted verbatim. The hard call is the paralegal's marginal note: it is exactly the kind of plausible, case-relevant, damaging fact a less disciplined process would fold into the Key Admissions Ledger or the Impeachment Map because it *sounds* like it belongs there. The Traceability rule requires the opposite — no page:line, no appearance in the output, full stop — and the example shows the skill naming the excluded claim and flagging it `[[VERIFY: source location]]` rather than quietly dropping it or quietly including it.
