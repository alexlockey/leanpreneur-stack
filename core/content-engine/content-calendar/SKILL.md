---
name: content-calendar
description: Build a weekly content calendar across LinkedIn, X, blog, newsletter, and video for a given window. Inputs: start date, weeks (default 4), platform frequencies (parameterised), content pillars from the operator's voice file, scheduled interviews or events. Output: a structured calendar with dates, pillars, hooks, posting windows, lead-magnet placement, and interview derivative pipeline. Use when the operator says "build content calendar", "content planning", "editorial calendar", "posting schedule", "what to post", "weekly content plan", or "plan my content". One window per invocation. Does not draft individual posts.
triggers:
  - build content calendar
  - content planning
  - editorial calendar
  - posting schedule
  - what to post
  - weekly content plan
  - plan my content
  - content schedule for next month
context_loads:
  - /memory/voice/content-pillars.md
  - /memory/voice/voice-rules.md
version: 1.0
---

# content-calendar

Generate a structured posting calendar across platforms for a given window. Composes the operator's content pillars, platform frequency defaults, and any scheduled interviews into a deterministic table. The output is the plan, not the posts.

This skill assumes the operator maintains a content-pillars file under `/memory/voice/content-pillars.md` listing three to five named pillars. Adapt the path if your layout differs. The skill works without the file (defaults below), but the calendar drifts toward generic if pillars are absent.

## When to use

- The operator needs a 1 to 8 week plan across multiple platforms.
- A new content campaign or product launch needs scheduled coverage.
- An interview is booked and the derivative pipeline needs slots.
- The operator is staring at a blank week and asks "what should I post".

## When not to use

- Drafting individual post copy. Use post-generator skills.
- Analysing post performance after the fact. Use a performance-analytics skill.
- One-off post for today only. Just write it.
- Long-range strategy (quarterly themes, annual content roadmap). This skill is tactical, weekly.

## Inputs

Gather these before generating:

1. **Start date.** Default: the next Monday.
2. **Window.** Number of weeks. Default: 4.
3. **Platform frequencies.** Defaults below. Operator overrides if needed.
4. **Pillars.** Read from `/memory/voice/content-pillars.md` if present. Otherwise ask for 3 to 5.
5. **Scheduled events.** Interviews, product launches, talk slots, community milestones that the calendar should build around.

If pillars are missing and the operator declines to name them, fall back to a single generic pillar set (Lean operations, Practical AI, Build in public). Note the fallback in the output.

## Procedure

### 1. Confirm intake

Restate inputs back to the operator in one block. Show the dates explicitly (real dates, not "Week 1 Monday"). If they correct anything, take the correction before generating.

### 2. Apply the ratio framework

Apply these defaults unless the operator overrides:

- 70% core (the operator's primary pillars).
- 20% adjacent (industry trends, tools, case studies, partner features).
- 10% personal (behind the scenes, reflections, wins, vulnerability).
- 80% value posts to 20% conversion posts. Space conversion posts 3 to 4 value posts apart.

### 3. Apply platform defaults

Use these unless the operator overrides:

| Platform | Frequency | Best days | Best time | Notes |
|---|---|---|---|---|
| LinkedIn | 3 to 4 per week | Tue, Wed, Thu | 08:00 to 10:00 local | Max one post per day. 30 to 60 min engagement window after publish. Algorithm window 60 to 90 min. |
| X | 3 to 5 per week | Daily | 08:00, 12:00, 17:00 | Threads, replies, reposts count. High velocity. |
| Video | 2 per week | Mon, Thu | 08:00 launch | 8 to 12 min typical. Interview plus educational. |
| Blog | 1 per week | Tue or Wed | 10:00 publish | Long form. Repurposed from interviews or newsletter. |
| Newsletter | 1 per week | Thu evening | 18:00 send | 400 to 800 words. |

### 4. Build the calendar table

Use the shape below. Every row is one scheduled post or content moment. Use real dates.

```
| Date | Day | Platform | Pillar | Format | Hook preview | Type | Source |
|------|-----|----------|--------|--------|--------------|------|--------|
| <YYYY-MM-DD> | Mon | Creation | -- | Write + plan | Plan week, outline interviews | -- | Placeholder |
| <YYYY-MM-DD> | Wed | Interview | <pillar> | Interview record | <guest>, <topic> | Value | Record |
| <YYYY-MM-DD> | Thu | LinkedIn | <pillar> | Long-form | <one-line hook> | Value | Publish |
| <YYYY-MM-DD> | Fri | X | <pillar> | Thread | <thread topic> | Value | Write |
| <YYYY-MM-DD> | Mon | Blog | <pillar> | Repurpose | <interview-derived blog post> | Value | Repurpose (D+5) |
```

Rules for the table:

- Real dates, not "Week 1 Monday".
- Mark creation days (Mon write and plan, Wed interview) separately from derivative days (repurposed content).
- Best posting window for LinkedIn: 08:00 to 10:00 local time on Tue, Wed, Thu.
- Link interview derivatives: blog at D+2 from record, LinkedIn snippets at D+3 and D+5, video clips at D+7, newsletter feature at D+7.

### 5. Schedule lead-magnet conversion posts

Conversion posts go on every fourth value post. Mark them with the offer, the keyword, and the CTA hook angle.

Use this shape:

```
| Date | Platform | Pillar | CTA | Offer | Keyword | Hook |
|------|----------|--------|-----|-------|---------|------|
| <YYYY-MM-DD> | LinkedIn | <pillar> | "Comment to receive" | <offer> | <topic keyword> | <one-line hook> |
```

Default offers (operator overrides if needed):

- Newsletter sign-up (top of funnel, no friction).
- Community membership (mid-funnel, recurring revenue).
- Service conversation (bottom of funnel, sales call).

### 6. Integrate interviews into the derivative pipeline

If interviews are scheduled, use this rhythm:

- **Wed 14:00 local:** record interview, 45 to 60 min.
- **Fri D+2:** publish blog post, 1,200 to 1,500 words, SEO-optimised.
- **Mon D+3:** LinkedIn post 1, quote snippet plus link.
- **Wed D+5:** LinkedIn post 2, different angle plus link.
- **Fri D+7:** video clips, 3 to 5 clips of 60 to 90 seconds each, scheduled across the platform.
- **Thu D+7:** newsletter feature, guest intro plus key insight.

### 7. Seed week 1 with starter hooks

If the operator has no draft hooks lined up for week 1, seed each pillar with one starter hook. Format: "<one-line hook tied to the pillar>".

Generic templates per pillar (adapt to the operator's actual pillars):

- Pillar 1 (operations, margin, leverage): "<concrete number> is where most <audience> hit a ceiling. Here is the system that breaks it."
- Pillar 2 (practical AI, tools, workflows): "Your <tool> prompts will improve <percentage> just by fixing one thing. It is not complexity."
- Pillar 3 (story, identity, pivot): "I spent <duration> doing <old identity>. This is what I should have built instead."
- Pillar 4 (build in public, evidence): "We hit <milestone> this month. Here is what changed the <metric>."
- Pillar 5 (personal): "Spent the morning <activity>. Why I will not let this become a black box."

### 8. Apply the pre-publish checklist

Append this block to the calendar output for the operator to apply on each post before scheduling:

- [ ] Pillar ratio correct (70/20/10, 80/20 value to conversion).
- [ ] Hook strong and specific, not generic.
- [ ] CTA clear if conversion post.
- [ ] Image or video attached for LinkedIn and X.
- [ ] Link valid if blog or landing page.
- [ ] Formatted for readability (line breaks, bold, bullets).
- [ ] Scheduled for best-time window.
- [ ] 30 to 60 min engagement block on calendar post-publish.
- [ ] Post ladders to current business objective.

### 9. Self-critique

Before returning the calendar, verify:

- [ ] Real dates, not week-number placeholders.
- [ ] Narrative arc across the week, not random posts.
- [ ] Interviews feed the derivative pipeline (2 per week creates 8 to 10 derivative posts).
- [ ] Conversion posts spaced every 4 value posts, not clustered.
- [ ] Week ladders to a stated business objective.
- [ ] Engagement blocks on the calendar after each publish.
- [ ] Hooks specific and testable, not generic ("AI is the future" fails).
- [ ] Ratios land (70% core).

### 10. Return

Output the calendar table, lead-magnet schedule, interview pipeline, week 1 starter hooks (if seeded), and the pre-publish checklist. No preamble, no trailing summary.

If the operator's next message is "write the first post", route to the hook-writer skill plus the relevant post-generator skill.

## Hard rules

1. Real dates. Never "Week 1 Monday".
2. Mix all pillars across the window. No pillar absent for more than 7 days.
3. Conversion posts spaced 3 to 4 value posts apart. Never two conversion posts back to back.
4. No emdashes anywhere in the output. Hyphens, commas, full stops only.
5. The calendar is the plan, not the posts. Do not draft post bodies inside this skill.
6. Interview derivative pipeline is non-negotiable when an interview is scheduled. One interview produces five derivative posts.

## Edge cases

- **Operator has no content-pillars file.** Use the fallback pillar set (Lean operations, Practical AI, Build in public). Note the fallback at the end and suggest creating `/memory/voice/content-pillars.md`.
- **No interviews in the window.** Skip the derivative pipeline section. Note "no interviews scheduled" in Sources.
- **Window longer than 8 weeks.** Push back. Tactical calendars work at 1 to 8 weeks. For quarterly or annual planning, use a separate strategy skill.
- **Single-platform invocation.** Drop the multi-platform table. Run the same procedure for the one platform only.
- **Operator overrides platform frequency to zero on a platform.** Drop that platform entirely from the table.

## Acceptance tests

Hand-walked during build:

1. **"Build content calendar starting next Monday for 4 weeks, LinkedIn 4 per week, newsletter Thursday, one interview Wed week 2."** Returns 4-week table with real dates, 16 LinkedIn posts, 4 newsletters, 1 interview slot, 5 derivative posts pipelined from the interview, conversion posts every 4 value posts. PASS.
2. **"Plan my content for May, all platforms."** Returns 4-week May calendar across all platform defaults. PASS.
3. **No pillars file, no operator pillars given.** Returns fallback pillar set, notes the fallback at the end. PASS.
4. **"Plan content for next 12 weeks."** Pushes back. Suggests breaking into three 4-week tactical windows or routing to a strategy skill. PASS.
5. **No emdashes across all expected outputs.** PASS.

## Cross-skill boundaries

| Operator wants | Route to |
|---|---|
| Weekly plan across platforms | this skill |
| First line for one piece | `hook-writer` skill |
| Closing line or CTA | `cta-handraiser` skill |
| Repurpose an existing piece across platforms | `content-repurposer` skill |
| Convert a video or interview transcript into posts | `video-to-content` skill |
| Quarterly themes or annual roadmap | strategy skill (out of scope here) |
| Post performance analysis | analytics skill |

## Maintenance

Update this skill when:

- The operator's pillars change. The fallback list in the procedure is the safety net.
- A new platform enters regular use. Add a row to platform defaults.
- Posting-window data shifts (algorithm changes, audience timezone). Update best-time defaults.
- Operator phrases drift and new triggers should route here.
