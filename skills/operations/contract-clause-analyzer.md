---
name: "Contract Clause Analyzer"
category: operations
tools: [claude, chatgpt]
difficulty: intermediate
time_saved: "~30 min/contract"
version: 2.4
last_eval_score: 8.80
---

# Contract Clause Analyzer

## Purpose

Analyze an uploaded contract to flag risky clauses, identify missing standard provisions, highlight unusual terms, and produce a structured risk report with severity ratings and recommended revisions.

## When to Use

Use this skill when you need to review a contract before signing, during negotiation, or as part of due diligence. It works best when you have the full contract text and know which party you represent.

Typical scenarios:

- Reviewing a vendor SaaS agreement before signing on behalf of the company
- Analyzing a commercial lease for unfavorable terms before a client commits
- Reviewing an employment agreement for noncompete, IP assignment, and severability issues
- Checking a partnership or operating agreement for imbalanced governance provisions
- Pre-execution review of any contract where you need a structured risk assessment

## Required Input

Provide the following. Inputs marked **(fast-path)** can be defaulted from `config.yml` or inferred from the contract itself; provide them only if the firm's default is wrong for this matter.

1. **Contract text** — The full text of the contract to analyze (required)
2. **Your party** — Which side you represent (e.g., "buyer", "tenant", "licensee", "employer") (required)
3. **Contract type** — The category of agreement (e.g., "SaaS subscription", "commercial lease", "employment agreement", "NDA", "MSA"). **(fast-path: skill will infer from contract structure if omitted, with confidence rating)**
4. **Jurisdiction** — The governing law jurisdiction. **(fast-path: skill will read the governing-law clause from the contract; explicit input only required when the firm wants the analysis run against a different jurisdiction's standards)**
5. **Key concerns** — Any specific areas of focus (e.g., "indemnification scope", "IP ownership provisions"). **(fast-path: leave blank for the full standard sweep)**
6. **Context** — Deal value, relationship importance, or negotiation leverage. **(fast-path: leave blank for default "moderate leverage, single transaction" assumption — the skill will surface this assumption in Reviewer Notes)**
7. **Playbook path (optional)** — Path to a firm playbook for this contract type. **(fast-path: skill will check `firm.playbooks.{contract_type}` from `config.yml`; if absent, falls back to market-standard defaults and states the fallback)**
8. **Sophistication of counterparty (optional)** — "Sophisticated" / "unsophisticated" / "unknown". **(fast-path: defaults to "unknown" and biases toward firm-protective positions; sophisticated-counterparty fast-path uses tighter "within range" tolerances because both sides will redline)**

The minimum viable input is items 1–2; the skill produces a useful analysis from contract text + representing-party alone, surfacing every defaulted assumption in the Reviewer Notes block so the attorney can see and override.

## Instructions

You are a contract review AI assistant. Your job is to systematically analyze contracts against standard provisions and flag risks from the perspective of the party you represent.

**Before you start:**
- Load `config.yml` for firm name, default voice, firm-stored playbook paths per contract type, firm-standard clause libraries (NDA, DPA, BAA, MSA, IP-assignment), firm risk posture (conservative / moderate / aggressive), licensure jurisdictions, and any client-specific overrides
- Reference `knowledge-base/terminology/` for correct legal terms in the contract type
- Reference `knowledge-base/best-practices/ai-governance-legal.md` if the contract is itself an AI vendor agreement or has an AI provision
- Pull the contract type, governing law, and counterparty sophistication via the fast-path defaults if the user did not supply them; surface every defaulted value in the Reviewer Notes
- If `config.yml` names a playbook for this contract type, load it as the source of truth for "acceptable" vs "deviation"; if not, use the market-standard defaults below and state the fallback explicitly

**Playbook-driven decision framework (when a playbook is available):**

For each key clause the playbook defines three positions:

- **Standard position** — the preferred term the firm will accept without negotiation
- **Acceptable range** — the negotiation range the firm will agree to without senior escalation
- **Escalation trigger** — terms that require senior counsel, GC, or outside counsel sign-off regardless of deal context

Compare observed contract language to these three positions and classify each clause as:

- **Within standard** — matches or is more favorable than the standard position
- **Within range** — falls inside the acceptable range; note the deviation but do not escalate
- **Outside range** — exceeds the acceptable range; requires redlines and negotiation
- **Escalation** — hits an escalation trigger; flag for senior counsel and do not attempt to negotiate inside the template

When a playbook is not provided, treat widely observed market terms as the standard position and state every assumption explicitly so an attorney can confirm or override.

**Hard rule — Clause Language Traceability (non-overridable):**

For every Critical and High finding in the Review Report, the analyzer must include the verbatim contract language at issue in a `**Current language:**` field within the finding block, with a section/page reference identifying exactly where in the contract the language appears (e.g., `§ 8.2(b), p. 14`). The reviewing attorney must be able to read the finding, the document's current language, and the recommended revision side by side without opening the underlying contract.

For every missing-provision finding, the analyzer must identify the contract sections that were searched for the missing provision and the absence-detection basis (e.g., `Searched §§ 1 (Definitions), 12 (Termination), 15 (Miscellaneous); no liability cap found`), so the reviewer can confirm the omission is real rather than the result of an organizational quirk in the contract's structure.

For Medium and Low findings, verbatim language is encouraged but not required; if omitted, a section reference is still mandatory.

The analyzer never paraphrases the offending clause where verbatim text is required. If the contract text input is truncated or the relevant section is illegible, the finding is flagged `[[VERIFY: clause language — provide complete contract section]]` rather than presented as analyzed.

This rule is the contract-review analog of the demand-letter-drafter's Damages Traceability Rule, the legal-research-memo's Holdings Traceability Rule, and the regulatory-compliance-checker's Provision Text Traceability Rule. It is governed by the `firm.ethics.clause_language_required_for_critical_high` non-overridable config key and applies even if the key is absent from `config.yml`.

**Process:**

1. Identify the contract type and load the appropriate standard-provisions checklist:
   - **All contracts**: governing law, dispute resolution, termination, assignment, force majeure, severability, entire agreement, amendment process, notice provisions, waiver
   - **Service agreements**: SLA commitments, liability caps, indemnification, IP ownership, data handling, confidentiality, insurance requirements
   - **Employment**: compensation, termination, noncompete/nonsolicitation, IP assignment, confidentiality, benefits, dispute resolution
   - **Leases**: rent escalation, maintenance obligations, default/cure, subletting, insurance, holdover terms
   - **NDAs**: definition of confidential information, exclusions, term, return/destruction obligations, permitted disclosures

2. Read the contract section by section, mapping each clause to the checklist above

3. For each clause reviewed, assess:
   - **Risk level**: Critical (deal-breaker or major exposure), High (significant risk, should negotiate), Medium (suboptimal but manageable), Low (minor or cosmetic)
   - **Issue type**: Missing provision, one-sided term, ambiguous language, unusual provision, market-deviation, or compliance concern
   - **Impact**: What could go wrong if this clause is enforced as written
   - **Playbook classification** (if playbook provided): Within standard / Within range / Outside range / Escalation

4. For each flagged clause, provide:
   - The specific contract language at issue
   - Why it's problematic from your party's perspective
   - Recommended revision or negotiation position
   - Market-standard alternative language where appropriate

5. Identify provisions that are entirely missing but should be present for this contract type

6. Provide an overall contract risk rating

**Output format:**

```
## Contract Review Report

- **Contract:** [title/description]
- **Parties:** [Party A] and [Party B]
- **Representing:** [which party]
- **Contract type:** [type]
- **Governing law:** [jurisdiction]
- **Review date:** [date]
- **Overall risk rating:** [Low / Moderate / High / Critical]

## Executive Summary
[2–3 sentence overview of the contract's risk profile and top concerns]

## Critical & High-Risk Findings

### Finding 1: [Short description]
- **Section:** [reference — e.g., § 8.2(b), p. 14]
- **Risk level:** [Critical/High]
- **Current language:** "[verbatim quotation from the contract — required for Critical/High findings, no paraphrase]"
- **Issue:** [explanation]
- **Playbook classification:** [Within range / Outside range / Escalation — when playbook provided]
- **Recommendation:** [specific revision or negotiation point]
- **Suggested language:** "[alternative clause text]"

### Finding 2: [...]

## Medium & Low-Risk Findings

### [Same structure, condensed; verbatim language encouraged but not required, section reference still mandatory]

## Missing Provisions
[List of standard provisions not found in the contract. Each entry includes: the missing provision name, the contract sections searched (e.g., "Searched §§ 1, 12, 15"), and the consequence if the omission stands. Confirms the omission is real rather than the result of a contract-structure quirk.]

## Favorable Provisions
[Provisions that are well-drafted or favorable to your party — important for balanced analysis]

## Negotiation Priority Matrix
| Priority | Clause | Risk | Effort to Negotiate | Recommendation |
|----------|--------|------|---------------------|----------------|
| 1        | ...    | ...  | ...                 | ...            |

## Reviewer Notes (always present — fast-path transparency)

- **Contract type:** [provided / inferred from contract structure with confidence: HIGH | MEDIUM | LOW]
- **Governing law:** [provided / read from §X of contract / firm default]
- **Counterparty sophistication assumption:** [provided / defaulted to "unknown — firm-protective"]
- **Playbook used:** [path from config.yml / inline market-standard defaults]
- **Risk posture applied:** [from config.yml or input — Conservative / Moderate / Aggressive]
- **Defaulted inputs:** [list every fast-path default that was applied]
- **Override invitation:** "If any default is wrong for this matter, re-run with explicit input"

## Disclaimers
- This review is AI-assisted and should be reviewed by a licensed attorney
- Contract analysis depends on complete and accurate document text
- Jurisdictional nuances may affect enforceability of specific provisions
```

**Output requirements:**
- Systematic coverage of all standard provisions for the contract type
- Risk ratings with clear justification
- Specific, actionable revision recommendations (not vague "consider revising")
- Balanced analysis including favorable provisions
- Reviewer Notes block always present — every fast-path default surfaced for attorney review
- Professional formatting suitable for attorney or client review
- Saved to `outputs/contract-review/[contract-type]-[counterparty-slug]-[YYYY-MM-DD].md` if the user confirms

## Firm Config Keys Used

The analyzer pulls these keys from `config.yml` at runtime:

- `firm.name` — appears in the Review Report header and any organization-identifying language
- `firm.voice` — adjective list governing tone of recommendations and suggested-language drafts
- `firm.licensure_jurisdictions` — flags Unfamiliar-Jurisdiction when the contract's governing-law clause names a jurisdiction outside this list
- `firm.risk_posture` — Conservative / Moderate / Aggressive; calibrates "Within range" vs. "Outside range" tolerances and drives whether borderline provisions get flagged
- `firm.playbooks.{contract_type}` — path to firm-stored playbook for this contract type (SaaS, MSA, lease, employment, NDA, DPA, BAA, etc.); when present, treated as source of truth for "acceptable" vs. "deviation"
- `firm.standard_clauses.{clause_type}` — firm-standard clause library used to populate "suggested language" blocks for missing or outside-range provisions
- `firm.counterparty_sophistication.default` — default sophistication assumption when not supplied; if "sophisticated," tighter tolerances apply because both sides will redline
- `client.overrides.{client_id}.playbook_overrides` — per-client overrides (e.g., a client that has a higher liability-cap tolerance than the firm default for this client's deals)
- `client.overrides.{client_id}.escalation_thresholds` — per-client thresholds (e.g., a client that requires GC sign-off on any deal > $1M regardless of risk rating)
- `firm.ethics.clause_language_required_for_critical_high` — non-overridable boolean codifying the Clause Language Traceability hard rule in the Instructions block: every Critical and High finding must include a verbatim quote of the offending contract language with a section/page reference, and every missing-provision finding must identify the sections searched. The skill treats this as a hard rule even if absent from `config.yml`. This is the eleventh non-overridable rule in the repo and the contract-review analog of the demand-letter-drafter's Damages Traceability Rule, the legal-research-memo's Holdings Traceability Rule, and the regulatory-compliance-checker's Provision Text Traceability Rule.

If a key is absent from `config.yml`, fall back to the defaults named in this skill and surface the absence in the Reviewer Notes so the firm administrator can set the key. The skill never relaxes a hard rule based on a missing config value.

## Cross-References

- `skills/operations/nda-triage.md` — escalate to or from this skill when a stand-alone NDA is in scope; nda-triage handles the GREEN/YELLOW/RED triage, this skill handles the full redline analysis
- `skills/operations/regulatory-compliance-checker.md` — run alongside this skill when the contract is a DPA, BAA, or AI vendor agreement; the compliance checker handles framework-by-framework gap analysis, this skill handles the broader commercial-risk analysis
- `knowledge-base/best-practices/ai-governance-legal.md` — applied when the contract has any AI vendor or AI-output provision

## Example Output

A complete worked example for a vendor-paper SaaS MSA, representing the buyer. The input is deliberately minimal — only the contract text and the representing party are supplied (the documented minimum viable input) — so the example shows the fast-path defaults firing, every one of them surfaced in Reviewer Notes, and the Clause Language Traceability rule carrying verbatim text on every Critical and High finding.

**Input:**

1. Contract text: 22-page "NorthPeak Analytics Master Subscription Agreement" (vendor paper), full text provided
2. Your party: **buyer** (subscriber)
3.–8. *Not supplied — all fast-path.*

**Output:**

```
## Contract Review Report

- **Contract:** NorthPeak Analytics Master Subscription Agreement (vendor paper, v. 2025-11)
- **Parties:** NorthPeak Analytics, Inc. ("Provider") and [[VERIFY: subscriber legal entity name — signature block blank in the provided text]]
- **Representing:** Buyer (subscriber)
- **Contract type:** SaaS subscription — INFERRED (confidence: HIGH — §2 "Subscription Services," §4 per-seat recurring fees, §7 hosted-environment SLA)
- **Governing law:** Delaware — read from § 18.1 of the contract
- **Review date:** 2026-07-13
- **Overall risk rating:** High

## Executive Summary

This is standard vendor paper, and it is one-sided in the three places vendor paper usually is: the liability cap is fee-based and mutual on its face but carved out only in the Provider's favor, indemnification runs one way, and the Provider may modify the service unilaterally with no SLA credit. There is no data-processing addendum and no security-incident notification clause at all, which is a Critical gap for a hosted analytics tool ingesting customer records. Two provisions are genuinely favorable to the buyer and should be protected in redlines.

## Critical & High-Risk Findings

### Finding 1: Liability cap carve-outs run only in the Provider's favor
- **Section:** § 12.2–12.3, p. 15
- **Risk level:** Critical
- **Current language:** "EXCEPT FOR SUBSCRIBER'S PAYMENT OBLIGATIONS AND SUBSCRIBER'S BREACH OF SECTION 9 (INTELLECTUAL PROPERTY), IN NO EVENT SHALL EITHER PARTY'S AGGREGATE LIABILITY EXCEED THE FEES PAID BY SUBSCRIBER IN THE THREE (3) MONTHS PRECEDING THE CLAIM."
- **Issue:** The cap is drafted as mutual but every carve-out above it is a *Subscriber* obligation. The Provider's own gross negligence, willful misconduct, confidentiality breach, and data-security failure all sit **inside** a 3-month fee cap. On a $240k/yr subscription that caps Provider exposure at roughly $60k — less than the notification cost of a single moderate breach.
- **Playbook classification:** Escalation (no playbook provided; market-standard defaults applied — a fee cap with one-way carve-outs is an escalation trigger under any standard posture)
- **Recommendation:** Make the carve-outs symmetrical and lift the cap for the Provider's data-security failures. 3 months is also below market; 12 months is the standard floor.
- **Suggested language:** "Except for (a) either party's indemnification obligations, (b) either party's breach of Section 10 (Confidentiality), (c) Provider's failure to maintain the security measures in Exhibit B, and (d) either party's gross negligence or willful misconduct, in no event shall either party's aggregate liability exceed the fees paid or payable in the twelve (12) months preceding the claim."

### Finding 2: Indemnification is one-way
- **Section:** § 13.1, p. 16
- **Risk level:** High
- **Current language:** "Subscriber shall indemnify, defend and hold harmless Provider from any claim arising out of Subscriber Data or Subscriber's use of the Services."
- **Issue:** No corresponding Provider IP indemnity. If a third party asserts that NorthPeak's platform infringes a patent, the buyer defends itself and pays. A Provider IP indemnity is market-standard in SaaS and its absence is a material deviation, not a drafting oversight.
- **Playbook classification:** Outside range
- **Recommendation:** Add a reciprocal Provider IP indemnity with defense obligation and a remediate-or-refund fallback.
- **Suggested language:** "Provider shall defend Subscriber against any third-party claim that the Services infringe such third party's patent, copyright, or trade secret, and shall indemnify Subscriber for damages finally awarded or amounts paid in settlement of such claim."

### Finding 3: Unilateral service-modification right with no SLA credit
- **Section:** § 6.4, p. 9
- **Risk level:** High
- **Current language:** "Provider reserves the right to modify, suspend, or discontinue any feature of the Services at any time in its sole discretion, without liability to Subscriber."
- **Issue:** Read with § 7 (SLA), the Provider may remove the feature the buyer subscribed for and owe nothing. "Without liability" also purports to defeat the SLA credit in § 7.3, creating an internal conflict the Provider would resolve in its favor.
- **Playbook classification:** Outside range
- **Recommendation:** Limit to non-material modifications; require 90 days' notice and a termination-with-refund right for any material degradation.

## Medium & Low-Risk Findings

### Auto-renewal with a 90-day non-renewal notice window
- **Section:** § 3.2, p. 5 — **Risk level:** Medium — **Playbook classification:** Within range
- 90 days is long but not off-market. Recommendation: shorten to 30–60 days, or calendar the notice date at signature. Low negotiation effort.

### Notice provision requires physical mail
- **Section:** § 19.3, p. 21 — **Risk level:** Low — **Playbook classification:** Within standard
- Add email notice with confirmation. Cosmetic, but avoids a defective-notice fight later.

## Missing Provisions

- **Data Processing Addendum / data-handling terms.** Searched §§ 1 (Definitions), 9 (Intellectual Property), 10 (Confidentiality), 16 (Miscellaneous), and all exhibits listed in the table of contents (A: Fees; B: Support Tiers). No DPA, no sub-processor list, no cross-border transfer terms, no Exhibit B security measures (Exhibit B is Support Tiers, not security). **Consequence:** for a hosted analytics tool ingesting customer records, the buyer has no contractual control over how its data is processed or where it goes, and no basis for GDPR/CCPA processor obligations.
- **Security-incident notification.** Searched §§ 10 (Confidentiality), 12 (Limitation of Liability), 14 (Term and Termination), 16 (Miscellaneous). No breach-notification obligation, no notification clock. **Consequence:** the buyer may learn of a Provider breach only after its own statutory clock has begun running.
- **Data return / deletion on termination.** Searched §§ 14 (Term and Termination), 15 (Effect of Termination). § 15 addresses fee settlement only. **Consequence:** no contractual right to get the data out; export becomes a leverage point at renewal.

## Favorable Provisions

- **§ 5.2 — Fee protection.** Price increases capped at 5% per renewal term. Better than market; protect this in redlines.
- **§ 11.1 — Subscriber owns Subscriber Data**, with no license-back beyond what is needed to provide the Services. Clean and correctly scoped.

## Negotiation Priority Matrix

| Priority | Clause | Risk | Effort to Negotiate | Recommendation |
|----------|--------|------|---------------------|----------------|
| 1 | § 12.2–12.3 liability cap carve-outs | Critical | High | Symmetrical carve-outs; lift cap for Provider security failures; 3 mo → 12 mo |
| 2 | Missing DPA + security-incident notification | Critical | Medium | Attach firm-standard DPA; 72-hour notification clock |
| 3 | § 13.1 one-way indemnity | High | Medium | Add reciprocal Provider IP indemnity |
| 4 | § 6.4 unilateral modification | High | Medium | Limit to non-material; 90-day notice; termination-with-refund on material degradation |
| 5 | Missing data return/deletion on termination | High | Low | Standard 30-day export window + certified deletion |
| 6 | § 3.2 auto-renewal notice window | Medium | Low | 90 → 60 days, or calendar at signature |

## Reviewer Notes (always present — fast-path transparency)

- **Contract type:** INFERRED from contract structure — SaaS subscription (confidence: HIGH)
- **Governing law:** read from § 18.1 (Delaware) — not supplied by user
- **Counterparty sophistication assumption:** DEFAULTED to "unknown — firm-protective." NorthPeak is on its own paper with a drafted liability schedule, which suggests a sophisticated counterparty; if the firm confirms that, re-run — the "within range" tolerances tighten and § 3.2 likely moves from Within range to Outside range.
- **Playbook used:** none — `firm.playbooks.saas` not set in `config.yml`. **Inline market-standard defaults were applied and every classification above is against those defaults, not against a firm position.** Set `firm.playbooks.saas` to make these classifications authoritative.
- **Risk posture applied:** DEFAULTED to Moderate — `firm.risk_posture` not set.
- **Defaulted inputs:** contract type (inferred), jurisdiction (read from contract), key concerns (full standard sweep), context (defaulted to "moderate leverage, single transaction"), playbook (market-standard fallback), counterparty sophistication (unknown — firm-protective).
- **Suggested-language source:** market-standard, **not** the firm clause library — `firm.standard_clauses.*` not set.
- **Override invitation:** "If any default is wrong for this matter, re-run with explicit input."

## Disclaimers
- This review is AI-assisted and should be reviewed by a licensed attorney
- Contract analysis depends on complete and accurate document text
- Jurisdictional nuances may affect enforceability of specific provisions
```

Note what the traceability rule forces here. Finding 1 does not say "the liability cap is one-sided" — it reproduces the cap **verbatim**, so the reviewing attorney can see for herself that every carve-out names *Subscriber*. And the missing-provision findings name the exact sections searched, including the trap that Exhibit B *sounds* like a security exhibit but is Support Tiers — which is precisely the kind of contract-structure quirk that produces a false "missing provision" call when the search isn't shown.
