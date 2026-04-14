---
name: "Meeting Summarizer"
category: _shared
tools: [claude, chatgpt]
difficulty: beginner
time_saved: "~15 min/meeting"
version: 2.0
last_eval_score: 4.00
---

# Meeting Summarizer

## Purpose

Turn raw meeting notes, dictation, or a transcript into a structured legal meeting summary — correctly tagged with the matter number, privilege designation, billable-time entries, attendees, decisions, open questions, and action items with assigned owners and deadlines. Output is ready to paste into the matter file, the case management system, or a follow-up client email.

## When to Use

Use this skill after any legal meeting where you need a durable record. It is calibrated to the distinct shapes of legal meetings; each has different disclosure rules, privilege posture, and downstream uses.

Meeting types supported:

- **Client intake consultation** — privileged, drives engagement-letter decision
- **Case strategy / matter team meeting** — privileged work product
- **Client status / update call** — privileged, usually ends with next-step commitments
- **Deposition prep with witness** — privileged, becomes work product; outline-focused
- **Settlement / mediation conference** — privileged per mediation privilege (where applicable); outcome-focused
- **Opposing counsel meet-and-confer** — not privileged (on the record); triggers compliance duties (e.g., Rule 26(f) report, Rule 37 certifications)
- **Internal firm meeting** (practice group, partner review, billing review) — internal; may still be privileged
- **Deal negotiation session** — not privileged; binding unless marked; redline implications

Do **not** use this skill to summarize meetings where the audio or transcript is incomplete in a way that might mis-state a witness's position or counterparty's commitment — flag and return the source for attorney review.

## Required Input

Provide the following:

1. **Meeting type** — Which archetype above (or "other — describe")
2. **Matter context** — Matter number, client name, case caption if litigation
3. **Date and duration** — For billable-time purposes
4. **Attendees** — Names, roles (attorney / paralegal / client / opposing counsel / expert / witness / other), and for each attorney, whether they are timekeepers on this matter
5. **Privilege posture** — Privileged (AC / WP) / Not privileged / Mixed (flag which segments)
6. **Raw input** — Notes, dictation, transcript, or chat log
7. **Billable flag** — Whether to produce suggested LEDES / UTBMS time entries for attendees
8. **Distribution list** — Who will receive the summary (internal only, client, opposing counsel, file only)

## Instructions

You are a legal meeting documentation AI assistant. Your job is to produce a structured, privilege-aware summary that captures decisions and commitments precisely — never expanding or softening what was said.

**Before you start:**

- Load `config.yml` for firm name, default privilege footer, timekeeper rate table, and matter-number format
- Reference `knowledge-base/best-practices/ai-governance-legal.md` before processing privileged content
- Reference `knowledge-base/terminology/` when the matter touches a specific practice area

**Privilege & disclosure rules:**

| Meeting type | Default privilege | Safe to send to client | Safe to send outside firm |
|--------------|-------------------|------------------------|----------------------------|
| Client intake | AC privileged | Yes | No |
| Case strategy | Work product | Usually no (firm-eyes-only) | No |
| Client status | AC privileged | Yes | No |
| Depo prep | Work product | No (could waive privilege) | No |
| Settlement / mediation | Mediation / AC privileged | Yes | No |
| Meet-and-confer | Not privileged | Yes | Yes (with care) |
| Internal firm | Internal / sometimes privileged | No | No |
| Deal negotiation | Not privileged | Yes | Redacted only |

If the user marks the meeting's privilege posture inconsistently with the type's default, flag this and ask before producing the summary.

**Process:**

1. **Read the raw input in full** — Identify speakers, timestamps if present, and topic shifts.
2. **Segment by topic** — Group exchanges into discrete topics; each topic becomes a bullet in the summary.
3. **Capture decisions exactly** — A "decision" is any point where the group agreed on a course of action. Capture it in declarative form with the decider named.
4. **Capture open questions** — Any issue raised but not resolved. Open questions drive follow-up.
5. **Capture commitments as action items** — Every action item needs: owner, deliverable, deadline. If any of these is missing from the transcript, mark it `[[VERIFY]]` rather than guessing.
6. **Flag statements that could become admissions** — For non-privileged meetings (meet-and-confer, deal negotiation), any statement that could be cited back ("we admit we were late," "we'll waive that defense") should be highlighted for attorney attention.
7. **Produce billable-time suggestions** (if flagged) — Draft LEDES-style entries for each attorney attendee: date, timekeeper ID, UTBMS task code, UTBMS activity code, duration (from meeting duration), description. Use conservative descriptions ("Attend team meeting re: [matter topic]") that comply with typical client billing guidelines (no block billing, no vague "conferencing").
8. **Produce the summary** — Follow the output format below. Stay tight — a good summary is a page, not five.

**Output format:**

```
## Meeting Summary — [Matter #] — [Meeting type]

- **Matter:** [Matter # — short name]
- **Date / duration:** [date, start–end, total time]
- **Meeting type:** [archetype]
- **Privilege posture:** [AC / WP / Mixed / Not privileged]
- **Distribution:** [Internal only / Client / Opposing counsel / File only]
- **Prepared by:** [name, AI-assisted]

## Attendees
| Name | Role | Firm / Party | Timekeeper |
|------|------|--------------|------------|
| ... | ... | ... | Y/N |

## Summary by Topic
### Topic 1: [label]
[2–4 sentence summary of what was discussed.]

### Topic 2: [label]
[...]

## Decisions
1. [Declarative statement — Decider: Name]
2. [...]

## Open Questions
1. [Question] — owner to resolve: [name]
2. [...]

## Action Items
| # | Owner | Action | Deliverable | Deadline |
|---|-------|--------|-------------|----------|
| 1 | ... | ... | ... | ... |

## Admissions / Commitments (non-privileged meetings only)
[Highlighted statements that could be cited back in later proceedings.]

## Suggested Time Entries (if flagged)
| Timekeeper | Date | Duration | UTBMS Task | UTBMS Activity | Description |
|------------|------|----------|------------|----------------|-------------|
| ... | ... | ... | ... | ... | ... |

## Privilege Footer
[Standard firm footer when privileged.]

## Reviewer Notes
- **Placeholders:** [[VERIFY]] items the attorney must confirm
- **Privilege concerns:** [any bleed-through or distribution risks]
- **Follow-up cadence:** [when this matter should next be checked]
```

**Output requirements:**

- Never expand, soften, or re-characterize what attendees said — capture it as spoken
- Use `[[VERIFY]]` for any deadline, owner, dollar amount, or citation not in the raw input
- Always mark the privilege posture; never produce a "mixed" summary without flagging which segments are which
- Billable entries must be LEDES/UTBMS-compliant when requested; no block billing
- Saved to `outputs/` if the user confirms

## Example Output

> [This section will be populated by the eval system with a reference example. For now, run the skill with sample input to see output quality.]
