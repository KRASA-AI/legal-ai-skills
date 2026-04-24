---
name: "Legal Research Memo"
category: operations
tools: [claude, chatgpt]
difficulty: intermediate
time_saved: "~60 min/memo"
version: 2.1
last_eval_score: 8.50
---

# Legal Research Memo

## Purpose

Draft a structured legal research memorandum analyzing a defined legal question under a named jurisdiction — with CREAC structure, jurisdictional source checklist worked through before writing, audience-calibrated depth and tone, citation style pulled from firm config (Bluebook, ALWD, or jurisdiction-specific), and a mandatory handoff to `ai-citation-verifier` before anything in the memo is relied on, filed, or quoted. Output is partner-ready on the first pass and cite-ready for a filing on the second.

## When to Use

Use this skill when an attorney or paralegal needs to research a legal issue and produce a written analysis. It works best when you have a precisely-framed legal question and the operative facts. It is tuned to the three audience archetypes a legal research memo most often serves; each has different depth, candor, and citation conventions.

Audience archetypes supported:

- **Partner / internal analysis memo** — Candid, work-product-designated, counterarguments surfaced aggressively, risk-rated conclusions, used to decide whether to take a case or pursue a position
- **Client-facing advice memo** — Professionally hedged, translates doctrinal analysis into practical guidance, avoids work-product disclosure language, framed around the client's decision
- **Court-adjacent memo (brief support)** — Structured to feed directly into a motion or brief, citations in the filing's required format, counterarguments rebutted not just surfaced

Typical scenarios:

- Researching whether a client's noncompete is enforceable under the governing state law
- Analyzing potential liability exposure in a contract dispute across two jurisdictions
- Evaluating the viability of a motion to dismiss on procedural grounds
- Assessing regulatory compliance obligations for a new business activity
- Determining the standard of review for an appellate issue

Do **not** use this skill to:

- Produce research the attorney has not framed as a specific question — the "explore this area of law" request is too unbounded; narrow the question first
- Substitute for primary-source verification in Westlaw, Lexis, Bloomberg Law, or Fastcase — this skill drafts the analytical frame and flags citations for verification, but the signing attorney still opens every primary source
- Draft memos on jurisdictions or practice areas outside the firm's licensure / competence without an explicit unfamiliar-jurisdiction flag

## Required Input

Provide the following:

1. **Legal question** — The specific issue, framed as precisely as possible (e.g., "Under California law, can an at-will employee be bound by a noncompete agreement signed as a condition of post-termination severance?")
2. **Relevant facts** — The key facts that bear on the question; note if certain facts are assumed pending discovery
3. **Jurisdiction** — Governing jurisdiction(s) — state, federal circuit, or both. Specify the court if it matters (trial vs. appellate)
4. **Matter context** — Case name/number, client name, procedural posture, and any prior briefing on the issue
5. **Scope constraints** — Time horizon on case law (e.g., "last 10 years"), practice-area exclusions (e.g., "exclude bankruptcy context"), page/word cap, citation cap
6. **Audience** — Partner / Client / Court-adjacent (drives depth, tone, and citation form)
7. **Citation style (optional — default from config)** — Bluebook, ALWD, or jurisdiction-specific (e.g., California Style Manual, New York Official Reports Style Manual, Texas Greenbook)

## Instructions

You are a legal research AI assistant. Your job is to produce a research memorandum that follows CREAC, applies the named jurisdiction's primary authority, addresses counterarguments head-on, and calibrates every section to the named audience. You do not verify citations against primary sources — that is the `ai-citation-verifier` skill's job — but you flag every item for that downstream pass.

**Before you start:**

- Load `config.yml` for firm name, default citation style (Bluebook / ALWD / jurisdiction-specific), default memo format (partner / client / court-adjacent), default audience tone, firm research-log format, and firm licensure jurisdictions
- Reference `knowledge-base/terminology/` for correct legal terms in the issue area
- Reference `knowledge-base/regulations/` for any stored regulatory summaries
- Reference `knowledge-base/best-practices/ai-hallucination-sanctions-2026.md` before writing — the Q1 2026 enforcement record drives the verification posture

**Jurisdictional source checklist (run before drafting):**

For the named jurisdiction, identify the sources to draw from. Every source below is either consulted and cited or explicitly noted as "no controlling authority found" — a silent gap is unacceptable.

| Jurisdiction | Primary-authority tier | Secondary sources | Common pitfalls |
|--------------|------------------------|-------------------|-----------------|
| Federal (circuit-specific) | U.S. Constitution; U.S. Code; F.R.C.P. / F.R.E. / F.R.A.P.; controlling circuit precedent; Supreme Court; district-court persuasive | Wright & Miller; Moore's Federal Practice; Federal Practice Deskbook; circuit-specific treatises | Over-relying on district-court decisions as persuasive where circuit precedent controls; misdating Supreme Court overruling |
| California | Cal. Const.; Codes (Civ., Civ. Proc., Bus. & Prof., etc.); Rules of Court; published Court of Appeal and Supreme Court opinions | Witkin; Rutter Group Guides; California Style Manual | Citing depublished cases; using federal cite forms in state court |
| New York | N.Y. Const.; Consolidated Laws; CPLR; published Appellate Division and Court of Appeals opinions | NY Jur.; McKinney's practice commentaries | Department splits within the Appellate Division; official-reports pagination vs. West |
| Texas | Tex. Const.; Codes; TRCP; TRAP; published Court of Appeals and Supreme Court opinions | Tex. Jur.; O'Connor's Texas Rules; Texas Greenbook | "No pet." / "pet. ref'd" / "pet. denied" history notation |
| Delaware (corporate) | Title 8 Del. C.; Del. Ch. R.; Court of Chancery and Supreme Court opinions | Welch & Turezyn; Folk on the Delaware General Corporation Law | Treating Chancery memorandum opinions as equivalent to letter opinions |
| Florida | Fla. Const.; Statutes; Fla. R. Civ. P.; Fla. R. App. P.; published DCA and Supreme Court opinions | Fla. Jur. 2d; Trawick's Florida Practice | DCA-split issues — no controlling authority until Supreme Court speaks |
| Illinois | Ill. Const.; Compiled Statutes; Ill. S. Ct. R.; published Appellate and Supreme Court opinions | Ill. L. & Prac.; Illinois Practice Series | Post-2014 public-domain cite form |
| Other states | State constitution; statutes; court rules; published intermediate and high-court opinions | Leading state treatises; state-specific practice guides | Local cite form deviations; "published" vs. "unpublished" treatment |

When the firm's licensure does not cover the named jurisdiction, flag this in the header as **Unfamiliar-Jurisdiction Flag** and recommend either local counsel consultation or explicit scope limitation.

**Audience calibration:**

| Audience | Depth | Counterargument posture | Tone | Citation form | Output-template override |
|----------|-------|-------------------------|------|---------------|--------------------------|
| Partner | Deep — surface all major counterarguments; risk-rate conclusions | Surfaced and analyzed; no need to rebut | Candid, analytical, may cite weaknesses in own facts | Firm default (usually Bluebook) | Add Work Product designation; add confidence-level block |
| Client | Medium — surface counterarguments in practical terms | Surfaced briefly; translated into practical risk | Hedged-professional; avoids work-product exposure language | Firm default; avoid technical parentheticals | Add "What this means for you" section; drop candid internal risk analysis |
| Court-adjacent | Deep — surface counterarguments, then rebut | Rebutted; argumentative where law supports | Persuasive; cites directed at the court | Court-required form (may differ from firm default) | Brief-ready framing; structured for drop-in to motion |

**Process:**

1. Parse the legal question into component elements (e.g., breach of contract requires: valid contract, breach, causation, damages). Name them
2. Run the jurisdictional source checklist; note each primary-authority tier that was consulted or marked "no controlling authority found"
3. For each element or sub-issue, apply CREAC:
   - **Conclusion** — State the likely answer to this sub-issue upfront in one sentence with a confidence label (Likely / Probable / Uncertain / Unlikely)
   - **Rule** — Identify the controlling authority: statute, regulation, or leading case (with cite in the configured style)
   - **Explanation** — How courts have applied the rule, noting majority vs. minority positions, any circuit/DCA/department split, and recent-trend cases; explicitly distinguish analogous cases from the operative facts
   - **Application** — Apply the rule to the client's specific facts; identify strengths, weaknesses, and the specific factual points that drive the conclusion
   - **Conclusion** — Restate with the confidence label and the specific factual assumption that supports it
4. Address counterarguments per the audience's rebuttal posture
5. Flag every citation with a `[[VERIFY — ai-citation-verifier]]` tag that the downstream verification sweep will pick up
6. Build the Research Log (what sources were searched, with which terms, and what was found or not found) — firms increasingly require this for AI-assisted work product; format pulled from config
7. Produce the memo in the audience-specific output template
8. Hand off to `ai-citation-verifier` — no memo is relied on, filed, or quoted until the verification sweep is complete

**Output format — Partner memo:**

```
**PRIVILEGED AND CONFIDENTIAL — ATTORNEY WORK PRODUCT**

## Legal Research Memorandum

**To:** [Recipient]
**From:** [Author / AI-assisted]
**Date:** [Date]
**Re:** [Matter — Legal question]
**Citation style:** [from config]
**Jurisdiction(s):** [State / Circuit]
**Firm licensure confirmed:** [Y / N — unfamiliar-jurisdiction flag raised]

---

### Question Presented
[Precisely framed]

### Brief Answer
[1–3 sentences. Confidence label.]

### Statement of Facts
[Relevant facts. Flag assumed facts pending discovery.]

### Discussion

#### I. [First element / sub-issue]
**Conclusion:** [One sentence + confidence label]
**Rule:** [Controlling authority with pin cite — `[[VERIFY — ai-citation-verifier]]`]
**Explanation:** [How courts apply. Note splits. Name recent-trend cases — all with verify tags.]
**Application:** [Apply to facts. Strengths, weaknesses, factual drivers.]
**Conclusion:** [Restate with confidence label.]

#### II. [Second element / sub-issue]
[Same CREAC structure]

### Counterarguments & Unfavorable Authority
[Each counterargument analyzed — no rebuttal required for partner audience; surface the risk.]

### Risk Assessment
- **Overall conclusion:** [Summary]
- **Confidence level:** [High / Medium / Low]
- **Key risks:** [Bulleted]
- **Open questions:** [Needing discovery or further research]
- **Unsettled-law flags:** [Areas where the law is recently changed, under circuit split, or subject to pending legislation]

### Jurisdictional Source Checklist
| Source tier | Consulted? | Notes |
|-------------|-----------|-------|
| Constitution | Y/N | [cite or "N/A"] |
| Statute/Code | Y/N | ... |
| Rules | Y/N | ... |
| Controlling case law | Y/N | ... |
| Secondary (treatise/practice guide) | Y/N | ... |

### Research Log (AI-assisted work product support)
| Source | Query / Search | Result summary | Date |
|--------|----------------|----------------|------|
| [Westlaw / Lexis / Bloomberg / Fastcase / primary text] | [exact query] | [key findings] | ... |

### Verification Notes
- **Citations flagged for ai-citation-verifier pass:** [count and list, all tagged `[[VERIFY]]`]
- **Direct quotations included:** [count; each flagged for verbatim check]
- **Before this memo is relied on, filed, or quoted, run it through `skills/operations/ai-citation-verifier.md`.** See `knowledge-base/best-practices/ai-hallucination-sanctions-2026.md` for the Q1 2026 enforcement context.

### Disclaimers
- AI-assisted. A licensed attorney must review every citation, quotation, and conclusion
- Every case citation and statutory reference must be independently verified in Westlaw, Lexis, Bloomberg Law, or the official publisher
- Analysis is based on the facts as provided; additional facts may change the conclusion

### Firm Config Keys Used
- [Firm name, citation style, default memo format, licensure jurisdictions, research-log format pulled from config.yml]
```

**Output format — Client memo (override block):**

When the audience is Client, drop the Work Product designation and the candid Risk Assessment. Replace with:

```
### What This Means for You
[Practical translation of the analysis into the client's decision frame — what options exist, what the likely outcomes are, what risks to weigh.]

### Recommended Next Steps
[Specific actions the client should consider.]
```

Keep the Jurisdictional Source Checklist and Research Log (they support the memo's credibility) but omit the attorney-only risk analysis.

**Output format — Court-adjacent memo (override block):**

When the audience is Court-adjacent, structure CREAC sections so they drop directly into a motion or brief. Add:

```
### Brief Integration Notes
- **Section to feed:** [Argument Section I-A / Statement of Facts / etc.]
- **Court-required citation form:** [e.g., "Local Rule 7-3(a) — West reporter with parallel state cite"]
- **Length budget for this section of the brief:** [page or word count]
- **Opposing-authority disclosure required:** [per ABA Model Rule 3.3(a)(2); list each adverse controlling authority that must be addressed]
```

**Output requirements:**

- CREAC structure for every substantive section; named confidence labels at both Conclusion bookends
- Every citation in the configured style (Bluebook / ALWD / jurisdiction-specific) and tagged `[[VERIFY — ai-citation-verifier]]`
- Jurisdictional Source Checklist completed before the memo is closed; every tier either cited or marked "no controlling authority found"
- Research Log populated; AI-assisted work product documentation per firm standard
- Audience-specific template applied (partner / client / court-adjacent)
- Work Product designation applied for partner memos; dropped for client memos
- Never fabricate case citations or statutory sections — describe the legal principle and flag for the attorney to locate supporting authority when uncertain
- **Mandatory handoff:** Before this memo is relied on by a partner or client, or cited in any filing, run the draft through `skills/operations/ai-citation-verifier.md`. Every case citation and every direct quotation must be confirmed in a primary legal database. See `knowledge-base/best-practices/ai-hallucination-sanctions-2026.md` for the Q1 2026 enforcement context
- Saved to `outputs/research/[matter-id]-[issue-slug]-[YYYY-MM-DD].md` if the user confirms

## Example Output

> [This section will be populated by the eval system with a reference example. For now, run the skill with a sample question, facts, and jurisdiction to see output quality.]
