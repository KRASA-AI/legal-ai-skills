# Court-Ordered AI Disclosure and Certification — 2026 Landscape

## Overview

By April 2026, the question "must I disclose that I used AI in this filing?" has stopped being theoretical for many jurisdictions and has become a concrete filing requirement. Federal district judges, state trial courts, and an increasing number of state administrative offices have issued standing orders, administrative orders, and local rule amendments requiring lawyers (and in many cases self-represented litigants) to certify whether generative AI was used in preparing a court submission, and to certify that any AI-generated content was independently verified before filing. The certification language and triggering scope vary by jurisdiction, but the common spine across the 2026 orders is: identify the tool, certify independent verification, accept responsibility for every quotation and citation regardless of source, and be prepared for sanctions for both AI errors *and* failure to disclose.

This entry is a working reference for the skills in this repo whose output may end up in a court filing — `ai-citation-verifier`, `pre-filing-independent-review`, `legal-research-memo`, `discovery-response-drafter`, `regulatory-compliance-checker`, `privilege-log-reviewer`, `demand-letter-drafter` — so the disclosure check can be added to the certification step rather than missed at filing time.

## Why This Matters

A draft can clear the drafter-side citation sweep, clear the institutional second-verifier sweep, and still get rejected, struck, or returned-with-sanctions if the court has a standing order that requires a specific certification and the certification is missing. The April 2026 *Prince Group* incident illustrates the substantive failure mode (citations and characterizations); the standing-order failure mode is procedural — the court does not need to find that the AI hallucinated to sanction the failure to certify.

A second reason: the disclosure landscape is changing fast and is jurisdictionally fragmented. A litigator with a multi-jurisdiction docket cannot rely on a single firm-wide template; the filing template must be matter-specific to the tribunal, and the firm needs an internal index that is kept current.

## Anchor Orders (United States — Federal and State, 2026)

The orders below are the anchor examples this repo treats as design references. The list is not exhaustive — the 2026 trend is that each new published incident produces additional standing orders within weeks — and any specific firm should maintain its own jurisdiction index against the courts where it actually files.

**Florida — Eleventh Judicial Circuit (Miami-Dade County) — Administrative Order No. 26-04 (Jan. 15, 2026).** Issued by the Chief Judge requiring an affirmative certification when generative AI is used in preparing a filing.

**Florida — Seventeenth Judicial Circuit (Broward County) — Administrative Order No. 2026-03-Gen (Jan. 26, 2026).** Companion order to the 11th Circuit AO; same general design; certification language tracks the 11th Circuit form with local-rule references.

**Florida — Palm Beach County — Administrative Order No. 2.109-4/26 (signed Apr. 10, 2026).** Extends mandatory generative-AI disclosure to attorneys *and* self-represented litigants when AI is used in preparing pleadings, motions, memoranda, responses, proposed orders, or other court documents. The order provides a form certification, identifying the AI program used and certifying that all factual assertions, legal authorities, and citations were independently reviewed and verified for accuracy.

**Federal district courts — judge-specific standing orders.** A non-uniform but growing set of district judges (varying by district and chambers) require a per-filing AI disclosure with their first scheduling order or first chambers practices document. The repo treats these as a category rather than a complete list because they change weekly; the matter-opening checklist must include a search for the assigned judge's chambers rules at matter open and again before each substantive filing.

**State appellate courts following *Noland v. Nazar* (CA Court of Appeal).** The California court's decision to publish the *Noland* opinion "as a warning" has been cited approvingly in commentary across New York, Texas, Illinois, Washington, and Florida courts of appeal as a model. Watch for additional state-appellate opinions and accompanying administrative orders during 2026.

**Federal Circuit Courts.** As of late April 2026, no federal circuit has adopted a uniform AI-disclosure rule applicable across all panels (the Fifth Circuit publicly declined to adopt a circuit-wide rule after attorney pushback). Individual circuit panels have addressed AI in published sanctions opinions (5th Cir. *Fletcher*; 6th Cir. *Whiting*) without imposing a forward-looking certification requirement. Counsel filing at the circuit level must read the panel's specific orders rather than rely on a single circuit-wide template.

## Anchor Regulatory Deadlines (Non-U.S.)

**EU AI Act — Article 6 / Annex III high-risk classification — full enforceability August 2, 2026.** AI systems used "in the administration of justice and democratic processes" are classified as high-risk under Annex III. Firms deploying AI in legal research, case analysis, evidence evaluation, or alternative dispute resolution that fall within the EU territorial scope must have conformity assessments, human oversight, transparency disclosures, logging, and GDPR-compliant data processing in place by August 2, 2026. Penalties for non-compliance run up to €35 million or 7% of worldwide turnover for prohibited practices, up to €15 million or 3% for other infringements, and up to €7.5 million or 1% for supplying incorrect or misleading information. The U.S. parallel does not exist as of April 2026, but firms with EU offices, EU clients, or matters that will be relied on by EU regulators should treat August 2, 2026 as a hard deadline.

**United Kingdom — Civil Justice Council consultation (February 2026).** Consultation on the use of AI for preparing court documents; non-binding as of April 2026 but signals the direction of UK procedural reform.

## The Proposed "Hyperlink Rule"

A separate but related design pattern surfaced in 2026 commentary is the proposed mandatory **Hyperlink Rule**: every electronically filed pleading, motion, memorandum of law, or brief must include a functional hyperlink for each citation to a court decision, statute, rule, regulation, or other legal authority — pointing to a reputable legal database (Westlaw, Lexis, Bloomberg Law, Fastcase) or an official court or agency website. The 2020 New York Commercial Division Administrative Order AO/133/20 (mandating hyperlinks to NYSCEF docket entries and authorizing courts to require hyperlinks to court decisions, statutes, rules, regulations, treatises, and other legal authorities hosted in legal research databases or official government websites) is the precedent commentators cite for federal and state adoption.

The mechanism is that a hallucinated case cannot be hyperlinked to a real database record, so the act of attempting to add the hyperlink at the drafting stage exposes the fabrication before the document is filed. Several federal-rule and state-bar working groups are evaluating this proposal as a 2026 prophylactic. The repo's `ai-citation-verifier` skill can be run with a "hyperlink-or-fail" mode to anticipate this rule: if a cited authority cannot be linked to a reputable database record, the citation is automatically RED.

## Common Spine of the 2026 Disclosure Orders

Across the orders surveyed above, the certification language tends to require the filer to state, in substance:

- Whether generative AI was used in any part of the preparation of the filing.
- The name of the AI program or platform used (e.g., "ChatGPT-4o, OpenAI"; "Claude Sonnet 4.6, Anthropic"; "CoCounsel, Thomson Reuters"; "Harvey").
- Whether AI use was disclosed to the client and authorized.
- That every factual assertion in the filing is supported by record evidence.
- That every legal authority and every citation was independently reviewed and verified for accuracy by the signing attorney.
- That the signing attorney accepts personal responsibility for every quotation, citation, and characterization in the filing regardless of whether AI was used.

Some orders also require disclosure of the prompt used to generate AI-assisted content, retention of AI-session logs for a defined period, and an undertaking to provide the logs to the court on request.

## Operational Implications for This Repo

### For matter opening (intake)

When a new matter opens, the intake skill (`skills/admin/client-intake-summary`) should capture the assigned tribunal and the filing courthouse so the firm can run a one-time check at matter open against the court's chambers rules and standing orders, and pull the AI-disclosure certification template into the matter file before any substantive filing. This is a small step at intake that prevents a frantic last-mile fix at filing time.

### For the citation verifier

The drafter-side `ai-citation-verifier` skill already includes a "Jurisdictional AI disclosure, if any, has been satisfied" line in its certification checklist. When the matter is in a jurisdiction with a known disclosure order (e.g., the Florida circuits surveyed above), the certification line should include the specific administrative-order number and confirm that the form certification has been added to the filing. The skill's RED criteria should also incorporate a "hyperlink-or-fail" check for matters where the Hyperlink Rule is in effect — a citation that cannot be linked to a reputable database record is automatically RED.

### For the institutional second-verifier

The `pre-filing-independent-review` skill's Step 5 (AI-disclosure and standing-order check) is the right home for this verification at the institutional level. The reviewer must confirm that the certification language matches the administrative-order's prescribed form (not a paraphrase), that the program and platform are correctly named, and that the verification undertaking is in the form the order requires. A correctly substantively-clean filing that omits a court-required certification is still a Defect List output, not a Release-to-File.

### For the firm's AI governance policy

The governance policy (`knowledge-base/best-practices/ai-governance-legal.md`) should add an "AI disclosure compliance" obligation: maintain an internal index of court AI-disclosure orders for the jurisdictions where the firm files, refresh the index quarterly, and require every matter-opening check to confirm the assigned tribunal's posture. Treat the disclosure order's prescribed certification as a Tier 3 governance object — the matter cannot ship a filing without the form attached.

### For client-side disclosure

Several 2026 orders extend the disclosure obligation to self-represented litigants (notably Palm Beach County). When the firm's client-facing materials (engagement letter, intake form, client-portal upload guidance) discuss AI use, the firm should warn the client that some courts also require disclosure for filings the client makes pro se on collateral matters (e.g., separate small-claims proceedings) and that consumer AI use in those filings can produce parallel sanction risk.

## Maintaining the Jurisdiction Index

Because the disclosure landscape is jurisdictionally fragmented and is changing each quarter, the operational artifact a firm needs is not a single template but a maintained jurisdiction index. The index should record, per jurisdiction the firm files in:

- Whether a disclosure order is in effect (Y/N).
- The order's citation (administrative order number, effective date).
- The order's scope (which filings trigger; whether SRL filers are covered; whether AI session logs must be retained).
- The prescribed certification form (text or hyperlink).
- The order's penalty schedule for non-disclosure, if any.
- The date the index entry was last verified.

Keep the index in `outputs/disclosure-index/jurisdictions.md` (or equivalent) and refresh on a quarterly cadence and immediately after any high-profile incident in a jurisdiction the firm files in.

## Cross-References

- `knowledge-base/best-practices/ai-hallucination-sanctions-2026.md` — Sanctions landscape, including the substantive (hallucination) failure mode that makes disclosure orders the procedural complement.
- `knowledge-base/best-practices/ai-privilege-and-work-product.md` — Tool-tier framework (consumer / enterprise / firm-hosted) that informs how AI use is disclosed and described in the certification.
- `knowledge-base/best-practices/ai-governance-legal.md` — Standing governance frame; the disclosure jurisdiction index is a Tier 3 governance object.
- `skills/operations/ai-citation-verifier.md` — Drafter-side certification step where the disclosure line is initialed.
- `skills/operations/pre-filing-independent-review.md` — Institutional verifier step where the disclosure form is matched to the prescribed order language.
- `skills/admin/client-intake-summary.md` — Intake-time tribunal check that pulls the right certification template into the matter file.

## Caveat

This entry is a design reference for this repo, not legal advice. Disclosure orders are jurisdictionally specific and change frequently; firm policies and templates should be reviewed by counsel against the current rules of professional conduct, the local rules of the courts where the firm practices, and any chambers-specific standing orders before reliance.
