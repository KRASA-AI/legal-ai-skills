# AI, Attorney-Client Privilege, and the Work Product Doctrine

## Overview

Courts in 2026 are beginning to shape the boundaries of attorney-client privilege and the work product doctrine as they apply to content generated with AI tools. Early decisions — most notably *United States v. Heppner* (S.D.N.Y. Feb. 10, 2026 bench ruling; Feb. 17, 2026 written opinion) — have held that client-initiated use of consumer-grade AI platforms to analyze litigation issues does not, without more, cloak the resulting materials in either privilege or work product protection. A February 10, 2026 Eastern District of Michigan ruling by Magistrate Judge Patti, since formally captioned in commentary as *Warner v. Gilbarco, Inc.*, reached the opposite conclusion on the work-product axis for a pro se civil litigant under Sixth Circuit law, holding that disclosing information to a consumer AI tool did not constitute waiver because waiver requires disclosure to an adversary or in a manner likely to reach the adversary, and reasoning that "generative AI platforms are tools, not persons" — using one to assist in drafting is no more a work-product waiver than using a word processor, a search engine, or a legal research database. The two February rulings — *Heppner* and *Warner v. Gilbarco* — establish, in the same week, a doctrinal split on the work-product treatment of consumer-AI outputs. Courts addressing the question over the rest of 2026 will have to reason with both decisions. This entry distills the working principles legal teams should apply when their own workflows touch AI.

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
- **What was the purpose?** General knowledge question (weak), matter-specific analysis in anticipation of litigation (potentially work product — and the *Warner v. Gilbarco* ruling in E.D. Mich. supports this for pro se civil litigants under Sixth Circuit law), confidential communication facilitating legal advice (potentially privileged).
- **Who saw the inputs and outputs?** Only the attorney-client dyad (strongest), internal to a privileged circle (strong), shared with third parties (often waived). *Heppner* treats inputs into a consumer AI tool as analogous to disclosure to a third party for privilege purposes; *Warner v. Gilbarco* treats the same conduct as not a work-product waiver because the AI is "a tool, not a person" and not an adversary. Both framings are plausible; the choice may be outcome-determinative on a particular fact pattern.
- **Was the use documented as counsel-directed?** Contemporaneous engagement and scope documentation materially strengthens both privilege and work product arguments. Post hoc rationalization — sharing AI outputs with counsel after independent use — does not convert prior independent use into attorney-directed work.

A strong answer on every question is not a guarantee — courts still split, and the February 2026 *Heppner* / *Warner v. Gilbarco* split shows the divergence is real — but weak answers across multiple axes substantially increase the risk that the materials are discoverable.

## Consumer vs. Enterprise AI — Post-*Heppner* Framework

The February 2026 decisions together establish the following working framework for firms choosing which AI tools to use and which to whitelist for matter work:

**Consumer AI tools** — publicly available, free or consumer-tier paid, terms of service that permit the vendor to use inputs for model training or allow disclosure to third parties, no contractual confidentiality, no data-processing agreement:

- *Heppner* holds that client-initiated use of these tools does not preserve privilege, and can operate as a waiver over the underlying information if privileged content is input.
- Work-product treatment is split as of February 2026: the S.D.N.Y. (*Heppner*) denies protection; the E.D. Mich. (*Warner v. Gilbarco*) preserves it for pro se civil litigants under Sixth Circuit law on the rationale that "generative AI platforms are tools, not persons."
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

## Protective-Order Provisions Governing AI Use in Discovery

A separate but adjacent line of 2026 cases sits at the intersection of confidentiality, work product, and the protective order itself. Where the privilege analysis above asks whether a particular AI use waives a protection over a particular communication or document, the protective-order analysis asks an upstream question: what AI-tool restrictions does the order itself impose on the parties' handling of discovery material once produced?

Two early-2026 federal magistrate opinions illustrate two distinct approaches that firms should expect to encounter going forward:

**Confidentiality-anchored approach (contractual-safeguards path).** *Morgan v. V2X, Inc.* (D. Colo., 2026) treated the AI question through the lens of confidentiality risk. The parties agreed the existing confidentiality order needed to be amended to address the submission of confidential discovery material to AI tools but disagreed on the scope. The court required that any AI tool used to process confidential material be subject to contractual safeguards, including: (i) a prohibition on using the inputs to train the model; (ii) restrictions on onward disclosure; and (iii) the ability to delete data. The order does not categorically prohibit AI use; it conditions it on tool-side terms that look like the enterprise-AI characteristics described above.

**Open-vs-closed approach (categorical-restriction path).** *Jeffries v. Harcros Chemicals, Inc.* (D. Kan., 2026) took a broader posture. The court granted a motion amending the protective order to restrict the use of "open" or publicly accessible AI tools — distinguished from "closed" or "secure" AI tools — for the entirety of the discovery record, including material that was not designated confidential. The premise is that even non-confidential discovery material in the aggregate can carry strategy and matter-context value that should not flow through a public model.

The two opinions are not in conflict: *Morgan* sets a confidentiality floor; *Jeffries* sets a broader integrity floor. Both are consistent with the privilege-side framework above (consumer/open tools are highest-risk; enterprise/closed tools with appropriate terms are the operating default). The practical change is that the constraints are now embedded in the protective order itself — a tool selection that previously sat in the firm's internal AI Use Policy may now be a court-ordered constraint enforceable through the order's own enforcement mechanisms.

What this means operationally:

- **Read the protective order before any AI step on discovery material.** Treat the AI-tool restriction as a first-class provision alongside AEO, attorneys-eyes-only, and outside-counsel-only designations. The deposition-transcript-analyzer and discovery-response-drafter skills already gate on protective-order status; add the AI-tool restriction to the gate.
- **Negotiate the AI-tool clause early.** The 2026 default is no longer silence on AI; it is one of the *Morgan* contractual-safeguards approach, the *Jeffries* open-versus-closed approach, or some hybrid. Counsel should propose a clause aligned with the firm's tool whitelist rather than waiting for opposing counsel to draft a clause that the firm's preferred tools cannot satisfy.
- **Rule 502(d) plus the AI clause are not interchangeable.** A Rule 502(d) order addresses inadvertent privilege production; an AI-tool clause addresses *what tools may touch the material at all*. A firm that has only the 502(d) order has not addressed the AI question.
- **Map the AI clause to the firm's tool whitelist before the run.** If the protective order restricts AI use to "closed" tools as in *Jeffries*, the orchestrator (see `agentic-legal-workflow-design.md`) must enforce that selection at every tool boundary in the workflow, not only at the first one. Logging the tool-selection decision per step gives the firm a clean record if the opposing party later asks how the order was honored.

Counsel should also expect that the *Morgan* / *Jeffries* line will continue to develop in 2026 — the candidate questions include whether a protective order's AI-tool clause can be enforced against a litigant who used a public tool before the order entered, whether vendors' "no-training" representations are sufficient evidence of the *Morgan* contractual-safeguard requirements, and whether the *Jeffries* open-versus-closed line covers downstream AI features in otherwise enterprise products (vendor-side experimental features, default-on retention, etc.).

## Cross-References

- `knowledge-base/best-practices/ai-governance-legal.md` — General governance framework, accountability, and competence obligations
- `knowledge-base/best-practices/ai-hallucination-sanctions-2026.md` — Sanctions landscape, including the April 2026 *Prince Group* incident which bears on chain-of-custody and enterprise-vs-consumer tool selection.
- `skills/admin/document-intake-extractor.md` — Upstream AI workflow where prompt/output preservation applies.
- `skills/admin/client-intake-summary.md` — Intake is the correct point to ask the client whether they have used a consumer AI tool in connection with the matter (the *Heppner* preservation question).
- `skills/operations/deposition-prep-outline.md` — Includes question sequences for exploring AI use by witnesses and opposing parties.
- `skills/operations/discovery-response-drafter.md` — Tier 2/3 skill that consumes protective-order status and must honor any AI-tool restriction in the order at the point of drafting.
- `skills/operations/deposition-transcript-analyzer.md` — Tier 3 skill that gates on protective-order designations; the *Morgan* / *Jeffries* protective-order line of cases means the gate now includes the AI-tool restriction in the order itself.
- `skills/operations/pre-filing-independent-review.md` — Chain-of-custody audit for AI tool use at each drafting stage; treats consumer AI tool use on Tier 2/3 filings as disqualifying.

## Open Questions

Courts have not yet settled: whether attorney-directed AI use can reliably be treated as the functional equivalent of engaging a paralegal or expert; whether vendor access to inputs, even when contractually limited, waives confidentiality; whether work product protection extends to draft prompts the attorney discards; or how selective-waiver frameworks apply to AI outputs shared with specific government regulators. The February 2026 *Heppner* / *Warner v. Gilbarco* work-product split remains unresolved — the S.D.N.Y. and E.D. Mich. positions are both plausible readings of the doctrine, and a circuit-level opinion (Second Circuit reviewing *Heppner*, or Sixth Circuit reviewing *Warner v. Gilbarco* in a represented-litigant case) is the most likely resolution vehicle. A separate open question is whether a court will extend the *Heppner* confidentiality reasoning to enterprise AI tools on the ground that vendor access — even if contractually limited — is still third-party access. The *Morgan* / *Jeffries* protective-order line raises a parallel set of questions: whether a court-ordered AI-tool restriction can reach pre-order conduct, what level of vendor-side evidence is required for the *Morgan* contractual-safeguard approach, and whether the *Jeffries* open-versus-closed framing captures vendor experimental features layered on top of an otherwise enterprise tool. Monitor matter-level decisions in each jurisdiction where the firm practices.

## Disclaimers

This entry is a working reference for firm governance and is not legal advice. Privilege and work product determinations are fact-intensive and jurisdiction-specific. Consult with qualified counsel for matter-specific questions.
