# AI, Attorney-Client Privilege, and the Work Product Doctrine

## Overview

Courts in 2026 are beginning to shape the boundaries of attorney-client privilege and the work product doctrine as they apply to content generated with AI tools. Early decisions — most notably *United States v. Heppner* (S.D.N.Y. 2026) — have held that client-initiated use of consumer-grade AI platforms to analyze litigation issues does not, without more, cloak the resulting materials in either privilege or work product protection. Other courts have reached narrower conclusions and treated attorney-directed AI use more like traditional expert or paralegal assistance. The law is unsettled and likely to split further. This entry distills the working principles legal teams should apply when their own workflows touch AI.

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

- **Who initiated the use?** Client acting alone (weakest), client at counsel's direction (stronger), lawyer or lawyer's agent (strongest).
- **What was the tool?** Consumer-grade with broad ToS (weakest), enterprise with data processing terms (stronger), firm-hosted or on-premises (strongest).
- **What was the purpose?** General knowledge question (weak), matter-specific analysis in anticipation of litigation (potentially work product), confidential communication facilitating legal advice (potentially privileged).
- **Who saw the inputs and outputs?** Only the attorney-client dyad (strongest), internal to a privileged circle (strong), shared with third parties (often waived).
- **Was the use documented as counsel-directed?** Contemporaneous engagement and scope documentation materially strengthens both privilege and work product arguments.

A strong answer on every question is not a guarantee — courts still split — but weak answers across multiple axes substantially increase the risk that the materials are discoverable.

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
- `skills/admin/document-intake-extractor.md` — Upstream AI workflow where prompt/output preservation applies
- `skills/operations/deposition-prep-outline.md` — Includes question sequences for exploring AI use by witnesses and opposing parties

## Open Questions

Courts have not yet settled whether attorney-directed AI use can reliably be treated as the functional equivalent of engaging a paralegal or expert; whether vendor access to inputs, even when contractually limited, waives confidentiality; whether work product protection extends to draft prompts the attorney discards; or how selective-waiver frameworks apply to AI outputs shared with specific government regulators. Monitor matter-level decisions in each jurisdiction where the firm practices.

## Disclaimers

This entry is a working reference for firm governance and is not legal advice. Privilege and work product determinations are fact-intensive and jurisdiction-specific. Consult with qualified counsel for matter-specific questions.
