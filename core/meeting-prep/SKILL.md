---
name: meeting-prep
description: On-demand meeting or call prep. Pulls entity context, last 30 days of activity (distilled memory, CRM activity log, meeting transcripts, email, calendar), and assembles a fixed-shape brief covering attendees, recent context, open threads, likely topics, suggested agenda, and a next action. Read-only. Use when the operator says "prep me for [X]", "call prep [X]", "meeting prep [X]", "brief me on [X] meeting", "what do I need to know before my meeting with [X]", or "get me ready for [X]". Output to chat by default. Pass `--save` to also write to /reports/meeting-prep/.
triggers:
  - prep me for
  - call prep
  - meeting prep
  - brief me on this meeting
  - brief me on the meeting
  - brief me on my call
  - what do I need to know before
  - what do I need to know about my meeting
  - get me ready for
  - prep for the call
  - prep for the meeting
  - what's the context on this meeting
context_loads:
  - /memory/entities/people/
  - /memory/entities/companies/
  - /memory/distilled/
  - /context/filing-rules.md
version: 1.0
---

# meeting-prep

On-demand prep for an upcoming meeting or call. Composes existing context layers into a fixed-shape brief. One meeting per invocation. Read-only.

This skill assumes:

- An entity layer under `/memory/entities/` (see the `entity-lookup` skill).
- A distilled memory layer under `/memory/distilled/`.
- Connector access to your CRM, calendar, meeting-transcript tool, and email. The skill is written generically. Swap connector names for your stack.

## When to use

- The operator says "prep me for [X]" or "what do I need to know before my meeting with [X]".
- A specific calendar event needs deeper context than your blanket nightly calendar-prep task produces.
- Pre-call moment, ten minutes before joining, where the operator wants the highlights without trawling memory by hand.

## When not to use

- Generic "who is X" lookups. Use `entity-lookup`.
- Drafting outreach to someone the operator has not yet met. Use the outreach skill set.
- Pipeline-wide review ("what's hot right now"). Use the weekly pipeline-review skill or task.
- Meeting summaries after the fact. Use a post-meeting digest skill.
- Bulk prep ("brief me on every meeting tomorrow"). A nightly calendar-prep task should handle this.

## Inputs

One of:

- Person name: "prep me for Jane Smith"
- Company name: "call prep Acme Corp"
- Calendar reference: "prep me for my Tuesday meeting"
- Event title: "prep me for the Acme intro call"
- Multiple attendees: "prep me for the Acme sync with Jane and Sam"

Optional flag:

- `--save` writes the brief to `/reports/meeting-prep/<slug>-<YYYY-MM-DD>.md` in addition to chat.

If the request is ambiguous (e.g. "prep me for my Tuesday meeting" with multiple Tuesday events), ask which one. Do not guess.

## Procedure

### 1. Resolve the meeting

Parse the input for attendee references and any calendar pointer.

If a calendar reference is present (date, time, event title, "Tuesday", "tomorrow"), call your calendar connector with a tight window and match.

- One match: use it.
- Multiple matches: list and ask.
- No matches: continue with named-attendee path only, note in Sources.

If a calendar event is resolved, capture: event title, start time, duration, attendee emails, location or video link, description body.

### 2. Resolve attendees and company

For each attendee in the resolved set:

1. Run the `entity-lookup` procedure inline. Compose it directly rather than sub-invoking, since scheduled tasks may not have a skill-invocation tool.
2. Capture: warmth, last_interaction, role, company, current status, next action, last 3 relationship-history items.
3. If the attendee has a company entity file, also capture that file's Current Status and Next Action lines.

Skip the operator's own email and any internal-team addresses.

If an attendee has no entity file, note the absence in the brief Sources block. Do not invent context for missing attendees.

### 3. Pull 30-day context

For each resolved attendee or company, query the four context layers with a 30-day window:

**A. Distilled memory.** Grep `/memory/distilled/` for the attendee name, email, or company name. Capture date and one-line context per hit, up to 10 most recent.

**B. CRM activity log.** If the company is a known deal, pull the last 10 activity-log entries via your CRM connector, filtered by company.

**C. Meeting transcripts.** Call your transcript connector (Granola, Fireflies, Otter, etc.) with the attendee email or name in the participants filter, last 30 days. Capture title, date, and one-line takeaway. Cap at 5 most relevant.

**D. Email.** Search your email for `from:<email> OR to:<email>` over the last 30 days. Capture subject, date, last sender, one-line context. Cap at 5 most relevant. Skip outreach-tool warm-up replies (these tend to be generic auto-replies from outreach domains).

If a connector returns an error or rate-limit, log the outage in Sources and continue. Do not crash. Do not silently drop.

### 4. Assemble brief

Use the fixed shape below. No improvisation on section order or names. The brief is deterministic in shape. The agenda and next-action lines are the only synthesised parts.

```
# Meeting prep: <attendee or topic>

<event title> | <date/time> | <duration> | <location or video link>
(omit this line if no calendar event was resolved)

## At a glance
- <Attendee 1 name>: <role> at <company>, <warmth>, last spoke <last_interaction>
- <Attendee 2 name>: ...
(one line per attendee)

## Recent context (last 30 days)
- <YYYY-MM-DD>: <abstracted bullet, source layer in parens>
- <YYYY-MM-DD>: ...
(up to 10 bullets, most recent first, deduplicated across layers)

## Open threads
- <thing still in flight that this meeting might touch>
(extracted from Current Status and Next Action lines plus CRM open items)

## Likely topics
- <topic inferred from the open threads and recent context>
(do not invent topics that are not grounded in the gathered context)

## Suggested agenda
1. <item>
2. <item>
3. <item>
(3 to 5 items. Tight. Should map back to Open threads and Likely topics.)

## Next action you should be ready to commit to
<one specific, falsifiable thing the operator should be prepared to say yes to or propose>

## Sources
- entity-lookup: <slugs hit, or "no entity file" if missing>
- distilled memory: <count of files hit over 30d>
- CRM: <deal name and activity-log entry count, or "not in pipeline">
- transcripts: <count of meetings in window, or "none">
- email: <count of threads in window, or "none">
- calendar: <event title and ID if resolved, else "no calendar event">
- outages: <list any connector that failed, else "none">
```

### 5. Save (optional)

If `--save` was passed, also write the brief to `/reports/meeting-prep/<slug>-<YYYY-MM-DD>.md`. Slug rule: prefer the company slug if all attendees share one company, else the primary attendee slug. Add `-002`, `-003` suffix on same-day collision.

Default is chat-only. Persisting every brief clutters `/reports/`.

### 6. Return

Print the brief to chat. No preamble, no trailing summary.

If the next operator message is "draft a follow-up email" or "what should I send after", route to the outreach skill set.

## Hard rules

1. Read-only across all layers. Memory, entities, CRM, transcripts, email, calendar all read-only.
2. No autonomous send. This skill produces a brief, never an email or message.
3. No invented context. Empty 30-day window for a layer means Sources says "none". Recent context lists only real, dated hits.
4. Abstract the brief. Even though it is for the operator, write it as if it could be lifted into content tomorrow without scrubbing.
5. No emdashes. Hyphens, commas, full stops only.
6. Disambiguate, do not guess. Multiple matching events, slugs, or companies sharing a name: list and ask.
7. Brief shape is fixed. Eight sections, in order, every time.
8. 30-day window is the default. Widen only on explicit instruction.

## Edge cases

- **No entity file for any attendee.** Run the `entity-lookup` missing-entity fallback. List paths in Sources. Offer to route to `create-entity` after the brief.
- **Calendar event not found.** Continue named-attendee. Sources: "calendar: no event matched".
- **Multiple matching events.** List up to 5, ask which.
- **Outreach-tool warm-up replies in email hits.** Skip. Generic auto-replies from outreach domains add noise without signal.
- **Transcript exists but no summary.** Use title and date only. Do not pull the full transcript into the brief.
- **Internal-team meeting.** Run anyway. Most context layers will be sparse. Brief still useful for surfacing recent decisions or open threads.

## Cross-skill boundaries

| Operator wants | Route to |
|---|---|
| Brief on a specific upcoming meeting | this skill |
| Canonical entity record without meeting context | `entity-lookup` |
| Tomorrow's calendar covered automatically | nightly calendar-prep task |
| Outreach draft for first contact | outreach skill set |
| Pipeline-wide review | weekly pipeline-review skill or task |
| Post-meeting summary | post-meeting digest skill |
| Create a missing entity file | `create-entity` |

## Acceptance tests

Hand-walked during build:

1. **`"prep me for Jane Smith"`.** Resolves to `/memory/entities/people/jane-smith.md`. No calendar event resolved. Brief renders with at-a-glance line, company cross-link, recent context bullets, open threads from active deal, suggested agenda grounded in open threads, full Sources block. PASS.
2. **`"call prep Acme Corp"`.** Company-first input. Brief renders with all key people in At a glance, deal context from CRM, transcript count, agenda mapped to active retainer follow-up. PASS.
3. **`"prep me for my Tuesday meeting"`.** Calendar lookup. One Tuesday event -> resolve. Multiple -> list and ask. PASS on disambiguation.
4. **`"prep me for the cold intro call"`.** No entity file for the attendee. Missing-entity fallback runs. Sources block names the absence, Recent context shows corpus mentions only. Offers to create entity at the end. PASS.
5. **Connector outage.** Transcript connector returns 500. Brief renders with that line in Sources reading "transcripts: outage at <time>", other layers populated. PASS on fail-soft.

No emdashes anywhere across all five expected outputs.

## Maintenance

Update this skill when:

- The entity schema changes. Realign with `entity-lookup` and your filing rules first.
- A new context layer comes online (e.g. a new chat connector). Add it to step 3 with its own Sources line.
- A scheduled calendar-prep task starts invoking this skill's procedure inline. Document the inline-paste pattern, since scheduled tasks may not have a skill-invocation tool.
- Operator phrases drift and new triggers should route here.

## How this skill is invoked

**Interactive (operator in chat):** the IDE composes the procedure normally. All connectors are tool-callable. Output to chat.

**From a scheduled task:** if your IDE's scheduled tasks lack a skill-invocation tool, inline the Procedure block above into the task prompt verbatim. The skill is written so the procedure copy-pastes cleanly.
