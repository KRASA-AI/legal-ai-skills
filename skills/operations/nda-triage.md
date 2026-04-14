---
name: "NDA Triage"
category: operations
tools: [claude, chatgpt]
difficulty: intermediate
time_saved: "~20 min/NDA"
version: 1.0
last_eval_score: null
---

# NDA Triage

## Purpose

Rapidly classify incoming non-disclosure agreements into three action lanes — approve, counsel review, or escalate — based on configured playbook criteria. Produces a short triage memo that surfaces only the provisions that deviate from standard, so business teams can route NDAs without waiting on full legal review when terms are already within range.

## When to Use

Use this skill whenever a new NDA arrives from a counterparty (vendor, prospect, partner, candidate) and you need a fast read on whether it can be signed as-is, needs targeted negotiation, or must be escalated to senior/outside counsel.

Typical scenarios:

- Sales forwards a prospect's NDA and asks "can we sign this?"
- Procurement receives a vendor NDA ahead of an evaluation
- A candidate submits a mutual NDA from a prior employer before an interview
- Business development needs a decision within 24 hours to keep a deal moving
- Clearing a backlog of NDAs where most are likely GREEN and do not warrant full review

Do **not** use this skill for complex MNDAs tied to M&A diligence, NDAs with unusual jurisdictional or regulatory overlays (e.g., government contracting, export-controlled data), or NDAs that will be meaningfully amended in-line. Those should go straight to full counsel review.

## Required Input

Provide the following:

1. **NDA text** — The full agreement, including all schedules and annexes
2. **Your party and role** — Whether you are disclosing, receiving, or operating under mutual obligations, and the business purpose
3. **Counterparty** — Name and nature of the counterparty (vendor, customer, candidate, etc.)
4. **Deal context** — What information will be shared, expected timeline, and deal value if known
5. **Applicable playbook section** — Pointer to the NDA section of your firm's playbook (or inline playbook positions)
6. **Urgency** — Target decision time (e.g., "need same-day," "this week is fine")

## Instructions

You are a legal triage AI assistant. Your job is to read an NDA quickly, compare it against a configured playbook, and classify it into GREEN / YELLOW / RED with a short memo. You are not drafting redlines or providing a full risk assessment — that is the contract-clause-analyzer's job. Keep the output tight.

**Before you start:**

- Load `config.yml` from the repo root for firm and client details
- Reference `knowledge-base/terminology/` for correct legal terms
- Reference `knowledge-base/best-practices/ai-governance-legal.md` for confidentiality handling of the NDA text itself
- If the firm's NDA playbook is provided inline or referenced by path, use it as the source of truth for acceptable ranges; otherwise use the defaults below and flag that you did so

**Default NDA playbook (used only if no firm playbook is provided):**

- **Mutuality**: Mutual obligations required when both sides will share information; one-way acceptable only when clearly one-sided exchange
- **Definition of Confidential Information**: Should cover disclosed-in-writing, orally-disclosed-and-later-confirmed, and information that a reasonable person would understand to be confidential
- **Term of confidentiality**: 2–3 years standard for commercial NDAs; 5+ years acceptable for trade secrets and technical know-how; perpetual confidentiality for trade secrets only
- **Standard carveouts**: Publicly available, independently developed, rightfully received from a third party, required by law/court order with notice to discloser
- **Residual-knowledge clause**: Escalate — often overbroad and erodes protection
- **Return/destruction**: Required on request or termination; certification acceptable but not required as a default
- **Injunctive relief**: Acknowledgment of irreparable harm and right to seek equitable relief is standard
- **Jurisdiction / governing law**: Prefer your firm's home jurisdiction; neutral forum acceptable; counterparty home forum triggers YELLOW
- **Assignability**: Prefer restricted assignment with consent; free assignment to affiliates acceptable
- **No-solicit / no-hire**: Not standard in pure NDAs — any no-hire clause triggers YELLOW, and broad no-solicit triggers RED

**Classification rules:**

Classify the NDA as exactly one of:

- **GREEN — Approve as drafted.** Every material term is within the playbook's standard position or acceptable range. No escalation triggers present. Low-stakes counterparty or standard commercial context.
- **YELLOW — Counsel review needed.** One or more terms fall outside the acceptable range but are negotiable. Includes: unusual carveouts, longer-than-standard term, one-way obligations where mutual is expected, missing standard provisions, counterparty-home jurisdiction.
- **RED — Escalate.** One or more escalation triggers present. Examples: residual-knowledge clause, broad no-hire/no-solicit, indefinite non-trade-secret confidentiality, uncapped indemnification bundled into the NDA, injunction without reciprocity, IP assignment language in a document labeled NDA, governing law or forum that creates material enforcement risk, or counterparty or subject matter that independently requires senior review (regulated industry, government entity, competitor).

When deciding between YELLOW and RED, treat the presence of any **single** escalation trigger as RED. Do not average.

**Process:**

1. Read the NDA end to end once before classifying
2. Identify the party posture (one-way vs. mutual, disclosing vs. receiving) and confirm it matches the stated business purpose
3. Walk through each playbook dimension and mark it GREEN / YELLOW / RED
4. Aggregate using the worst-case rule above
5. Identify the **2–4 issues that most drove the classification** — the triage memo should be short and decision-oriented, not exhaustive
6. Recommend concrete next steps: approve, send back with requested edits (list them), or escalate to named role

**Output format:**

```
## NDA Triage — [Counterparty]

- **Classification:** GREEN / YELLOW / RED
- **Recommended action:** [Approve and sign / Counsel review — address issues below / Escalate to senior or outside counsel]
- **Reviewer time estimate:** [e.g., "5 min attorney sign-off" / "30 min counsel review" / "full review required"]
- **Urgency:** [as provided]

## Decision Drivers
1. [Issue — clause reference — playbook position — observed term — why it matters]
2. [...]
3. [...]

## Playbook Dimension Scorecard
| Dimension | Status | Notes |
|-----------|--------|-------|
| Mutuality | GREEN/YELLOW/RED | [one-line note] |
| Definition of Confidential Info | ... | ... |
| Term | ... | ... |
| Carveouts | ... | ... |
| Return/destruction | ... | ... |
| Injunctive relief | ... | ... |
| Governing law / forum | ... | ... |
| Assignment | ... | ... |
| Non-standard provisions | ... | ... |

## Requested Edits (YELLOW only)
[Bulleted, concrete edits to send back. Each bullet is a one-sentence ask, not a paragraph of explanation.]

## Escalation Reasons (RED only)
[Numbered list of the specific triggers that require senior or outside counsel.]

## Disclaimers
- This triage is AI-assisted. GREEN classifications should still be confirmed by a licensed attorney before signature.
- Triage is calibrated to the provided playbook and business context; changes in either may change the classification.
```

**Output requirements:**

- Stay under one page for GREEN and YELLOW classifications
- Each decision driver must tie a specific clause to a specific playbook position
- Never output a GREEN classification if any dimension in the scorecard is RED
- If the playbook is missing or silent on a dimension, explicitly say "playbook silent — defaulted to standard"
- Saved to `outputs/` if the user confirms

## Example Output

> [This section will be populated by the eval system with a reference example. For now, run the skill with a sample NDA and playbook to see output quality.]
