# AI Hallucination Sanctions — 2026 Landscape

## Overview

By late April 2026, U.S. federal and state courts have moved decisively past the "novelty" phase of AI-citation sanctions and into a structured enforcement posture. Q1 2026 alone produced more than $145,000 in sanctions against attorneys for filings that contained AI-fabricated citations or quotations. Two additional inflection points landed in March–April 2026: the first federal *circuit court* sanctions, and the first public Big Law hallucination incident. The pattern is consistent across jurisdictions: the courts are not punishing the use of AI. They are punishing the failure to verify AI output against primary authority before filing, and — increasingly — they are punishing firms that had written policies on paper but no institutional checkpoint that caught this filing.

This knowledge-base entry summarizes the 2026 landscape as a design reference for the skills in this repo — particularly the `ai-citation-verifier` skill (drafter-side sweep), the `pre-filing-independent-review` skill (institutional second-verifier), and any skill that produces text that will be filed, served, or sent externally.

## Anchor Cases and Benchmarks

**District of Oregon — April 4, 2026 — the *Brigandi* matter.** U.S. Magistrate Judge Mark Clarke entered an opinion imposing $96,000 in direct sanctions against a pro bono attorney, with total monetary penalties exceeding $110,000 once opposing counsel's fees were added. Three filings contained 23 fabricated citations and 8 fabricated quotations. The court relied on Federal Rule of Civil Procedure 11 and Oregon RPC 3.3 (duty of candor to the tribunal), struck the errant briefs without leave to refile, and, on evidence of client participation in the pattern, dismissed the plaintiff's claims with prejudice.

**Oregon Court of Appeals — December 2025 tariff schedule.** The court established a per-infraction rate for AI hallucination misconduct: $500 per fabricated citation and $1,000 per fabricated quotation. This rate has been cited in 2026 sanctions opinions as a floor rather than a ceiling.

**Fifth Circuit — *Fletcher v. Experian Information Solutions* — Spring 2026.** The first federal *circuit court* sanction specifically implicating AI-assisted drafting. Counsel filed a reply brief on appeal containing 16 fabricated quotations and 5 additional misrepresentations of law or fact. On the show-cause response, counsel admitted using two commercial legal AI platforms to "help organize and structure" argument. The court imposed a $2,500 monetary sanction and framed the matter as a lawyering problem rather than a technology problem: AI use does not reduce a lawyer's professional duties, and the signer is responsible for every quotation, citation, and factual representation.

**Sixth Circuit — *Whiting v. City of Athens, Tenn.* — March 13, 2026.** Two Tennessee attorneys were sanctioned $15,000 each (aggregate $30,000) plus full reimbursement of appellees' attorney fees and double court costs for filings containing more than two dozen fabricated or misrepresented citations across three consolidated appeals. The court did not expressly find AI use but articulated what became the governing line across the 2026 docket: "no filing should contain citations, however generated, that a lawyer has not personally read and verified." The court invoked its inherent authority and Appellate Rule 38.

**California Court of Appeal — *Noland v. Nazar*, B331918 (2d App. Dist.).** The first state appellate opinion to sanction an attorney for AI hallucinations. A $10,000 sanction was imposed and the opinion was published "as a warning." The court's framing — that hallucinated facts and law can be "circulated, believed, and become 'fact' and 'law' in some minds" — anticipates the adverse-authority and mischaracterization error categories that dominate the 2026 defect taxonomy.

**Big Law incident — *In re Prince Global Holdings Ltd.* (Chapter 15, S.D.N.Y. Bankr.) — April 2026.** On April 18, 2026, a partner at a top-tier Wall Street firm filed a letter to the Chief Judge of the U.S. Bankruptcy Court for the Southern District of New York apologizing for AI-generated errors in an April 9 emergency motion and in four accompanying documents (the verified petition, the joint administration motion, the scheduling motion, and two declarations). A Schedule A attached to the apology letter catalogued approximately 40 corrections: statutory misquotations of the Bankruptcy Code, mischaracterizations of authority, and at least one fabricated case. Opposing counsel — not the filing firm's internal review — surfaced the errors. The firm's own statement acknowledged that "notwithstanding its comprehensive policies and training requirements on AI use, the protocols were not followed, and the firm's ordinary citation-checking processes also failed to catch the invented authorities and other mistakes." As of the apology letter, the court had not yet ruled on sanctions. The incident is the cleanest 2026 illustration that written AI policies and "ordinary citation-checking processes" alone are not sufficient — a firm needs an institutional second-verifier checkpoint whose attestation does not depend on the drafting team's self-report.

**Q1 2026 aggregate.** Commentary across Law.com, NPR, and state-bar publications tallies at least $145,000 in AI-hallucination sanctions in the first three months of 2026. With the 5th and 6th Circuit sanctions and the California state-appellate opinion layered on, the enforcement posture now spans three federal circuits, at least one state appellate court, and multiple district courts. The tally does not include reputational and disciplinary costs, case-dispositive sanctions (strike-with-prejudice orders), fee-shifting awards, or the not-yet-ruled April 2026 Big Law incident.

## What the Courts Are Punishing

The opinions share a common structural critique. The sanctionable conduct is not the use of AI tools. It is the combination of:

1. Reliance on AI-generated citations without opening the cited authority in a primary database.
2. Failure to use a conventional citator (Shepard's, KeyCite, BCite) on the AI output before filing.
3. Signing and filing under Rule 11 or its state equivalents when the signer had not personally verified the work.
4. Escalation from a single missed check to a systemic practice pattern — multiple briefs, multiple matters, same firm or same attorney.
5. (Emerging — post-April 2026 *Prince Group* incident.) Existence of a firm AI policy that was not followed on the specific filing, combined with the absence of an institutional second-verifier who would have caught the policy departure. The argument that "the firm has a policy" now works against the signer once the opposing side shows the policy was not enforced on this filing.

The "I trusted the tool" defense has not landed. The "associate did it" defense has not landed. The "it was cite-checking software that hallucinated" defense has not landed. The "we have a written AI policy" defense has not landed — see *In re Prince Global Holdings Ltd.* (firm's own apology letter acknowledges policies not followed). The consistent holding is that verification is a non-delegable duty of the signer, and firms that put the institutional checkpoint on paper without putting it into workflow are increasingly at risk that the paper will be read against them.

## Expanded Defect Taxonomy (post-April 2026)

The *Prince Group* apology letter, the 6th Circuit *Whiting* opinion, and the California *Noland* opinion together push the repo-internal defect taxonomy past "bad citations" into four operational error categories that any drafter-side and reviewer-side sweep should address:

- **A. Citation errors.** Fabricated cases; wrong reporter, volume, year, or pin cite; non-existent subsections. The majority of 2025 sanctions were in this bucket.
- **B. Quotation errors.** Quoted text that does not appear verbatim in the source; ellipsis that hides qualifying language; quotation marks around paraphrase. The Oregon tariff schedule and the 5th Circuit *Fletcher* sanction weight these higher than citation errors.
- **C. Characterization errors.** The citation is real, but the draft says the court held X when it held Y — or treats dicta as holding, a concurrence as the majority, or a reversal as an affirmance. These dominate the *Prince Group* Schedule A catalogue and are the hardest errors for the signer to catch without reading the underlying opinion.
- **D. Statutory and rule-text errors.** Bankruptcy Code, U.S.C., state code, or FRCP/local-rule text quoted incorrectly or to a subsection that does not exist. Several of the *Prince Group* corrections fell in this category. The drafter-side citation verifier traditionally focuses on A and B; the reviewer-side sweep is the natural home for C and D.

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

The `ai-citation-verifier` skill is the canonical drafter-side pre-filing sweep. Its GREEN / YELLOW / RED labeling, verification queue, cross-validation prompts, and Rule 11 / RPC 3.3 certification checklist map directly to the failure modes the 2026 opinions describe. Any drafting skill that touches citations should cross-reference it.

### For the pre-filing independent review

The `pre-filing-independent-review` skill is the institutional counterpart to the citation verifier and the design response to the April 2026 *Prince Group* incident. The independent reviewer is an attorney who did not draft the filing, audits the chain-of-custody of the draft (which AI tools touched it, at which stage, under whose account), runs the four-category error sweep across the full draft (citations, quotations, characterizations, and statutory/rule text), checks adverse authority, and either issues a Release-to-File attestation or returns a defect list. Tier 3 filings (appellate briefs, emergency motions, sanctions responses, any filing against a sophisticated adversary, and any filing where a generative AI tool touched the draft) should not ship without this pass.

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
- The ruling on the April 2026 *Prince Group* apology letter — whether the S.D.N.Y. Bankruptcy Court treats "policy in place but not followed" as mitigating or aggravating will shape firm-wide AI governance defaults for the rest of 2026.
- Whether additional federal circuits follow the 5th and 6th Circuits in treating AI-assisted appellate briefing errors as sanctionable at the circuit level (7th, 9th, 11th, and D.C. Circuits are most likely to produce the next published opinion).
- Further state-appellate decisions following California *Noland v. Nazar* in publishing opinions "as a warning" — New York, Texas, Illinois, Washington, and Florida courts of appeal are candidates.
- Published standing orders requiring certification language in every filing that AI was or was not used in its preparation.
- Disciplinary opinions moving from monetary sanctions to license-impacting consequences (suspension, CLE requirements, practice monitoring).
- Opposing-counsel strategy: motions framed around an opponent's AI-generated filings, coupled with requests for fee-shifting under Rule 11 or the inherent authority of the court. *Prince Group* makes opposing-counsel review a live litigation weapon, not only a correction mechanism.
- Malpractice carrier underwriting terms conditioning coverage on documented AI verification workflows and documented institutional second-verifier processes.

## Cross-References

- `skills/operations/ai-citation-verifier.md` — drafter-side pre-filing sweep with GREEN/YELLOW/RED labeling and Rule 11 / RPC 3.3 certification checklist.
- `skills/operations/pre-filing-independent-review.md` — institutional second-verifier pass; chain-of-custody audit, four-category error sweep (citations / quotations / characterizations / statutory), adverse-authority check, Release-to-File attestation or defect list.
- `knowledge-base/best-practices/ai-governance-legal.md` — standing governance framework (accountability, confidentiality, competence).
- `knowledge-base/best-practices/ai-privilege-and-work-product.md` — privilege and work-product treatment of AI-generated material after *Heppner* and the February 2026 privilege-doctrine split.
- `knowledge-base/best-practices/agentic-legal-workflow-design.md` — design reference for multi-step legal workflows; verification is a required phase gate.

## Caveat

This entry is a design reference for this repo, not legal advice. Firm policies should be reviewed by counsel against the current rules of professional conduct, local court rules, and any applicable state bar ethics opinions in the jurisdictions where the firm practices.
