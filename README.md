# Legal AI Skills

**Free, open-source AI prompts and workflows built for lawyers.** Clone this repo, point your AI assistant at it, and start saving hours every week.

> Built and maintained by [KRASA AI](https://krasa.ai) — free AI tutorials and skills for every industry.
> See all industries at [krasa.ai/industries](https://krasa.ai/industries).

---

## What's Inside

This repo is a complete AI toolkit for legal. Every skill is a standalone prompt file that works with **Claude, ChatGPT, or any major AI assistant** — no coding required.

| Skill | What it does | Time saved |
|-------|-------------|------------|
| AI Citation & Quote Verifier | Run a pre-filing sweep over a draft brief, motion, memorandum, or letter to catch fabricated or misused authorities before they reach a court, regulator, or opposing counsel. | ~45 min/brief |
| Contract Clause Analyzer | Analyze an uploaded contract to flag risky clauses, identify missing standard provisions, highlight unusual terms, and produce a structured risk report with severity ratings and recommended revisions. | ~30 min/contract |
| Demand Letter Drafter | Generate a first-draft demand letter from case facts, legal basis, and desired outcome — formatted as a professional attorney letter ready for review, customization, and sending on firm letterhead. | ~30 min/letter |
| Deposition Prep Outline | Create a structured deposition question outline tailored to the deposition type (fact witness, party, 30(b)(6) corporate representative, expert, or hostile/adverse), with cross-testimony contradiction analysis when multiple prior statements are available. | ~45 min/depo |
| Deposition Transcript Analyzer | Turn one or more completed deposition transcripts into a litigation-ready work product that a partner can use for motion practice, trial preparation, and cross-examination planning. | ~4 hr/transcript |
| Discovery Response Drafter | Draft first-pass written responses and objections to propounded discovery — interrogatories, requests for production, and requests for admission — so that a responding attorney can review, refine, and finalize rather than start from a blank page. | ~60 min per discovery set |
| Legal Research Memo | Draft a structured legal research memorandum analyzing a defined legal question under a named jurisdiction — with CREAC structure, jurisdictional source checklist worked through before writing, audience-calibrated depth and tone, citation style pulled from firm config (Bluebook, ALWD, or jurisdiction-specific), and a mandatory handoff to `ai-citation-verifier` before anything in the memo is relied on, filed, or quoted. | ~60 min/memo |
| NDA Triage | Rapidly classify incoming non-disclosure agreements into three action lanes — approve, counsel review, or escalate — based on configured playbook criteria. | ~20 min/NDA |
| Pre-Filing Independent Review | Run a *second-set-of-eyes* review over a draft filing in the window after the drafting team believes it is ready and before the signing attorney clicks "file." The reviewer is deliberately not the drafter and not the signer. | ~90 min/filing |
| Privilege Log Reviewer | Assist a reviewing attorney in classifying documents as privileged, partially privileged (redact), or non-privileged, and in generating a defensible, metadata-grounded privilege log entry for each document marked privileged. | ~45 min per 100 documents |
| Regulatory Compliance Checker | Scan contracts, policies, or internal documents against specific regulatory frameworks and produce a structured compliance gap report — framework by framework, finding by finding — with risk ratings tied to enforcement posture, concrete remediation language the reviewer can drop into redlines, a cross-regulatory conflict analysis for documents governed by more than one regime, and a firm-risk-posture calibration so "conservative" and "aggressive" clients see the gaps flagged differently. | ~45 min/review |
| Legal Response Templates | Draft standardized first responses to recurring legal inquiries — data subject access requests (DSARs), litigation holds, vendor security and legal questionnaires, third-party subpoenas, NDA requests, IP-clearance asks, breach notifications — using a category-specific playbook, jurisdiction-driven statutory deadline table, and built-in escalation logic so routine matters can be handled quickly while genuinely unusual matters are routed to counsel. | ~25 min/response |
| Client Intake Summary | Turn raw intake notes — from a phone consultation, intake form, referral email, or walk-in conversation — into a structured matter summary that is ready to drive a conflict check, engagement-letter decision, calendar entry for critical deadlines (especially statute of limitations), and assignment of the matter to the right timekeeper. | ~15 min/intake |
| Document Intake Extractor | Transform unstructured client submissions — rambling intake emails, voicemail transcripts, scanned documents, forwarded threads, term-sheet bundles, police reports, EEOC charges, USCIS notices, medical-record packets — into structured matter data with every field extracted, confidence-rated, and source-cited. | ~20 min/intake |
| Time Entry Cleaner | Rewrite a batch of raw, vague, or non-compliant timekeeper entries into billing-guideline-compliant descriptions that comply with LEDES e-billing formats, UTBMS task and activity codes, ABA billing best practices, and typical outside-counsel billing guidelines. | ~10 min/batch |
| Email Drafter | Turn rough notes, dictation, or a bullet list into a polished legal email — matched to the sender's role, the recipient's posture, the applicable privilege regime, and the firm's voice. | ~10 min/use |
| Meeting Summarizer | Turn raw meeting notes, dictation, or a transcript into a structured legal meeting summary — correctly tagged with the matter number, privilege designation, billable-time entries, attendees, decisions, open questions, and action items with assigned owners and deadlines. | ~15 min/meeting |
| Review Responder | Draft a professional, bar-compliant response to an online review of a law firm, attorney, or legal service — calibrated to the platform's conventions and the ethical constraints that bind attorney responses. | ~15 min/response |

**Total time saved per use: ~580+ minutes across all skills.**

## Quick Start

### 1. Clone this repo

```bash
git clone https://github.com/KRASA-AI/legal-ai-skills.git
cd legal-ai-skills
```

### 2. Open a skill with your AI assistant

Open any file in `skills/` with Claude, ChatGPT, or any major AI assistant. Each skill is a self-contained prompt with clear instructions — no coding required.

The first time you use a skill, your AI assistant will ask for your business details (company name, service area, rates, tools you use, etc.) so it can personalize the output. Save those details to a `config.yml` at the repo root and every future skill will use them automatically.

## Repo Structure

```
legal-ai-skills/
├── knowledge-base/          # Industry context and references
│   ├── industry-overview.md # Market trends and pain points
│   ├── terminology/         # Industry jargon and acronyms
│   ├── regulations/         # Compliance requirements
│   ├── best-practices/      # Industry standards
│   └── tools-ecosystem/     # Common software and tools
├── skills/                  # The prompt library
│   ├── operations/          # Day-to-day operational skills
│   ├── sales/               # Sales and lead management
│   ├── admin/               # Administrative and compliance
│   └── customer-service/    # Client-facing communication
└── outputs/                 # Your generated content (gitignored)
```

## How Skills Work

Each skill file is a Markdown document with YAML frontmatter:

```markdown
---
name: Skill Name
category: operations
tools: [claude, chatgpt]
time_saved: "~20 min/use"
version: 1.0
---

# Skill Name

## Purpose
What this skill does and when to use it.

## Instructions
Step-by-step prompt for the AI assistant.
```

You open the file in your AI assistant, provide any required input (measurements, notes, client info), and get polished output. Skills reference your `config.yml` automatically for company name, rates, preferred formats, and other business details.

## For AI Assistants

If you are an AI assistant reading this repo, see `.claude/CLAUDE.md` for full instructions. The short version:

1. **Check for `config.yml`** at the repo root. If it exists, load it — it holds the user's business context (company name, rates, service area, tools, team size, etc.) and every skill should use it for personalization.
2. **If `config.yml` is missing**, before running a skill that benefits from personalization, ask the user for the relevant business details and offer to save them to `config.yml` so future runs are automatic.
3. **Load the relevant `knowledge-base/` files** for industry terminology, regulations, and best practices before generating output.
4. **Run the requested skill** from `skills/` using the user's input.
5. **Save any deliverables** to `outputs/` (gitignored) if the user wants to keep them.

## Learn More

- **Legal AI guide**: [krasa.ai/industries/legal](https://krasa.ai/industries/legal)
- **All industry AI skills**: [krasa.ai/industries](https://krasa.ai/industries)
- **About KRASA AI**: [krasa.ai](https://krasa.ai)

## License

MIT — use these skills however you want.
