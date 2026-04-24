# AI, Attorney-Client Privilege, and the Work Product Doctrine

## Overview

Courts in 2026 are beginning to shape the boundaries of attorney-client privilege and the work product doctrine as they apply to content generated with AI tools. Early decisions — most notably *United States v. Heppner* (S.D.N.Y. Feb. 10, 2026 bench ruling; Feb. 17, 2026 written opinion) — have held that client-initiated use of consumer-grade AI platforms to analyze litigation issues does not, without more, cloak the resulting materials in either privilege or work product protection. A February 10, 2026 Eastern District of Michigan ruling by Magistrate Judge Patti reached the opposite conclusion on the work-product axis for a pro se civil litigant under Sixth Circuit law, holding that disclosing information to a consumer AI tool did not constitute waiver because waiver requires disclosure to an adversary or in a manner likely to reach the adversary. The two February rulings establish, in the same week, a doctrinal split on the work-product treatment of consumer-AI outputs. Courts addressing the question over the rest of 2026 will have to reason with both decisions. This entry distills the working principles legal teams should apply when their own workflows touch AI.

## Why This Matters

Three realities collide here. First, lawyers, clients, and litigants are all using AI tools — frequently, and often with no formal protocol. Second, inputs and outputs from those tools are discoverable by default unless a protection applies. Third, the protections we rely on — privilege and work product — were not drafted with generative AI in mind, so courts are reasoning by analogy and the analogies break down easily. Until the law settles, firms should manage AI use in litigation-sensitive matters the way they would manage any non-lawyer service provider: with documented scope, engagement, and confidentiality.

## Working Principles

The principles below are risk-reduction defaults. They are not a substitute for matter-specific advice, and they should be revisited as more courts decide these questions.

**1. Assume unprivileged by default.** Treat any AI interaction as discoverable and non-privileged unless a specific, documented protocol brings it inside counsel's direction. This applies to both clients and lawyers.

**2. Direct AI use through counsel.** When AI is used in connection with anticipated or pending litigation, the most defensible posture is that counsel directed the specific use — comparable to engaging an expert or paralegal. Document the direction contemporaneously, not after the fact.

**3. Use enterprise tools with appropriate terms.** Consumer-grade tools with generic terms of service that permit training or broad access are the riskiest category. Prefer enterprise offerings that include data processing terms barring training on inputs, limit retention, and support audit logging. Confirm that the tool's vendor is not a potential adverse party, co-defendant, or subject of the matter.

**4. Separate matter-specific workflows.** Do not mix matter-specific AI sessions with general-purpose use. Log which tool, which account, and which workspace was used for each matter so preservation and production obligations can be met cleanly.

**5. Preserve and log prompts and outputs.** Treat prompts and outputs the same way you would treat analyst notes and draft memoranda — preserve them, tag them to the matter, and be prepared to produce them (or assert a protection) if they become responsive.

**6. Prepare for AI-focused discovery.** Interrogatories and deposition questions increasingly ask: which AI tools were used, by whom, with what prompts, and when. Counsel should have a factual answer ready and should preserve the underlying records before the first discovery request.

**7. Clients deserve clear guidance.** Engagement letters should address whether clients may use AI tools independently in connection with the matter, and what channels preserve privilege. Silence invites the *Heppner* fact pattern.

**8. Avoid AI tools trained on your data.** Where AI outputs could become work product or embed client confidences, confirm that inputs are not used to train or improve the model, and that the vendor cannot review inputs except under narrowly scoped support workflows.

## Privilege Analysis Framework

When evaluating whether a specific AI interaction plausibly enjoys a protection, walk through these questions:

- **Who initiated the use?** Client acting alone (weakest; *Heppner* fact pattern), client at counsel's direction (stronger), lawyer or lawyer's agent (strongest).
- **What was the tool?** Consumer-grade with broad ToS (weakest; terms allowing training on inputs, retention, and disclosure to third parties undermined the confidentiality expectation in *Heppner*), enterprise with data processing terms (stronger; inputs excluded from training, contractual confidentiality, audit logging), firm-hosted or on-premises (strongest). Even enterprise protections do not *establish* privilege; they preserve the possibility of it.
- **What was the purpose?** General knowledge question (weak), matter-specific analysis in anticipation of litigation (potentially work product — and the *Patti* ruling in EDMI supports this for pro se civil litigants under Sixth Circuit law), confidential communication facilitating legal advice (potentially privileged).
- **Who saw the inputs and outputs?** Only the attorney-client dyad (strongest), internal to a privileged circle (strong), shared with third parties (often waived). *Heppner* treats inputs into a consumer AI tool as analogous to disclosure to a third party for privilege purposes; *Patti* treats the same conduct as not a work-product waiver because the AI is not an adversary. Both framings are plausible; the choice may be outcome-determinative on a particular fact pattern.
- **Was the use documented as counsel-directed?** Contemporaneous engagement and scope documentation materially strengthens both privilege and work product arguments. Post hoc rationalization — sharing AI outputs with counsel after independent use — does not convert prior independent use into attorney-directed work.

A strong answer on every question is not a guarantee — courts still split, and the February 2026 *Heppner* / *Patti* split shows the divergence is real — but weak answers across multiple axes substantially increase the risk that the materials are discoverable.

## Consumer vs. Enterprise AI — Post-*Heppner* Framework

The February 2026 decisions together establish the following working framework for firms choosing which AI tools to use and which to whitelist for matter work:

**Consumer AI tools** — publicly available, free or consumer-tier paid, terms of service that permit the vendor to use inputs for model training or allow disclosure to third parties, no contractual confidentiality, no data-processing agreement:

- *Heppner* holds that client-initiated use of these tools does not preserve privilege, and can operate as a waiver over the underlying information if privileged content is input.
- Work-product treatment is split as of February 2026: the SDNY denies protection; the EDMI preserves it for pro se civil litigants under Sixth Circuit law.
- These tools should not be used for matter-sensitive work by attorneys, staff, or clients. The engagement letter should say so affirmatively.

**Enterprise AI tools** — firm-procured under a written agreement, inputs excluded from training, contractual confidentiality and retention limits, audit logging, no reservation of rights to disclose except under narrowly scoped support or legal-process workflows:

- No published decision as of late April 2026 has ruled directly on whether use of an enterprise AI tool at attorney direction preserves privilege. The commentary across the February 2026 analyses consistently flags this as the "next question" for the courts.
- Enterprise protections are necessary but not sufficient: privilege analysis still asks whether the communication was for the purpose of obtaining legal advice and whether confidentiality was maintained in a privileged circle.
- Firms should treat enterprise AI access as a governance requirement for Tier 2 and Tier 3 work — not only to preserve privilege arguments, but to ensure the firm can produce a clean chain-of-custody log if opposing counsel challenges the filing under the pattern of the April 2026 *Prince Group* incident.

**Firm-hosted or on-premises AI** — no vendor access at all, deployed inside the firm's security perimeter:

- Strongest posture for privilege, subject to the same purpose and circle-of-confidentiality analysis.
- Operationally heavier; typically only justified for very large firms or very regulated practice areas.

**Client-side AI use** — an orthogonal problem. Clients increasingly use consumer AI tools on their own to think through their legal situation before meeting with counsel. The *Heppner* fact pattern should be assumed by default: clients who have input case facts, counsel's advice, or litigation strategy into a consumer AI tool may have waived privilege over the underlying material. Engagement letters should affirmatively warn clients about this, and intake processes should ask clients whether they have done so before the attorney proceeds.

## Practical Protocol for Litigation-Sensitive AI Use

Firms handling matters where AI tools may touch discoverable material should implement at least the following:

- **Pre-use authorization.** A lawyer on the matter authorizes the tool, account, and scope in writing before use.
- **Tool whitelist.** A maintained list of approved tools for litigation-sensitive work, with approved vendors, approved workspaces, and a short rationale.
- **Prompt/output preservation.** Automatic logging of prompts and outputs to the matter file, with retention aligned to the firm's standard work product retention policy.
- **Segregation.** No cross-matter reuse of sessions, workspaces, or accounts; no personal accounts for matter work.
- **Client instructions.** Engagement letters tell clients which tools are appropriate for matter-related use and which are not, with contact information if they are unsure.
- **Discovery readiness.** A one-page "AI use in this matter" memo is maintained from matter open, listing tools, users, dates, and purpose.
- **Periodic review.** Privilege and work product positions are re-evaluated as case law develops and whenever a new tool is introduced.

## Cross-References

- `knowledge-base/best-practices/ai-governance-legal.md` — General governance framework, accountability, and competence obligations
- `knowledge-base/best-practices/ai-hallucination-sanctions-2026.md` — Sanctions landscape, including the April 2026 *Prince Group* incident which bears on chain-of-custody and enterprise-vs-consumer tool selection.
- `skills/admin/document-intake-extractor.md` — Upstream AI workflow where prompt/output preservation applies.
- `skills/admin/client-intake-summary.md` — Intake is the correct point to ask the client whether they have used a consumer AI tool in connection with the matter (the *Heppner* preservation question).
- `skills/operations/deposition-prep-outline.md` — Includes question sequences for exploring AI use by witnesses and opposing parties.
- `skills/operations/pre-filing-independent-review.md` — Chain-of-custody audit for AI tool use at each drafting stage; treats consumer AI tool use on Tier 2/3 filings as disqualifying.

## Open Questions

Courts have not yet settled: whether attorney-directed AI use can reliably be treated as the functional equivalent of engaging a paralegal or expert; whether vendor access to inputs, even when contractually limited, waives confidentiality; whether work product protection extends to draft prompts the attorney discards; or how selective-waiver frameworks apply to AI outputs shared with specific government regulators. The February 2026 *Heppner* / *Patti* work-product split remains unresolved — the SDNY and EDMI positions are both plausible readings of the doctrine, and a circuit-level opinion is the most likely resolution vehicle. A separate open question is whether a court will extend the *Heppner* confidentiality reasoning to enterprise AI tools on the ground that vendor access — even if contractually limited — is still third-party access. Monitor matter-level decisions in each jurisdiction where the firm practices.

## Disclaimers

This entry is a working reference for firm governance and is not legal advice. Privilege and work product determinations are fact-intensive and jurisdiction-specific. Consult with qualified counsel for matter-specific questions.
