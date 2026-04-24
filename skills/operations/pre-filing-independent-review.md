---
name: "Pre-Filing Independent Review"
category: operations
tools: [claude, chatgpt]
difficulty: advanced
time_saved: "~90 min/filing"
version: 1.0
last_eval_score: null
---

# Pre-Filing Independent Review

## Purpose

Run a *second-set-of-eyes* review over a draft filing in the window after the drafting team believes it is ready and before the signing attorney clicks "file." The reviewer is deliberately not the drafter and not the signer. The reviewer reads the draft with an opposing-counsel mindset, audits the drafting team's verification record, and either issues a Release-to-File attestation or sends the draft back with a specific defect list.

This skill is the institutional counterpart to `skills/operations/ai-citation-verifier.md`. The citation verifier is what the drafter runs. The independent review is what the firm runs *on* the drafter's work product before filing. Both are designed for the same failure mode — AI-generated or AI-assisted text that slips through a competent drafter's own review — but they attack it from different angles. The citation verifier catches bad citations; the independent review catches the broader category of errors that includes statutory misquotation, mischaracterization of authority, fabricated procedural posture, and "the drafter checked the ones they remembered to check."

The skill is expressly designed around the April 2026 Big Law pattern: a motion prepared under the firm's own AI policy and routine citation-checking processes, filed by experienced partners, and later revealed by opposing counsel to contain dozens of errors — statutory misquotations, mischaracterized authorities, and at least one case that did not exist. The lesson is not that policies are useless. The lesson is that policies only bite when a person who did not draft the document is charged with checking it.

## When to Use

Use this skill before every filing in the Tier 2 or Tier 3 buckets below. Tier 1 filings may be released to file with the signing attorney's own verification sweep.

**Tier 3 — Independent Review Required.** Appellate briefs; emergency motions (TRO, preliminary injunction, stay, sealing, expedited relief); motions for summary judgment; motions in limine in trials with sophisticated adversaries; sanctions motions; responses to show-cause orders; petitions for writ or certiorari; Daubert motions; any filing signed by a partner in a matter where opposing counsel is itself a Big Law, appellate specialty boutique, or government enforcement office; any filing in which the drafter used a generative AI tool at any point in its production.

**Tier 2 — Independent Review Recommended.** Non-emergency motions with briefing; discovery motions that cite authority; demand letters where the recipient is represented; regulatory submissions; opinion letters and formal client memoranda that may be relied on by third parties.

**Tier 1 — Signing Attorney Self-Sweep.** Routine status reports; stipulated orders that cite only the FRCP or local rules; proposed orders that quote the motion verbatim; cover letters that do not themselves cite authority; transmittal correspondence. The signing attorney runs `ai-citation-verifier` and signs.

If the matter is designated high-stakes, high-visibility, or opposing-counsel-sophisticated by the matter partner, escalate by one tier.

## Required Input

1. The final draft of the filing (brief, motion, letter, or petition) — marked as "Ready for Independent Review" by the drafting team.
2. The drafting team's completed `ai-citation-verifier` output, with the Rule 11 / RPC 3.3 certification checklist partially executed (GREEN items confirmed; YELLOW items verified with database records; RED items resolved).
3. Matter metadata: matter number, signing attorney, drafting attorney(s), court/tribunal, filing deadline, opposing counsel firm name, stakes classification (Tier 1/2/3).
4. The chain-of-custody log for the draft: which AI tools were used at which stage, by whom, over which workspace or account, and whether the tool was grounded (retrieval) or ungrounded.
5. The firm's AI governance policy reference and any court-specific standing order on AI disclosure.
6. Reviewer identity: name, role, relationship to the matter. The reviewer must be an attorney who has not drafted, edited, or directed the drafting of this filing.

If any of items 1–5 is missing, stop and return to the drafting team for completion. Do not proceed with an independent review on an incomplete verification record.

## Instructions

You are the firm's pre-filing independent reviewer. You did not draft this filing and you are not signing it. Your job is to read the draft adversarially — as the best opposing counsel in the case would read it — and to audit the drafting team's verification record for gaps. You produce either a Release-to-File attestation or a defect list that sends the draft back to the drafting team with specific items to fix.

Follow the steps in order. Do not shortcut the chain-of-custody review; the April 2026 Big Law failure pattern is that the verification record *looked* complete and was not.

### Step 1 — Chain-of-custody audit

Before reading the draft substantively, audit the record of how the draft was produced. Confirm, item by item:

- The drafting team identified every AI tool that touched the draft at any stage — including research tools that surfaced authorities the drafter then used, not only drafting tools.
- Each tool is on the firm's approved list for the stakes tier of this filing. Consumer AI tools (no enterprise agreement, inputs may be used for training, no contractual confidentiality) are disqualifying for Tier 2 and Tier 3 filings.
- The `ai-citation-verifier` output exists, is complete, covers the *final* draft (not a prior draft), and shows every YELLOW and RED item resolved.
- The Rule 11 / RPC 3.3 certification checklist has been initialed by the drafting attorney and is ready for the signing attorney.
- No AI tool was used at a stage that the firm's policy prohibits for this filing type (e.g., no consumer tools on appellate briefs; no ungrounded tools on citations of first impression).

If any of these fail, output a Chain-of-Custody Defect record and stop. Do not proceed to substantive review until custody is clean. The S&C lesson is that custody gaps are the precursor to the visible errors in the filing.

### Step 2 — Opposing-counsel mindset reading

Read the draft from the perspective of the smartest, most motivated attorney on the other side — the kind who will file a motion for sanctions if they find anything wrong. For every paragraph, ask:

- Would a careful adversary pull this citation to verify it?
- Would an adversary pull this statutory quote to compare to the U.S.C. or state code?
- Is the procedural characterization of this case ("the district court held …", "the Supreme Court adopted the three-part test …") accurate against the actual opinion?
- Is the factual assertion about the record ("the witness testified that …") citable to a specific page in the transcript or exhibit?
- Does the argument rest on a case's *dicta* while implying it is a *holding*? Does it rest on a concurrence or dissent while implying it is the majority?
- Is a "see generally" citation doing work that only a direct-support citation should do?
- Is there a string cite in which any one member would not survive a direct-support read?

Flag every item that an adversary would pull. These flags become the independent reviewer's verification queue. The verification queue overlaps with, but is larger than, the drafter's `ai-citation-verifier` queue; it includes statutory text, factual assertions, and case characterizations, not just citations and quotations.

### Step 3 — Four-category error sweep

Classify the flags from Step 2 into the four 2026 error categories:

**A. Citation errors.** Case name, reporter, volume, page, year, court, pin cite. Already covered by the drafter's citation-verifier output; the reviewer spot-checks 20% of GREEN items and 100% of YELLOW and RED items against the drafter's database records.

**B. Quotation errors.** Direct quotation does not match the source verbatim; quotation marks placed around paraphrased text; ellipsis hiding material qualifying language; bracketed text introducing meaning not in the original. The Oregon Court of Appeals tariff schedule and the 5th Circuit *Fletcher* sanction both treat fabricated quotations as more serious than fabricated citations; quotation errors carry a higher weight in the reviewer's defect list.

**C. Characterization errors.** The draft says the court held X; the court actually held Y. Holding versus dicta. Majority versus concurrence or dissent. Persuasive authority described as binding. Circuit split described as settled law. Procedural posture misdescribed (e.g., a reversal described as an affirmance). These are the hardest errors to catch because the citation itself is correct; only reading the opinion catches them.

**D. Statutory and rule errors.** Statutory text does not match the U.S.C., state code, or regulation. Subsection citation does not exist. Effective date wrong. Rule text paraphrased where a direct quote is required. Local rule or standing order cited to a version that has been superseded.

For each flagged item, assign the error category, cite the specific passage in the draft, and cite the specific primary source the reviewer pulled (or the specific database record from the drafter's verification).

### Step 4 — Adverse-authority check

Independent reviewers are disproportionately effective at catching missing adverse authority because they did not fall in love with the argument. Ask, paragraph by paragraph:

- Is there a controlling adverse authority in this jurisdiction the draft has not disclosed? (ABA Model Rule 3.3(a)(2) and state equivalents require disclosure.)
- Is there a recent circuit or state supreme court decision that cuts against the draft's framing that the draft should distinguish rather than ignore?
- Is there a pending circuit split the draft acknowledges or papers over?

The adverse-authority check is separate from the citation verifier's scope; the citation verifier confirms that cited authorities exist and support what they are cited for, but it does not ask whether *un-cited* authorities should have been cited.

### Step 5 — AI-disclosure and standing-order check

Confirm that the draft's AI-disclosure posture aligns with:

- The court or tribunal's standing order on AI use, if any. If the court requires an affirmative certification, confirm the draft includes it in the form the standing order prescribes.
- The firm's own AI governance policy, including any internal disclosure requirements.
- Any client-specific AI policy embedded in the engagement letter.

If the court has a standing order and the draft does not have the certification, that is a defect regardless of whether citations are correct.

### Step 6 — Release-to-File attestation *or* Defect List

Produce one of two outputs, never both.

**Release-to-File attestation** — issued only when:

- Chain-of-custody is clean.
- No citation, quotation, characterization, or statutory error remains.
- Adverse authority has been disclosed where required.
- AI disclosure posture is correct.
- The signing attorney's certification checklist is ready.

Output includes: reviewer name, reviewer relationship to matter ("no drafting or direction on this filing"), time of review, list of spot-checked items, and an attestation sentence the signing attorney can rely on.

**Defect List** — issued whenever *any* Step 1–5 item fails. Output includes:

- Defects grouped by category (chain-of-custody, citations, quotations, characterizations, statutory/rule, adverse authority, AI disclosure).
- For each defect: exact location in the draft, specific issue, specific remediation (replace with verified cite, delete the passage, rewrite the characterization, add the disclosure certification, etc.).
- A re-review requirement: once defects are fixed, the drafting team resubmits and the same reviewer (not a new one) closes the loop.

Never issue a "Release-to-File with minor defects" attestation. The defect list either exists or does not.

## Output Format

Use this structure. Do not reproduce proprietary text from the source draft beyond what is necessary to identify each item.

```markdown
# Pre-Filing Independent Review — [Matter / Filing Name]

**Document reviewed:** [title or description]
**Signing attorney:** [name]
**Drafting attorney(s):** [names]
**Reviewer:** [name] — no drafting or direction on this filing
**Court / Tribunal:** [...]
**Opposing counsel firm:** [...]
**Stakes classification:** Tier [1 | 2 | 3]
**Filing deadline:** [date/time]
**Review time:** [...]
**AI tools that touched the draft:** [list from chain-of-custody log]
**Court standing order on AI disclosure:** [yes/no — text if yes]

## Verdict

[ ] RELEASE-TO-FILE
[ ] DEFECT LIST — drafting team to resolve and resubmit

---

## Chain-of-Custody Audit

| Check | Pass/Fail | Notes |
|-------|-----------|-------|
| Every AI tool identified (research + drafting stages) | | |
| All tools on firm-approved list for this tier | | |
| No consumer AI tools on Tier 2/3 filings | | |
| `ai-citation-verifier` output covers final draft | | |
| All YELLOW and RED items resolved | | |
| Drafting attorney initialed Rule 11 / RPC 3.3 checklist | | |

## Opposing-Counsel-Mindset Flags

| # | Location | Flag type | Passage | Reviewer action |
|---|----------|-----------|---------|-----------------|
| 1 | [page:line] | Citation / Quotation / Characterization / Statutory / Adverse-auth / Disclosure | [brief excerpt] | [verified / defect — defect ID] |

## Four-Category Error Sweep

### A. Citations (20% GREEN spot-check + 100% YELLOW/RED)
- Spot-checked: [count]
- Defects: [count]

### B. Quotations (100% verbatim check)
- Checked: [count]
- Defects: [count]

### C. Characterizations (holding vs dicta, majority vs concurrence, procedural posture)
- Checked: [count]
- Defects: [count]

### D. Statutory and rule text (verbatim vs code and rule text)
- Checked: [count]
- Defects: [count]

## Adverse-Authority Check

- Controlling adverse authority surfaced: [list or "none found"]
- Recent adverse authority not cited: [list or "none found"]
- Disclosure required by Model Rule 3.3(a)(2) / state equivalent: [yes/no — if yes, disclosure status]

## AI-Disclosure and Standing-Order Check

- Court standing order: [cite or "none"]
- Draft certification language present: [yes/no]
- Firm policy AI-disclosure language present: [yes/no]
- Client AI policy disclosure: [yes/no or "n/a"]

---

## Defect List (if verdict is DEFECT LIST)

### Defect [#] — [category]

- **Location in draft:** [page:section or paragraph reference]
- **Issue:** [specific description]
- **Primary source (what the authority actually says / what the code actually says):** [citation + quoted text]
- **Remediation:** [replace with verified cite / delete passage / rewrite characterization / add disclosure certification]

[Repeat for every defect. Order: chain-of-custody first, then statutory, quotations, characterizations, citations, adverse authority, disclosure.]

### Re-review requirement

- Same reviewer will close the loop when the drafting team resubmits.
- Time estimate for defect resolution: [...]

---

## Release-to-File Attestation (if verdict is RELEASE-TO-FILE)

> I, [reviewer name], have conducted an independent review of the above filing. I did not draft, edit, or direct the drafting of this filing. I audited the chain-of-custody log, spot-checked citations, verified every quotation verbatim, verified statutory and rule text against the code, reviewed for mischaracterization of authority, reviewed for undisclosed controlling adverse authority, and confirmed the AI-disclosure posture against the court's standing order and the firm's governance policy. I found no defects requiring return to the drafting team. This filing is released to the signing attorney for Rule 11 / RPC 3.3 certification and filing.
>
> Signed: ______________________    Date / Time: ______________
>       [Reviewer]
>
> Received: ____________________    Date / Time: ______________
>         [Signing attorney]

```

## Output Requirements

- Never issue a Release-to-File attestation with open defects. The defect list exists or the attestation exists; there is no middle output.
- Never let the drafter, a co-drafter, or the signing attorney function as the independent reviewer. The independent reviewer is institutionally independent of the drafting record.
- Never substitute a spot-check of the drafter's citation verifier output for running the four-category error sweep. The S&C pattern is that the citation verifier *was* run on the drafting team's side and the filing still shipped with 40+ errors because the reviewer-side sweep did not happen.
- Never treat chain-of-custody gaps as minor. Custody gaps are the leading indicator of downstream error; they warrant an immediate return to the drafting team.
- Never release an appellate brief, emergency motion, or sanctions response for filing without this independent-review pass, even if the deadline is tight. The 5th Circuit *Fletcher v. Experian* sanction and the 6th Circuit *Whiting v. City of Athens* $30,000 aggregate sanction both involved filings where the signing attorney believed their own review was sufficient and the circuit court disagreed.
- Save the review output to `outputs/pre-filing-review/[matter-id]-[filing-slug]-[YYYY-MM-DD].md`.
- Cross-reference: `skills/operations/ai-citation-verifier.md` (drafter-side sweep), `knowledge-base/best-practices/ai-hallucination-sanctions-2026.md` (enforcement landscape), `knowledge-base/best-practices/ai-governance-legal.md` (tiered governance), `knowledge-base/best-practices/ai-privilege-and-work-product.md` (consumer-vs-enterprise AI tool governance).

## Firm Config Keys Used

The reviewer pulls these keys from `config.yml` at runtime:

- `firm.ai_tools.approved_list` — approved tools by stakes tier
- `firm.ai_tools.consumer_tool_block_tiers` — tiers on which consumer AI tools are disqualifying
- `firm.filings.independent_review_required_tiers` — tiers at which this skill is mandatory
- `firm.filings.opposing_counsel_sophistication_override` — if true, any filing against the listed opposing firms escalates by one tier
- `matter.tribunal_standing_order_id` — court standing order on AI disclosure, if any
- `engagement_letter.ai_disclosure_clause` — matter-specific AI disclosure text the client has agreed to

## Example Output

> [This section will be populated by the eval system with a reference example. For now, run the skill with a sample draft, verification record, and matter metadata to see output quality.]
