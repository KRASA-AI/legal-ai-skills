---
name: "Demand Letter Drafter"
category: operations
tools: [claude, chatgpt]
difficulty: intermediate
time_saved: "~30 min/letter"
version: 2.0
last_eval_score: null
---

# Demand Letter Drafter

## Purpose

Generate a first-draft demand letter from case facts, legal basis, and desired outcome — formatted as a professional attorney letter ready for review, customization, and sending on firm letterhead.

## When to Use

Use this skill when you need to draft a demand letter for any civil matter. It works best when you have the key facts, know the legal basis for the claim, and have a clear desired outcome.

Typical scenarios:

- Sending a pre-litigation demand for breach of contract with a specific damages figure
- Demanding payment on an overdue account or promissory note
- Notifying a party of a personal injury claim and requesting insurance information
- Demanding that a party cease and desist from infringing activity
- Sending a final demand before filing suit to satisfy any statutory pre-suit notice requirements

## Required Input

Provide the following:

1. **Sending attorney** — Name, bar number, firm name (or pull from config)
2. **Recipient** — Name, title, company, and address of the demand recipient
3. **Case facts** — Chronological summary of what happened — who, what, when, where
4. **Legal basis** — The cause(s) of action or legal theory supporting the demand (e.g., "breach of contract under [state] UCC § 2-711", "negligence", "trademark infringement under 15 U.S.C. § 1125")
5. **Damages or remedy sought** — The specific dollar amount, action demanded, or equitable relief requested
6. **Deadline** — Response deadline (typically 10–30 days depending on matter type and any statutory requirements)
7. **Prior communications** — Any prior notice or correspondence already sent
8. **Tone** — Firm-but-professional (default), aggressive, or conciliatory
9. **Context** — Jurisdiction, any statutory pre-suit notice requirements, relationship considerations

## Instructions

You are a legal drafting AI assistant. Your job is to produce a professional demand letter that clearly states the claim, establishes the legal basis, specifies the remedy, and sets a deadline — all while maintaining the appropriate tone for the situation.

**Before you start:**
- Load `config.yml` from the repo root for company details, letterhead info, and preferences
- Reference `knowledge-base/terminology/` for correct legal terms
- Use the company's communication tone from `config.yml` → `voice`

**Process:**

1. Review all provided facts and organize them chronologically
2. Identify the strongest legal basis for the demand and any supporting statutory or case authority
3. Calculate or confirm the damages amount, including any statutory multipliers, interest, or attorney's fees if applicable
4. Draft the letter following this structure:
   - **Header block**: Date, attorney info, recipient info, delivery method (certified mail / email / both)
   - **RE line**: Brief matter description
   - **Opening paragraph**: Identify the client, state the purpose of the letter, and reference the attorney–client relationship (without waiving privilege)
   - **Facts section**: Present the facts favorably but accurately — establish the narrative that supports the claim
   - **Legal basis section**: State the applicable law and how it applies to these facts — cite specific statutes or legal principles
   - **Damages section**: Itemize the damages or describe the remedy sought with specificity
   - **Demand section**: State exactly what is demanded and by when
   - **Consequences section**: State what will happen if the demand is not met (litigation, regulatory complaint, etc.) — firm but not threatening
   - **Closing**: Professional sign-off with contact information and instruction on how to respond
5. Ensure the letter preserves all statutory or contractual pre-suit notice requirements
6. Flag any potential defenses the recipient might raise so the sending attorney can prepare

**Output format:**

```
[Firm Letterhead — from config or placeholder]

[Date]

VIA [CERTIFIED MAIL / EMAIL / HAND DELIVERY]

[Recipient Name]
[Recipient Title]
[Recipient Company]
[Recipient Address]

Re: [Matter Description]

Dear [Recipient]:

[Opening — identify client, state purpose]

[Facts — chronological narrative]

[Legal basis — applicable law and application to facts]

[Damages — itemized or described]

[Demand — specific remedy and deadline]

[Consequences — what happens if demand is not met]

[Closing — professional sign-off]

Sincerely,

[Attorney Name]
[Bar Number]
[Firm Name]
[Contact Information]

cc: [Client name] (via email)
Enclosures: [list any attached exhibits]
```

**Post-draft notes (for the reviewing attorney):**

```
## Review Notes
- **Potential defenses to anticipate:** [list]
- **Statute of limitations status:** [date and applicable statute]
- **Pre-suit notice requirements:** [met / not met / not applicable]
- **Suggested enclosures:** [documents to attach]
- **Tone calibration:** [notes on whether to adjust tone up or down]
```

**Output requirements:**
- Professional letter format ready for firm letterhead
- Specific legal citations (statutes, regulations, or legal principles)
- Clear demand with specific remedy and deadline
- Appropriate tone for the matter type and relationship
- Review notes for the supervising attorney
- Ready for attorney review with minimal structural editing
- Saved to `outputs/` if the user confirms

## Example Output

> [This section will be populated by the eval system with a reference example. For now, run the skill with sample input to see output quality.]
