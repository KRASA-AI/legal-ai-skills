# Agentic Legal Workflow Design

This knowledge base entry describes how to compose the skills in this repository into multi-step, agentic workflows — where an AI system plans, executes, and iterates across several skills with human review gates between steps — without losing attorney supervision, privilege protection, or defensibility. It is a design reference, not a skill. Skills in `skills/operations/`, `skills/admin/`, and `skills/customer-service/` should cross-reference this document whenever they describe how their output flows into the next step.

## Why this matters in 2026

Coverage across Bloomberg Law, ABA Law Practice Magazine (March/April 2026), Wolters Kluwer's *Future Ready Lawyer 2026* study, and Legora's *Year of Agents in Legal AI* analysis converges on the same observation: legal AI is moving from single-task prompting to orchestrated agent workflows in which multiple specialized steps run in sequence, each grounded in matter context and firm playbooks. Gartner projects that 40% of enterprise applications will embed task-specific AI agents by the end of 2026, up from under 5% at the start of 2025.

For legal teams, this shift raises two hard problems that a naive agent loop does not solve on its own:

1. **Supervision and defensibility.** An attorney of record is accountable for every decision made in the attorney's name. An agent that chains multiple steps without human gates converts a reviewable decision into an opaque series of actions.
2. **Privilege and work product boundaries.** An agent that moves documents between tools, writes intermediate artifacts to shared storage, or invokes consumer-grade AI for any step may erode privilege or create discoverable work product. The framework in `ai-privilege-and-work-product.md` applies to every link in the chain, not just the first one.

The guidance below is designed to let a firm adopt agentic workflows without giving up either.

## Core design principles

**Skill-as-step.** Treat each skill in this repository as a single, auditable step in a larger workflow. A skill has a defined input contract, a defined output contract, and a disclaimer that it is AI-assisted and requires attorney review. Chaining skills means chaining those contracts — never skipping the disclaimers or the review gates.

**Human review gates are mandatory between phases, not between every step.** A design that puts a human gate between every step reduces the workflow to a sequence of manual prompts and captures no leverage. A design that removes all gates forfeits defensibility. The right pattern is a small number of phase boundaries — typically intake, analysis, drafting, and review — with a human gate at each boundary and full traceability within each phase.

**Matter context is a first-class input to every step.** Every skill already accepts matter context as part of its `Required Input`. In an agent workflow, that context is carried forward from the first skill to the last, including the matter ID, the client identity, the governing jurisdiction, any protective order and Rule 502(d) order status, the applicable playbook, and the privilege posture. The orchestrator is responsible for ensuring this context is passed through every step without alteration.

**Playbook grounding beats prompt cleverness.** The Contract Clause Analyzer, NDA Triage, and Legal Response Templates skills all accept a configured playbook. An agent workflow should prefer playbook-configurable skills over one-off prompts, because the playbook is the durable, firm-specific artifact that makes the output defensible across matters.

**No tool changes without logging.** Whenever the agent switches from one AI tool to another, or from an enterprise AI to a non-enterprise AI, the switch must be logged with the reason. This is the single most important discipline for preserving privilege posture across a multi-step workflow.

## A canonical four-phase pattern for litigation matters

The following pattern is drawn from current practice at firms that have publicly described their agentic pilots (Artificial Lawyer coverage of April 2026) and is consistent with the ABA Law Practice Magazine's March/April 2026 introduction to agentic AI. It is organized around phase boundaries rather than individual steps.

**Phase 1 — Intake and framing.** Skills: `admin/client-intake-summary`, `admin/document-intake-extractor`, `operations/legal-research-memo` (issue-framing mode). Output: a structured matter record with parties, claims, governing jurisdiction, known deadlines, gaps, and an open-question list. Gate: the responsible attorney reviews and approves the matter record, sets the privilege posture, and confirms the protective-order and Rule 502(d) status before Phase 2 begins.

**Phase 2 — Analysis.** Skills: `operations/regulatory-compliance-checker`, `operations/contract-clause-analyzer`, `operations/privilege-log-reviewer` (for internal investigations), `operations/legal-research-memo` (full memo mode). Output: a set of analytical artifacts keyed to the matter record. Gate: a senior attorney reviews the analysis for substantive accuracy, flags any ESCALATE items, and authorizes drafting in Phase 3.

**Phase 3 — Drafting.** Skills: `operations/demand-letter-drafter`, `operations/discovery-response-drafter`, `operations/deposition-prep-outline`, `customer-service/legal-response-templates`. Output: court-filed or client-delivered drafts with all `[[CLIENT TO PROVIDE]]` and `[[VERIFY]]` placeholders resolved. Gate: final attorney review, client sign-off where required, and service.

**Phase 4 — Closeout and knowledge capture.** Skills: `admin/time-entry-cleaner`, `_shared/meeting-summarizer`, plus a playbook-update step (not yet a standalone skill — captured in the changelog for now). Output: clean time entries, a matter summary, and any playbook updates flowing back into the firm's configuration. Gate: billing partner sign-off on time entries and playbook-change approval by the practice group lead.

A comparable pattern for transactional and in-house workflows substitutes different skill sets — `nda-triage` at intake, `contract-clause-analyzer` in analysis, `legal-response-templates` in drafting — but preserves the same four-phase boundary structure.

## Orchestrator responsibilities

Whatever system runs the workflow — a human paralegal, an in-house workflow tool, or an agent framework — it has the same responsibilities at each phase boundary:

1. **Validate inputs before the next skill runs.** A skill with missing required input should not be invoked; the orchestrator should surface the gap to a human.
2. **Pass matter context forward unchanged.** Any change to jurisdiction, privilege posture, or protective-order status must come from an attorney instruction, not from an agent's inference.
3. **Log every step.** Each step's input, output, skill version, and reviewing attorney (where applicable) is recorded in a run log stored with the matter file. The log is the audit trail that supports both internal QC and discovery responses about the firm's AI use.
4. **Stop on ESCALATE.** Any `ESCALATE` classification anywhere in the workflow halts the agent until a human resolves it. The `nda-triage` and `privilege-log-reviewer` skills produce `ESCALATE` calls deliberately; the orchestrator must honor them.
5. **Preserve privilege at tool boundaries.** If the orchestrator must move an artifact between tools — for example, from the drafting skill to a document management system — it must confirm the destination is covered by the firm's privilege-preserving tool whitelist before the move.

## Review-gate discipline

Each phase gate has a minimum review standard. These are intentionally conservative; firms can relax them for low-stakes matters, but relaxations should be documented in the firm's AI governance policy (see `ai-governance-legal.md`).

- **Intake gate:** A licensed attorney reviews the matter record. Mandatory sign-off on jurisdiction, privilege posture, and deadlines.
- **Analysis gate:** A senior attorney reviews analytical artifacts. Mandatory sign-off on any compliance-gap findings, any privilege-log PRIVILEGED or REDACT classifications, and any regulatory-exposure findings.
- **Drafting gate:** The attorney of record reviews every draft before service or filing. Mandatory sign-off — no exceptions. This is the step that maps to FRCP 11 and its state equivalents.
- **Closeout gate:** The billing partner reviews cleaned time entries; the practice group lead approves any playbook changes flowing back from the matter.

## Failure modes to watch for

Based on the reported pilots and near-misses through April 2026, the common agentic failure modes are:

- **Silent step upgrade.** An orchestrator swaps a skill's version mid-matter without flagging the change; the output now reflects a different framework than the one approved at the intake gate. Guard: version every skill (already in the frontmatter) and log the version used at each step.
- **Context drift.** Matter context is paraphrased rather than passed through, and a jurisdictional detail quietly changes. Guard: the matter record is the single source of truth and is not rewritten by any downstream skill.
- **Privilege leak by convenience.** An agent copies a draft to a consumer AI tool for "quick cleanup," destroying the enterprise-tool chain. Guard: tool whitelist enforced by the orchestrator, not by the user.
- **Overclaim cascade.** A privilege call in Phase 2 propagates into Phase 3 drafting without a re-review. Guard: every PRIVILEGED or REDACT call has its own attorney sign-off and is not re-used without that sign-off.
- **Speed-to-served pressure.** Because the agent produced a draft fast, the responsible attorney feels comfortable compressing review time. Guard: the drafting gate's minimum review standard is absolute — no agent speed justifies skipping it.

## Notes on watch — agent-counterparty transactions

The framework above covers AI agents acting *inside* a firm's workflow, with the firm and its attorneys as the principals. A separate vector is now appearing in the public commentary: AI agents acting as counterparty proxies in transactions — placing offers, accepting offers, and binding their human principals to the terms reached. Anthropic's April 24, 2026 *Project Deal* experiment, in which 69 employees authorized AI agents to execute purchase-and-sale exchanges on their behalf and 186 deals closed at a total transaction value of just over $4,000, is the public marker for this pattern. The Legal IT Insider commentary on the experiment frames the legal-frameworks gap explicitly: contract formation, capacity, authority, and enforceability rules were not drafted with autonomous agents in mind.

This pattern is upstream of the skills in this repository — none of the skills in `operations/`, `admin/`, or `customer-service/` is designed to advise counterparties whose agents are themselves AI. But it surfaces issues that the firm should be ready to address when a client asks, when the firm itself enters an agent-mediated transaction (procurement, marketplace, or platform-routed deal), or when an agent-mediated transaction surfaces in litigation:

- **Authority and ratification.** Did the principal authorize the specific transaction or only a class of transactions? Is there a signed ratification step, and if so does it cure an out-of-scope agent action? The firm's counterparty work should not assume the answer is yes by default.
- **Formation and assent.** Whose objective manifestation of assent counts when both sides of the negotiation are AI? Click-through analogues (the National Law Review's "Who's Really Clicking 'Accept'?" framing) and the older mailbox-rule tradition both apply imperfectly. Counsel should review and document the formation theory the client is relying on before the platform launches.
- **Identity, KYC, and binding signature.** Where an agent acts on behalf of a regulated principal (broker-dealer, escrow agent, lender), the agent's identity, its authority, and its signature are open compliance items. Existing KYC and signature-block practice should be updated to address agent-mediated transactions explicitly.
- **Liability allocation.** Where an agent over-pays, under-delivers, leaks confidential terms, or is induced via prompt injection, allocation across principal, deploying organization, and tool vendor is unsettled. Standard indemnities and limitations of liability written for passive software typically do not cover the agent case.
- **Audit and traceability.** Records of prompts, decisions, and tool calls become primary evidence when an agent transaction is challenged. The orchestrator-responsibility points above (validate inputs, log every step, stop on ESCALATE) translate directly into the agent-counterparty setting; firms advising clients on agent deployments should require the same orchestration discipline of the client.

This is a notes-on-watch item, not a skill. The repo's `contract-clause-analyzer`, `nda-triage`, and `regulatory-compliance-checker` skills already carry the matter-context fields a future skill could extend; the practical near-term step is to expect counterparty AI-agent provisions in client requests for advice and to draft a firm-level take on each of the five items above before the first matter forces it.

## Open questions

- Appellate treatment of AI-generated work product after *United States v. Heppner* (S.D.N.Y. Feb 2026) — update `ai-privilege-and-work-product.md` as decisions come in.
- The right audit-log format for agent workflows in eDiscovery — whether a dedicated run log produced by the orchestrator is itself discoverable, and whether it should be work product or a business record.
- Whether a firm should disclose agentic workflow use to clients in the engagement letter, beyond the existing AI-use disclosure. Current practice is split.
- How to map agentic workflow time to LEDES/UTBMS billing codes given that an agent may complete in minutes what was historically billed in hours.
- Agent-counterparty transactions (above): when this pattern produces its first reported dispute, the formation, authority, and liability allocation questions move from notes-on-watch to a candidate stand-alone KB entry.

## Cross-references

- `knowledge-base/best-practices/ai-governance-legal.md` — the governance and risk framework that every phase gate must honor
- `knowledge-base/best-practices/ai-privilege-and-work-product.md` — the privilege and work-product framework applied at every tool boundary
- `skills/operations/privilege-log-reviewer.md` — the skill that produces `ESCALATE` calls which the orchestrator must honor
- `skills/operations/discovery-response-drafter.md` — the skill that consumes privilege calls and must not re-use them without attorney sign-off
- `skills/operations/nda-triage.md`, `skills/operations/contract-clause-analyzer.md`, `skills/customer-service/legal-response-templates.md` — playbook-configurable skills that are preferred inputs for agent workflows

*Last updated: 2026-04-27.*
