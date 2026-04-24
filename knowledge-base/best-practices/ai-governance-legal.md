# AI Governance for Legal Workflows

## Overview

As AI tools become embedded in legal practice — from research and drafting to contract review and deposition analysis — firms must establish governance frameworks that address accountability, data protection, quality assurance, and ethical compliance. AI changes how work is performed, but it does not change who remains accountable for the outcome.

## Core Principles

### 1. Attorney Accountability Remains Unchanged

AI-generated output must always be reviewed by a qualified attorney before reliance or submission. The supervising attorney bears responsibility for accuracy, completeness, and compliance with professional obligations regardless of whether the work was AI-assisted.

### 2. Confidentiality and Privilege Protection

All AI tools used with client data must comply with Rule 1.6 (Confidentiality) obligations. Key requirements include evaluating vendor data handling practices, confirming that client data is not used for model training, maintaining audit trails for privileged material processed through AI, and using enterprise-grade tools with appropriate data processing agreements.

### 3. Competence in AI Use

ABA Model Rule 1.1 Comment 8 requires lawyers to stay current with technology, including AI. Firms should provide training on effective AI use, establish protocols for prompt engineering and output verification, and document AI-assisted workflows for quality assurance purposes.

### 4. Transparency with Clients

Consider disclosure obligations around AI use. Some jurisdictions and clients require affirmative disclosure when AI tools are used in matter work. Engagement letters should address AI use policies where appropriate.

### 5. Bias and Fairness Monitoring

AI tools may reflect biases in training data. Legal teams should be aware of potential bias in AI-suggested language, case predictions, or risk assessments, and apply professional judgment as a check.

## Practical Governance Framework

### Tier 1: Low-Risk AI Use (Minimal Oversight)
- Internal drafting assistance (emails, memos)
- Summarization of public documents
- Formatting and administrative tasks
- Time entry cleanup

### Tier 2: Moderate-Risk AI Use (Attorney Review Required)
- Legal research and case law analysis
- First-draft contract clauses
- Client-facing communication drafts
- Intake data extraction

### Tier 3: High-Risk AI Use (Senior Attorney Oversight Required)
- Regulatory compliance analysis
- Litigation strategy recommendations
- Contract clause risk assessment on high-value deals
- Deposition preparation and contradiction analysis
- Any output that will be filed with a court or regulatory body

## EU AI Act Considerations (Effective August 2026)

High-risk AI provisions require organizations to implement risk assessments, documentation, and human oversight for AI systems used in high-stakes decisions. Legal teams using AI for access to justice, case assessment, or evidence evaluation may fall under high-risk classification and should prepare compliance documentation.

## Recommended Firm Policies

1. Maintain an approved AI tools list with security assessments for each
2. Require attorney sign-off on all AI-assisted deliverables at Tier 2 and above
3. Prohibit uploading client-identifiable data to consumer-grade AI tools
4. Log AI usage by matter for billing transparency and audit purposes
5. Conduct quarterly reviews of AI output quality and error rates
6. Establish incident response procedures for AI-related errors or data exposure

## Sources and Further Reading

- ABA Formal Opinion 512 (AI and Confidentiality)
- ABA Model Rules 1.1 (Competence), 1.6 (Confidentiality), 5.3 (Supervisory Duties)
- EU AI Act, Title III (High-Risk AI Systems)
- State bar ethics opinions on AI use (varies by jurisdiction)

## Cross-References

- `knowledge-base/best-practices/ai-hallucination-sanctions-2026.md` — 2026 enforcement landscape, now including the 5th and 6th Circuit sanctions, the California state-appellate opinion, and the April 2026 *Prince Group* Big Law incident. Read in conjunction with this entry when setting verification-pass policy and when deciding which filings require an institutional second-verifier.
- `skills/operations/ai-citation-verifier.md` — drafter-side pre-filing sweep that operationalizes the verification duty under FRCP 11 and RPC 3.3.
- `skills/operations/pre-filing-independent-review.md` — institutional second-verifier pass, expected for Tier 3 filings under the governance framework above. The skill audits chain-of-custody (which AI tools touched the draft, at which stage, under whose account, grounded or ungrounded), runs the four-category error sweep (citations, quotations, characterizations, statutory/rule text), and issues either a Release-to-File attestation or a defect list. Exists specifically to address the April 2026 *Prince Group* failure mode — a firm had comprehensive policies, the policies were not followed on one filing, and the ordinary citation-checking processes also failed.
- `skills/operations/deposition-transcript-analyzer.md` — Tier 3 AI use over deposition transcripts (typically privileged-adjacent and protective-order designated). The skill includes a traceability rule that anchors every quoted or paraphrased item to a page:line cite, mirroring the verification-by-design pattern of the citation verifier.
