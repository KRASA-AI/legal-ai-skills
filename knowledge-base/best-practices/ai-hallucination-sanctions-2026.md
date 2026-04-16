# AI Hallucination Sanctions — 2026 Landscape

## Overview

By April 2026, U.S. federal and state courts have moved decisively past the "novelty" phase of AI-citation sanctions and into a structured enforcement posture. Q1 2026 alone produced more than $145,000 in sanctions against attorneys for filings that contained AI-fabricated citations or quotations. The pattern is consistent across jurisdictions: the courts are not punishing the use of AI. They are punishing the failure to verify AI output against primary authority before filing.

This knowledge-base entry summarizes the 2026 landscape as a design reference for the skills in this repo — particularly the `ai-citation-verifier` skill and any skill that produces text that will be filed, served, or sent externally.

## Anchor Cases and Benchmarks

**District of Oregon — April 4, 2026 — the *Brigandi* matter.** U.S. Magistrate Judge Mark Clarke entered an opinion imposing $96,000 in direct sanctions against a pro bono attorney, with total monetary penalties exceeding $110,000 once opposing counsel's fees were added. Three filings contained 23 fabricated citations and 8 fabricated quotations. The court relied on Federal Rule of Civil Procedure 11 and Oregon RPC 3.3 (duty of candor to the tribunal), struck the errant briefs without leave to refile, and, on evidence of client participation in the pattern, dismissed the plaintiff's claims with prejudice.

**Oregon Court of Appeals — December 2025 tariff schedule.** The court established a per-infraction rate for AI hallucination misconduct: $500 per fabricated citation and $1,000 per fabricated quotation. This rate has been cited in 2026 sanctions opinions as a floor rather than a ceiling.

**Q1 2026 aggregate.** Commentary across Law.com, NPR, and state-bar publications tallies at least $145,000 in AI-hallucination sanctions in the first three months of 2026. The tally does not include reputational and disciplinary costs, case-dispositive sanctions (strike-with-prejudice orders), or fee-shifting awards.

## What the Courts Are Punishing

The opinions share a common structural critique. The sanctionable conduct is not the use of AI tools. It is the combination of:

1. Reliance on AI-generated citations without opening the cited authority in a primary database.
2. Failure to use a conventional citator (Shepard's, KeyCite, BCite) on the AI output before filing.
3. Signing and filing under Rule 11 or its state equivalents when the signer had not personally verified the work.
4. Escalation from a single missed check to a systemic practice pattern — multiple briefs, multiple matters, same firm or same attorney.

The "I trusted the tool" defense has not landed. The "associate did it" defense has not landed. The "it was cite-checking software that hallucinated" defense has not landed. The consistent holding is that verification is a non-delegable duty of the signer.

## Governing Authorities to Cite Internally

When a firm's AI policy, engagement letter, or skill-output certification references the sanctions landscape, the following authorities carry the weight:

- Federal Rule of Civil Procedure 11(b) — signature as certification of legal and factual support.
- ABA Model Rule 3.3 (candor toward the tribunal) and jurisdictional equivalents (e.g., Oregon RPC 3.3, New York Rule 3.3, California Rule 3.3).
- ABA Model Rule 1.1 cmt. 8 (duty of competence, including technology competence).
- Any local standing order on AI disclosure — a growing number of district and appellate courts now require an affirmative statement when AI was used in preparing a filing.
- State bar ethics opinions issued 2023–2026 on generative AI use in practice.

## Operational Implications for This Repo

### For every drafting skill

Any skill in this repo that produces text that will be filed, served, or sent externally (`demand-letter-drafter`, `discovery-response-drafter`, `legal-research-memo`, `privilege-log-reviewer`, `regulatory-compliance-checker`, `deposition-prep-outline`, `contract-clause-analyzer`) must assume that every citation and every quotation in its output is unverified until a human confirms it. Output requirements should steer the user toward the `ai-citation-verifier` sweep before filing.

### For the citation verifier

The `ai-citation-verifier` skill is the canonical pre-filing sweep. Its GREEN / YELLOW / RED labeling, verification queue, cross-validation prompts, and Rule 11 / RPC 3.3 certification checklist map directly to the failure modes the 2026 opinions describe. Any drafting skill that touches citations should cross-reference it.

### For AI governance

`ai-governance-legal.md` covers the standing governance frame (attorney accountability, confidentiality, competence, transparency). This entry supplies the 2026 enforcement overlay. Firms setting their AI policy should:

- Require a documented verification pass, not just attorney review, before any AI-assisted filing.
- Treat ungrounded AI output (no retrieval over a primary legal database) as categorically higher risk, and raise the default verification burden accordingly.
- Log every AI-assisted filing in a way that would survive a judicial inquiry — model used, prompts, retrieval source if any, verifier, and verification timestamp.
- Train on the pattern recognition of AI-fabricated citations: unusually clean quoted language, plausible case names paired with implausible reporters or years, sweeping rule statements with no hedges.

### For intake and matter opening

Consider adding a line to the engagement letter template and the `client-intake-summary` skill flagging that AI tools may be used in matter work, the firm verifies AI-generated authority against primary sources before filing, and the firm logs AI-assisted work product per policy. This is both a transparency measure and a defensive record if a later filing is challenged.

## Signals to Watch in 2026

- Additional circuits or states adopting per-infraction tariff schedules in the style of the Oregon Court of Appeals.
- Published standing orders requiring certification language in every filing that AI was or was not used in its preparation.
- Disciplinary opinions moving from monetary sanctions to license-impacting consequences (suspension, CLE requirements, practice monitoring).
- Opposing-counsel strategy: motions framed around an opponent's AI-generated filings, coupled with requests for fee-shifting under Rule 11 or the inherent authority of the court.
- Malpractice carrier underwriting terms conditioning coverage on documented AI verification workflows.

## Cross-References

- `skills/operations/ai-citation-verifier.md` — pre-filing sweep with GREEN/YELLOW/RED labeling and Rule 11 / RPC 3.3 certification checklist.
- `knowledge-base/best-practices/ai-governance-legal.md` — standing governance framework (accountability, confidentiality, competence).
- `knowledge-base/best-practices/ai-privilege-and-work-product.md` — privilege and work-product treatment of AI-generated material after *Heppner*.
- `knowledge-base/best-practices/agentic-legal-workflow-design.md` — design reference for multi-step legal workflows; verification is a required phase gate.

## Caveat

This entry is a design reference for this repo, not legal advice. Firm policies should be reviewed by counsel against the current rules of professional conduct, local court rules, and any applicable state bar ethics opinions in the jurisdictions where the firm practices.
