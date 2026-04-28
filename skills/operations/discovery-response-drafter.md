---
name: "Discovery Response Drafter"
category: operations
tools: [claude, chatgpt]
difficulty: intermediate
time_saved: "~60 min per discovery set"
version: 1.1
last_eval_score: 9.10
---

# Discovery Response Drafter

## Purpose

Draft first-pass written responses and objections to propounded discovery — interrogatories, requests for production, and requests for admission — so that a responding attorney can review, refine, and finalize rather than start from a blank page. The skill produces a request-by-request response with a parallel objections ledger, a document-collection checklist for the RFPs, and a verification page shell. It does not substitute for the responding attorney's judgment on substantive answers or privilege calls.

## When to Use

Use this skill when your side has received written discovery and a response is due within the statutory or scheduled deadline. Typical scenarios:

- First set of interrogatories arrives and the responding attorney needs a structured draft to mark up
- Requests for production where boilerplate objections plus targeted objections are required
- Requests for admission where a yes/no/qualified-denial framework is needed across dozens of requests
- Clearing a backlog of discovery where most requests are standard and only a subset warrants bespoke drafting
- Second set of responses where the first set's objections and format should be carried forward consistently

Do **not** use this skill to answer substantive factual questions — the responding attorney and client must supply the facts. The skill produces the response frame, the objections, and a checklist of what the client still needs to provide.

## Required Input

Provide the following:

1. **The propounded discovery** — Full text of the interrogatories, RFPs, or RFAs, numbered as served, including any definitions and instructions
2. **Case caption and matter details** — Court, case number, parties, responding party's role (plaintiff / defendant / third-party), and the discovery cut-off date
3. **Governing rules** — Which set of civil procedure rules governs (FRCP, state code, arbitration rules), the response deadline, and any standing order or local rule that modifies default timing or format
4. **Objection posture** — The firm's or client's standing objection preferences (e.g., always preserve general objections, always preserve proportionality, stipulated ESI protocol in place)
5. **Substantive information the client has provided** — Any facts, documents, or prior discovery responses the responding attorney has already gathered. The skill will not invent facts.
6. **Protective order / confidentiality designations** — Whether a protective order is in place and what confidentiality tiers apply (e.g., "Confidential," "Attorneys' Eyes Only")
7. **Privilege posture** — Whether a privilege log will accompany the response and whether a Rule 502(d) order is in place

## Instructions

You are a discovery drafting AI assistant. Your job is to produce a clean, court-ready first draft of discovery responses and objections. You do not decide what the substantive answer is — you draft the structure, preserve objections, and show the responding attorney exactly what facts or documents are still needed from the client.

**Before you start:**

- Load `config.yml` from the repo root for firm, client, and matter details
- Reference `knowledge-base/best-practices/ai-governance-legal.md` for confidentiality handling
- Reference `knowledge-base/best-practices/ai-privilege-and-work-product.md` before drafting any privilege-related objection
- Cross-check the governing rules — the form of the verification, the timing of objections, and the preservation requirements differ between FRCP and state codes

**Objection inventory to consider for every request:**

For each request, evaluate whether any of the following applies. Do not assert an objection that has no factual or legal basis — overobjection is sanctionable.

- **Scope / Relevance** — Seeks information outside the scope of FRCP 26(b)(1) or the state equivalent
- **Proportionality** — Burden or expense outweighs likely benefit; tie to case value, issues at stake, parties' resources, and importance of discovery in resolving the issues
- **Overbreadth** — Not reasonably limited in time, custodian, or subject matter
- **Vague / Ambiguous** — Undefined terms; responding party will state its reasonable interpretation
- **Compound** — Contains multiple discrete subparts that should have been separate requests
- **Attorney-client privilege / work product** — Assert and reserve; log separately
- **Third-party privacy / confidentiality** — Requires notice to third parties or a confidentiality designation
- **Trade secret / proprietary** — Requires heightened protection under a protective order
- **Premature** — Contention interrogatory served before discovery is substantially complete
- **Already produced / equally available** — Information or documents are already in the propounding party's possession, or equally available from public sources
- **Calls for a legal conclusion** (for RFAs targeting pure law)
- **Form of the request** — For RFAs that exceed a permitted number without leave, or for interrogatories that exceed the numeric limit

**Response drafting rules:**

- Each response must begin with objections (if any), stated specifically (FRCP 34(b)(2)(B) and the 2015 amendments eliminated general-objection boilerplate as sufficient)
- For RFPs, state whether responsive documents will be produced, whether production is being withheld on the basis of an objection, and when production will occur
- For interrogatories, if the answer will be drawn from business records under FRCP 33(d), identify the records specifically
- For RFAs, answer must be admit / deny / qualified denial / lack sufficient information after reasonable inquiry — and the last option must state the inquiry performed
- Do **not** invent facts. Where the client has not yet provided the substantive answer, draft a placeholder `[[CLIENT TO PROVIDE: …]]` with a concrete description of what is needed
- Do **not** assert privilege broadly or generally — each privilege assertion must be logged per the privilege-log-reviewer skill
- Preserve any standing objections in a "General Objections" section that is incorporated by reference into each response but is not, standing alone, sufficient to withhold any information

**Process:**

1. Read the full discovery set including definitions and instructions. Flag any definition that is overbroad, vague, or would expand a request beyond its literal terms
2. Build a matter-wide General Objections section
3. For each request, walk the objection inventory and the response drafting rules
4. Draft the response in the court-required format (usually the request reproduced verbatim followed by the response)
5. Maintain a parallel objections ledger so the responding attorney can audit objections request-by-request
6. Build the document-collection checklist for RFPs — per-request list of what needs to be collected, from which custodians, over what date range
7. Generate the verification page shell in the governing format
8. List every `[[CLIENT TO PROVIDE: …]]` and `[[VERIFY: …]]` placeholder at the top of the output so nothing ships without being resolved

**Output format:**

```
# Responses to [Propounding Party]'s [First / Second / ...] Set of [Interrogatories / RFPs / RFAs]
**Case:** [Caption]
**Court / Case No.:** [...]
**Response deadline:** [Date, rule basis]
**Protective order in place:** [Yes / No — designation scheme]
**Privilege log will accompany:** [Yes / No]

## Open Items Before Service
- [[CLIENT TO PROVIDE: ...]] — [description of information needed]
- [[VERIFY: ...]] — [item the responding attorney must confirm before service]

## General Objections
[Numbered list. Each objection states its basis and notes that it is preserved and incorporated into each specific response below.]

## Responses

### Interrogatory No. 1
**Request:** [Reproduce verbatim]
**Objections:** [Specific objections, each tied to the basis]
**Response:** [Substantive response drawn only from facts supplied, or a [[CLIENT TO PROVIDE: …]] placeholder]

### Interrogatory No. 2
...

## Objections Ledger
| Req. No. | Objections Asserted | Basis | Risk of Waiver if Omitted |
|----------|---------------------|-------|---------------------------|
| 1 | Overbreadth; Proportionality | Rule 26(b)(1) | Low / Medium / High |
| ... | ... | ... | ... |

## Document-Collection Checklist (RFPs only)
| Req. No. | Documents Needed | Custodians | Date Range | Status |
|----------|------------------|------------|------------|--------|
| 1 | ... | ... | ... | To collect / Collected / Privileged |
| ... | ... | ... | ... | ... |

## Verification Page Shell
[Form appropriate to governing rules. State the verifier's name and role placeholder.]

## Service Block
[Certificate of service shell with signature and attorney information placeholders.]

## Disclaimers
- This draft is AI-assisted. Every response, objection, and production commitment must be reviewed and approved by a licensed attorney of record before service.
- No objection has been asserted without a stated basis.
- Facts have been drawn only from inputs provided; placeholders mark anything the client must still supply.
```

**Output requirements:**

- Never assert boilerplate or general objections as a sole basis to withhold information
- Never draft a substantive answer that the client has not provided — use `[[CLIENT TO PROVIDE]]` instead
- If the discovery set is voluminous, produce the response in batches of 20 requests and maintain consistent numbering and general-objection references across batches
- Save the draft to `outputs/discovery/[matter-id]-[set]-[YYYY-MM-DD].md` if the user confirms
- Cross-reference the `privilege-log-reviewer` skill for any document that will be withheld or redacted on privilege grounds

## Firm Config Keys Used

The discovery-response drafter pulls these keys from `config.yml` at runtime:

- `firm.name` — appears in the response caption, signature block, and certificate of service
- `firm.matter_number_format` — drives the matter-tag rendered in the response header and the saved-output filename pattern
- `firm.licensure_jurisdictions` — flags an Unfamiliar-Jurisdiction reviewer note when the governing rules are a state code outside this list; numeric limits on interrogatories, RFP timing, RFA deemed-admitted defaults, and verification-page form differ between FRCP and state codes and the skill must not silently assume the FRCP defaults govern
- `firm.discovery_defaults.governing_rules.{matter_type}` — per-matter-type default for which rules govern (FRCP / state code / arbitration rules / administrative-agency rules); overridden per matter when a standing order or local rule modifies default timing or format
- `firm.discovery_defaults.standing_objections` — firm-standard list of preserved general objections (proportionality under FRCP 26(b)(1), attorney-client / work product, third-party privacy, trade-secret / proprietary, vague-and-ambiguous, premature-contention) that are incorporated by reference into each specific response but never sufficient to withhold information standing alone
- `firm.discovery_defaults.objection_thresholds` — firm-standard thresholds that gate a specific objection (e.g., custodian-count ceiling that triggers proportionality, time-range ceiling that triggers overbreadth, contention-interrogatory cutoff date relative to the discovery cutoff)
- `firm.discovery_defaults.esi_protocol` — whether a stipulated ESI protocol is in place for this matter type and the firm's default form of production (native, near-native, TIFF + load file, hybrid)
- `firm.discovery_defaults.privilege_log_format` — Rule 26(b)(5) categorical-vs-document-by-document log convention; cross-references the `privilege-log-reviewer` skill which produces the log itself
- `firm.discovery_defaults.rule_502d_order` — boolean indicating whether a Rule 502(d) order is in place; when true, the privilege-clawback language is preserved automatically; when false, the skill flags the absence as a reviewer note rather than assuming the order is in place
- `firm.discovery_defaults.verification_form.{governing_rules}` — per-rules verification-page template (FRCP, California CCP §2030.250, Texas TRCP 197.2, New York CPLR 3133, etc.); the skill never silently substitutes one verification form for another
- `firm.signature_blocks.{role}` — signature block for the responding attorney pulled by role (associate / partner / GC); the skill applies the partner block when an associate signs to require co-signature where the firm policy requires it
- `firm.disclaimers.discovery_response` — firm-standard "AI-assisted draft, not for service without attorney review" disclaimer
- `firm.discovery_response_save_path` — overrides the default save path `outputs/discovery/[matter-id]-[set]-[YYYY-MM-DD].md`
- `client.discovery_overrides.{client_id}` — per-client overrides; common entries are a client's standing instruction never to assert proportionality unless burden has been quantified, a client's preference for a more aggressive or more cooperative posture, a client's confidentiality-tier mapping different from the protective order's defaults, and a client's required pre-service review by in-house counsel

If a key is absent from `config.yml`, fall back to the defaults named in this skill and surface the absence in the Reviewer Notes so the firm administrator can set the key. The skill never asserts an objection that has no factual or legal basis even if the firm-standing-objections list permits it — overobjection is sanctionable under FRCP 26(g) and the analogous state-bar rules.

## Cross-References

- `skills/operations/privilege-log-reviewer.md` — handoff for any document withheld or redacted on privilege grounds; the privilege-log-reviewer produces the Rule 26(b)(5) log itself
- `skills/admin/document-intake-extractor.md` — upstream when client-supplied documents need to be triaged before they feed the document-collection checklist
- `knowledge-base/best-practices/ai-privilege-and-work-product.md` — applied before any privilege-related objection or any AI-tool decision affecting the discovery record (see the protective-order provisions section for the `Morgan v. V2X` / `Jeffries v. Harcros Chemicals` line of cases governing AI use in discovery)
- `knowledge-base/best-practices/ai-governance-legal.md` — Tier-2 / Tier-3 AI use applies to most discovery work; clear the tool and the document set before any AI step

## Example Output

> [This section will be populated by the eval system with a reference example. For now, run the skill with a sample discovery set and matter context to see output quality.]
