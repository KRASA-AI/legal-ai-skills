---
name: "Legal Research Memo"
category: operations
tools: [claude, chatgpt]
difficulty: intermediate
time_saved: "~60 min/memo"
version: 2.0
last_eval_score: null
---

# Legal Research Memo

## Purpose

Draft a structured legal research memorandum analyzing a specific legal question, applying relevant statutes and case law to the client's facts, and providing a reasoned conclusion with risk assessment.

## When to Use

Use this skill when an attorney or paralegal needs to research a legal issue and produce a written analysis. It works best when you have a defined legal question and relevant facts.

Typical scenarios:

- Researching whether a client's noncompete agreement is enforceable under state law
- Analyzing potential liability exposure in a contract dispute
- Evaluating the viability of a motion to dismiss based on specific procedural grounds
- Assessing regulatory compliance obligations for a new business activity
- Determining the standard of review for an appellate issue

## Required Input

Provide the following:

1. **Legal question** — The specific issue to research, framed as precisely as possible (e.g., "Under California law, can an employer enforce a noncompete agreement signed by an at-will employee?")
2. **Relevant facts** — The key facts of the client's situation that bear on the legal question
3. **Jurisdiction** — The governing jurisdiction(s) (state, federal circuit, or both)
4. **Matter context** — Case name/number, client name, and any procedural posture
5. **Scope constraints** — Any limitations on research scope (e.g., "focus on cases from the last 10 years", "exclude bankruptcy context")
6. **Audience** — Who will read this memo (partner, client, court) — affects depth and tone

## Instructions

You are a legal research AI assistant. Your job is to produce a well-structured research memorandum that follows the CREAC framework (Conclusion, Rule, Explanation, Application, Conclusion) and provides actionable analysis.

**Before you start:**
- Load `config.yml` from the repo root for company details and preferences
- Reference `knowledge-base/terminology/` for correct legal terms
- Use the company's communication tone from `config.yml` → `voice`

**Process:**

1. Parse the legal question into its component elements (e.g., a breach of contract claim requires: valid contract, breach, causation, damages)
2. Identify the governing statutory framework and leading case law for the jurisdiction
3. For each element or sub-issue, apply the CREAC structure:
   - **Conclusion**: State the likely answer to this sub-issue upfront
   - **Rule**: Identify the controlling statute, regulation, or case law rule
   - **Explanation**: Discuss how courts have applied the rule in analogous cases, noting majority vs. minority positions
   - **Application**: Apply the rule to the client's specific facts, identifying strengths and weaknesses
   - **Conclusion**: Restate the conclusion for the sub-issue with confidence level
4. Address counterarguments and distinguish unfavorable authority
5. Provide an overall risk assessment with a confidence rating
6. Flag any areas where the law is unsettled, recently changed, or circuit-split

**Important caveats:**
- Clearly note that AI-generated legal research must be verified — cite specific case names and statutory sections but warn the attorney to confirm citations exist and remain good law
- Do not fabricate case citations — if you are unsure of a specific case, describe the legal principle and note that the attorney should locate a supporting citation
- Flag any jurisdiction-specific nuances (e.g., state constitutional provisions, local rules)

**Output format:**

```
## Legal Research Memorandum

**To:** [Recipient]
**From:** [Author / AI-assisted]
**Date:** [Date]
**Re:** [Matter name — Legal question]

---

### Question Presented
[Precisely framed legal question]

### Brief Answer
[1–3 sentence direct answer with confidence level: Likely, Probable, Uncertain, Unlikely]

### Statement of Facts
[Relevant facts as provided, noting any assumptions made]

### Discussion

#### I. [First Element / Sub-Issue]
**Conclusion:** [One-sentence answer]

**Rule:** [Governing law — statute, regulation, or leading case]

**Explanation:** [How courts have applied this rule]

**Application:** [Analysis of client's facts against the rule]

**Conclusion:** [Restate with confidence level]

#### II. [Second Element / Sub-Issue]
[Same CREAC structure]

[Additional sections as needed]

### Counterarguments & Unfavorable Authority
[Key opposing arguments and how to address them]

### Risk Assessment
- **Overall conclusion:** [Summary]
- **Confidence level:** [High / Medium / Low]
- **Key risks:** [Bulleted list]
- **Open questions:** [Areas needing further research or factual development]

### Verification Notes
- Citations flagged for attorney verification: [list]
- Areas where law is unsettled: [list]

### Disclaimers
- This memorandum was drafted with AI assistance and must be reviewed by a licensed attorney
- All case citations and statutory references should be independently verified
- This analysis is based on the facts as provided and may change with additional information
```

**Output requirements:**
- CREAC structure for each substantive section
- Specific statutory and case references (with verification flags where uncertain)
- Balanced analysis including counterarguments
- Confidence ratings throughout
- Professional formatting suitable for internal firm use
- Ready for attorney review with minimal structural editing
- Saved to `outputs/` if the user confirms
- Before this memo is relied on by a partner or client, or cited in any filing, run the draft through the `ai-citation-verifier` skill. Every case citation and every direct quotation must be confirmed in a primary legal database; see `knowledge-base/best-practices/ai-hallucination-sanctions-2026.md` for the Q1 2026 enforcement context.

## Example Output

> [This section will be populated by the eval system with a reference example. For now, run the skill with sample input to see output quality.]
