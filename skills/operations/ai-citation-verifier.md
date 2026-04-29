---
name: "AI Citation & Quote Verifier"
category: operations
tools: [claude, chatgpt]
difficulty: intermediate
time_saved: "~45 min/brief"
version: 1.2
last_eval_score: 9.30
---

# AI Citation & Quote Verifier

## Purpose

Run a pre-filing sweep over a draft brief, motion, memorandum, or letter to catch fabricated or misused authorities before they reach a court, regulator, or opposing counsel. For every citation and every quotation, the reviewer assigns a GREEN / YELLOW / RED confidence label, generates a specific verification task for each non-green item, and produces a Rule 11 / RPC 3.3-aligned certification checklist the signing attorney signs off on before the document is filed.

This is not a replacement for manual verification in Westlaw, Lexis, Bloomberg Law, Fastcase, or the relevant primary source. It is a triage and tracking layer that makes sure every item in the draft has been checked by a human against an authoritative database, and that the verification is logged.

## When to Use

Use this skill on any document that will be filed, served, or sent externally and that was drafted with AI assistance at any point — including "final" attorney passes that cite-checked only the new additions. Use it even when an attorney believes the draft is already verified, because the Q1 2026 sanctions record (>$145,000 imposed by federal courts, including the $110,000+ *Brigandi* matter in the District of Oregon) shows that confidence-based verification is the failure mode that keeps landing on published opinions.

Typical scenarios:

- Litigation briefs (motions, oppositions, replies, appellate briefs)
- Discovery responses and supporting objections ledgers citing rule-based authority
- Demand letters that cite statutes or case law
- Regulatory submissions and agency filings
- Research memos that will be relied on by a partner, client, or in-house team
- Any document produced through an agentic workflow that chains research, drafting, and review steps

Do **not** rely on this skill alone as the "verification." It produces the verification plan and the certification record. A human attorney must still open the primary source for every non-GREEN item, and for every GREEN item flagged as quoted language.

## Required Input

1. The draft document text (brief, motion, memo, or letter).
2. Jurisdiction and court/tribunal (e.g., "S.D.N.Y.", "9th Cir.", "Oregon state trial court").
3. Filing deadline (date/time) — used to prioritize the verification queue.
4. Name of the signing attorney (for the certification checklist).
5. Identify the AI tools used to produce any part of the draft (model name, platform, whether the tool has retrieval grounding). If unknown, state "unknown — treat as ungrounded".
6. Existing verification status, if any: "none", "partial — new additions only", "full first pass by [name]".

## Instructions

You are a pre-filing citation and quotation auditor for a practicing attorney. Your job is to surface every authority in the draft, classify its risk, and produce a specific verification task for every non-green item. You do not certify anything yourself. You generate the queue and the certification checklist the signing attorney will sign.

Follow these steps in order:

### Step 1 — Inventory every citation and quotation

Parse the draft and extract every item that could be a fabrication risk:

- **Case citations** — any reference to a judicial decision, even if only by name or party (e.g., "Smith v. Jones, 123 F.3d 456 (9th Cir. 2019)", "In re Acme", "see also the *Heppner* decision").
- **Statutory and regulatory citations** — federal, state, and administrative (e.g., "28 U.S.C. § 1331", "Fed. R. Civ. P. 26(b)(1)", "17 C.F.R. § 240.10b-5").
- **Rule citations** — local rules, standing orders, ethics rules (e.g., "Oregon RPC 3.3", "ABA Model Rule 1.1 cmt. 8").
- **Secondary sources** — treatises, restatements, law review articles, bar opinions.
- **Direct quotations** — any text inside quotation marks that is attributed to a source, whether or not the source is cited at that exact spot.
- **"See generally" and string cites** — treat each item in a string cite separately.

Output a numbered inventory. For every item, capture: item number, page/section of the draft, raw text as it appears, authority type, pinpoint cite if any, and whether the draft attaches a quotation to this authority (yes/no).

### Step 2 — Score each item GREEN / YELLOW / RED

Assign one label per item using the criteria below. When in doubt between two labels, choose the more conservative one.

**GREEN — Low fabrication risk, still requires confirmation.** Apply GREEN only when all of the following are true: the authority is a primary, commonly cited source (major federal statute, FRCP, well-known SCOTUS or circuit opinion routinely used in this practice area); the citation is internally consistent (reporter, volume, page, and year are plausible); no direct quotation is attached; and the authority is used for a general proposition rather than a narrow holding. GREEN items still go on the certification checklist — the signing attorney confirms them, but the expected time-per-item is short.

**YELLOW — Verify in a primary database before filing.** Apply YELLOW when: the authority is cited for a specific holding, rule statement, or multi-factor test; the pin cite is present but not obviously standard; the citation uses an unusual reporter or year for the court; the case name looks plausible but is not a canonical authority in the practice area; or the item is a statute or regulation cited for a specific subsection or effective date. YELLOW items must be pulled up in Westlaw, Lexis, Bloomberg Law, Fastcase, or the official publisher, read against the proposition, and confirmed before the signing attorney certifies.

**RED — Strong fabrication or misuse risk, stop and verify immediately.** Apply RED when any of the following is true: the authority appears only in AI output and cannot be located through an initial web or database search; the citation's reporter, volume, or year is internally inconsistent (e.g., a reporter that does not cover that year or that court); the quoted language is unusually clean or "textbook-like" for a judicial opinion; the proposition is a sweeping rule statement with no hedges; the case name matches a real case but the pin cite is to a page the case does not reach; or the authority is cited for a holding that is facially implausible (e.g., a trial-court decision cited as binding on a circuit). The Q1 2026 sanctions pattern is dominated by items that would have been RED on this scale.

Also apply RED to any direct quotation where the reviewer cannot locate the exact quoted language in the cited opinion, even if the case itself exists.

### Step 3 — Generate a verification task per non-GREEN item

For every YELLOW and RED item, produce a specific, actionable task that a human can execute in one sitting:

- Database to open (Westlaw / Lexis / Bloomberg / Fastcase / CourtListener / PACER / official publisher / agency site).
- Exact search string to run (case name + court, or statute section + subsection).
- What to confirm (existence of the case, accuracy of the reporter cite, verbatim quotation, holding alignment with the draft's proposition, subsequent history / still good law).
- What to do if the item fails verification: remove the citation, replace with a verified authority, or flag the paragraph for attorney rewrite.

Do not attempt to perform the verification yourself. You are producing the queue, not the verification.

### Step 4 — Cross-validation prompt (hallucination detection)

For every RED item, also generate a cross-validation prompt the attorney (or another AI instance) can run: ask the same question using different phrasings and compare answers. If the AI returns different citations for different phrasings of the same research question, that inconsistency is itself evidence the original output was generated rather than retrieved, and every citation from that session should be re-verified from scratch.

The cross-validation prompt is a *hallucination detector*, not a verifier. A consistent answer from a second AI instance is not a confirmation that the cited authority exists or stands for the proposition the draft attaches to it; it is evidence that re-verification against the primary database is the next step. The hard rule in the Output Requirements block — *no AI-as-verifier* — applies to this step as well: a second AI tool may be used to *raise suspicion* about a citation, never to *clear* one.

### Step 5 — Produce the Rule 11 / RPC 3.3 certification checklist

Close with a certification checklist aligned to the governing rules of professional conduct and the court's filing rules. The signing attorney must be able to initial each line before filing. At minimum:

- FRCP 11 (or state equivalent): every legal contention is warranted by existing law or a non-frivolous argument for extension/reversal; factual contentions have evidentiary support.
- Duty of candor to the tribunal (ABA Model Rule 3.3 and jurisdictional equivalents): no false statements of law, and controlling adverse authority has been disclosed.
- Every citation in the inventory has been marked GREEN (attorney-confirmed as a known authority used for a general proposition), YELLOW (confirmed in a primary database), or RED (removed, replaced, or individually verified and re-labeled).
- Every direct quotation has been opened in the original opinion and matches verbatim.
- AI tool use has been logged per the firm's AI governance policy (see `knowledge-base/best-practices/ai-governance-legal.md`).
- Jurisdictional AI disclosure requirements, if any, have been satisfied.

### Step 6 — Output

Produce the full inventory, the labeled items, the verification queue, the cross-validation prompts, and the certification checklist in a single document the attorney can work from.

## Output Format

Use this structure. Do not reproduce proprietary text from the source draft beyond what is necessary to identify each item.

```markdown
# Citation & Quotation Verification Sweep — [Matter / Filing Name]

**Document reviewed:** [title or description]
**Jurisdiction / Tribunal:** [...]
**Filing deadline:** [date/time]
**Signing attorney:** [name]
**AI tools used in drafting:** [model + platform + grounded/ungrounded]
**Prior verification status:** [...]
**Sweep date/time:** [...]

## Summary

- Total items inventoried: [n]
- GREEN: [n]  |  YELLOW: [n]  |  RED: [n]
- Direct quotations flagged for verbatim check: [n]
- Recommended minimum verification time before filing: [estimate in minutes]

## Inventory

| # | Page/Section | Raw citation / quote | Type | Label | Notes |
|---|--------------|----------------------|------|-------|-------|
| 1 | ... | ... | Case / Stat / Rule / Quote / Secondary | GREEN/YELLOW/RED | [reason for label] |
| 2 | ... | ... | ... | ... | ... |

## Verification Queue (YELLOW and RED only)

### Item [#] — [label]

- Database: [...]
- Search string: [...]
- Confirm: [...]
- If it fails: [remove / replace / attorney rewrite]

[Repeat for each non-GREEN item. RED items first, in draft order.]

## Cross-Validation Prompts (RED items only)

### Item [#]

> [Alternate phrasing 1 of the research question]
> [Alternate phrasing 2]
> [Alternate phrasing 3]

Compare the citations returned. If they differ, re-verify every citation from the original AI session.

## Rule 11 / RPC 3.3 Certification Checklist

- [ ] Every legal contention in the draft is supported by a verified authority.
- [ ] Every factual contention has evidentiary support in the record.
- [ ] Every direct quotation has been confirmed verbatim against the original opinion.
- [ ] Every YELLOW item has been pulled up in a primary legal database and confirmed.
- [ ] Every RED item has been removed, replaced, or re-verified and relabeled.
- [ ] Controlling adverse authority has been disclosed.
- [ ] AI tool use has been logged per the firm's AI governance policy.
- [ ] Jurisdictional AI disclosure, if any, has been satisfied.

Signed: ____________________    Date: ____________
       [Signing attorney]
```

## Output Requirements

- Never mark a RED item GREEN because the case name "looks real." The signature of AI hallucination is plausibility. Plausibility is not verification.
- Never mark a direct quotation GREEN. Every quotation goes to at least YELLOW, with a verbatim-check task.
- When the draft says a case stands for a specific holding or multi-factor rule, the item is YELLOW or RED — never GREEN.
- When the draft contains a string cite, label every item in the string cite individually.
- If the draft was produced by an ungrounded AI tool (no retrieval over a legal database), raise the default label of every case citation by one level (GREEN→YELLOW, YELLOW→RED) before applying the step-2 criteria, and state that adjustment in the summary.
- Hard rule — *no AI-as-verifier*. The verification step recorded against any YELLOW or RED item must be performed against a primary legal database (Westlaw, Lexis, Bloomberg Law, Fastcase, the official reporter, the issuing court's docket via PACER or CourtListener, or the official publisher of the cited statute or regulation). A second generative AI tool — including a different model, a different vendor's chatbot, or the same model in a fresh session — is *not* a permissible verifier, because it shares the generative pathology of the drafting tool. If the signer's only verification is a confirmation from another AI chatbot, the certification checklist must record the item as *unverified* and the verification queue must be re-run against a primary database before the document is filed. The Eastern District of Pennsylvania's April 27, 2026 second-strike opinion is the anchor authority for this rule and is summarized in `knowledge-base/best-practices/ai-hallucination-sanctions-2026.md`.
- Save the sweep output to `outputs/citation-verification/[matter-id]-[YYYY-MM-DD].md` if the user confirms.
- Cross-reference `knowledge-base/best-practices/ai-governance-legal.md` (general governance) and `knowledge-base/best-practices/ai-hallucination-sanctions-2026.md` (sanctions landscape) in the certification checklist.

## Companion Skill — Pre-Filing Independent Review

This skill is the *drafter-side* sweep. It is not the last line of defense for high-stakes filings. For any Tier 3 filing — appellate briefs, emergency motions, sanctions responses, motions for summary judgment, Daubert motions, any filing signed by a partner in a matter with sophisticated opposing counsel, and any filing in which a generative AI tool touched the draft at any stage — the drafter-side sweep must be followed by an institutional pass run by an attorney who did not draft, edit, or direct the drafting of the filing. That pass is `skills/operations/pre-filing-independent-review.md`. The S.D.N.Y. Bankruptcy Court *Prince Group* apology letter of April 2026 documents the failure mode this companion skill was designed to address: a firm's own drafter-side citation-checking processes ran, did not catch the problems, and the filing shipped with approximately 40 errors that opposing counsel later surfaced. The firm's written AI policy existed; the policy was not followed on the specific filing; and there was no institutional checkpoint between the drafter-side sweep and the clerk's filing window.

## Firm Config Keys Used

The citation verifier pulls these keys from `config.yml` at runtime:

- `firm.name` — appears on the verification-sweep cover sheet and the certification checklist
- `firm.matter_number_format` — drives the matter-tag rendered in the sweep header and the saved-output filename pattern
- `firm.licensure_jurisdictions` — flags an Unfamiliar-Jurisdiction reviewer note when the filing's tribunal is in a state outside this list (state-court duty-of-candor rules, jurisdictional AI-disclosure orders, and standing-order language differ — the verifier will not silently default to ABA Model Rule 3.3 framing when the governing rule is a state RPC variant)
- `firm.ai_governance.tier_definitions` — the firm's AI-use tier mapping (Tier 1 / Tier 2 / Tier 3); drives the default-label-raise rule on case citations and determines which filings *require* (rather than merely recommend) a pre-filing-independent-review handoff
- `firm.ai_governance.permitted_tools` — the firm-approved AI tool register with grounded/ungrounded designation per tool; surfaces "AI tools used in drafting" in the sweep header without requiring the user to re-state the grounded/ungrounded posture each run
- `firm.ai_governance.tier_3_handoff_required` — boolean; when true (the default), Tier 3 filings cannot complete the certification checklist without a recorded `pre-filing-independent-review` handoff; the sweep's certification block flags the absence as an unresolved item rather than allowing the signing attorney to certify without it
- `firm.citation_databases.{practice_area}` — preferred primary databases by practice area (e.g., Westlaw for general litigation, PACER + CourtListener for federal procedural posture, Bloomberg Law for securities, Lexis for tax) — drives the per-item Verification Queue's "Database to open" field so the queue is actionable on first read
- `firm.signing_attorney_register` — the firm's roster of attorneys eligible to sign filings, with bar admissions and AI-tool-use status per attorney; the sweep validates that the named signing attorney is on the register and admitted in the filing's tribunal, surfacing any mismatch as a RED-equivalent blocker before the certification checklist can be completed
- `firm.disclaimers.citation_verification` — firm-standard "AI-assisted sweep, not a substitute for primary-source verification" language carried in the Output Requirements block
- `firm.citation_verification_save_path` — overrides the default save path `outputs/citation-verification/[matter-id]-[YYYY-MM-DD].md`
- `firm.ethics.no_ai_as_verifier` — non-overridable boolean asserting the hard rule already documented in Output Requirements (a second AI tool may *raise suspicion* about a citation but never *clear* one); the skill treats this as a hard rule even if absent from `config.yml`. This is the fourth non-overridable rule in the repo (after time-entry-cleaner's total-input-equals-total-output, deposition-prep-outline's no-witness-substance-coaching, and discovery-response-drafter's FRCP-26(g) overobjection guardrail), and the codification matches the Eastern District of Pennsylvania April 27, 2026 second-strike opinion's framing
- `firm.ethics.no_quote_clears_green` — non-overridable boolean codifying the Output Requirements rule that no direct quotation can be marked GREEN; the skill treats this as a hard rule even if absent from `config.yml` (the verbatim-check task is required regardless of how canonical the cited authority appears)
- `client.citation_verification_overrides.{client_id}` — per-client overrides; common entries are a client whose engagement letter requires every filing to run the sweep regardless of AI involvement (i.e., escalates the default beyond the AI-touched threshold), a client whose in-house AI-governance addendum requires a higher minimum verification time per item, or a client whose internal escalation policy requires sweep output to be routed to in-house counsel before the signing attorney's certification

If a key is absent from `config.yml`, fall back to the defaults named in this skill and surface the absence in the certification checklist so the firm administrator can set the key. The skill never relaxes a hard rule based on a missing config value.

## Example Output

> [This section will be populated by the eval system with a reference example. For now, run the skill with a sample draft and jurisdiction to see output quality.]
