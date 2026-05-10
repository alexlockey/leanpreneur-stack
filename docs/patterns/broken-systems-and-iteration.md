# Broken systems and iteration

A two-part operating pattern for any agentic system that calls external tools or APIs. Stops your agent wasting calls on things that are already known to be down, and gives it a discipline for what to do when something fails.

## The pattern

### Part 1: Known Broken Systems table

Maintain a single, named table somewhere your skills and scheduled tasks can read. When a tool, API, or integration is broken, log a row. The point of the table is to **stop wasting calls on things you already know are down.**

```
| System | Status | Discovered | Symptom | Workaround | Notes |
|---|---|---|---|---|---|
| {{TOOL_NAME}} | BROKEN | YYYY-MM-DD | What goes wrong | What to do instead | Optional |
```

**Status values:**

- `BROKEN` — do not call. Use the workaround.
- `DEGRADED` — call works, but slow or unreliable. Add timeout headroom.
- `WORKING` — remove from the table.

**How to use it:**

- Before any skill or scheduled task invokes an external system, scan this table.
- If the tool is listed as `BROKEN`, skip it and use the named workaround.
- If a tool fails twice in a row in normal operation, add a row rather than retrying a third time.
- Only humans (or a build session reviewing with a human) mark a tool as `BROKEN`. Skills and tasks read; they do not write to this table.

### Part 2: Iteration rule

When a tool call fails, follow this sequence:

1. **Read the error.** What actually went wrong? (auth / rate limit / network / schema mismatch / known bug)
2. **Check the Known Broken Systems table.** If listed, use the workaround. Do not retry.
3. **Try one alternative approach.** Different endpoint, different model, different tool, different framing. Document what you tried.
4. **Try a second alternative.** Same as step 3 — different angle.
5. **Only after three distinct attempts**, report failure to the calling context. Include what was tried and what the errors were.

**Never:**

- Retry the same broken approach more than once.
- Retry a tool already listed in Known Broken Systems.
- Report failure after a single attempt.

## Why it matters

Two failure modes the rule fixes.

**Repeated calls into known-broken systems.** Every retry into a 401-locked API costs you a token round-trip, a surfaced error in your logs, and (often) attention from the human in the loop. Multiplied across a fleet of skills and scheduled tasks, this is real waste.

**Single-attempt failure reporting.** The opposite mode: skill hits a 429, immediately reports "X is broken." Half of those failures are recoverable with a different angle (different endpoint, different model, different framing). The three-attempt floor catches the recoverable ones without infinite-looping on the unrecoverable ones.

Three attempts is the floor, not the ceiling. More is fine if the alternatives are genuinely different. The discipline is: alternatives, not repetition.

## How to adopt

In a Claude Code or similar agentic-IDE setup:

1. Create a Known Broken Systems table in your tools registry (wherever you keep `/context/tools.md` or equivalent). Empty at adoption — populated as failures surface.
2. Add the iteration rule to your operating rules file (wherever you keep `/context/rules.md`, `AGENTS.md`, or equivalent).
3. Reference both from skills that call external systems. Skill authors should know the rule exists and write their own error-handling against it.

Do not require an automation gate. This is a cultural rule — the value is the discipline, not the enforcement.

## Example

```
| System | Status | Discovered | Symptom | Workaround | Notes |
|---|---|---|---|---|---|
| Firecrawl | BROKEN | 2026-03-06 | 402 errors, credits exhausted | Use Tavily instead | |
| Brave Search API | BROKEN | 2026-03-06 | No API key configured | Use Tavily | Permanent fallback, not transient |
| Posts API | DEGRADED | 2026-04-20 | 500 on `/posts/list` endpoint, other endpoints OK | Skip post-listing in monthly tool ROI scan; mark row as `pending instrumentation` | Vendor said fix coming |
```

## Provenance

This pattern was lifted from a previous-generation agent OS that ran for several months across four businesses before being retired. The "Known Broken Systems" table consistently saved minutes of every operator's time per day. The iteration rule was added after a series of failure cascades where skills reported failure on first 401 instead of trying alternatives. Both made it into the harvest set when the runtime was retired.
