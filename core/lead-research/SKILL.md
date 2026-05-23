---
name: lead-research
description: Research a warm-pool prospect from public signals and draft a workflow-spotting outreach message in your own voice, for manual review and manual send. Use when you want to turn a list of warm contacts into ready-to-personalise first messages without doing the pre-research by hand. Drafts only, never sends. Names a specific broken workflow before naming any offer.
triggers:
  - draft warm outreach
  - lead research
  - research and draft for
  - warm pool drafts
  - who should I message
context_loads:
  - your warm-pool file (a list of prospects with segment and source)
  - your outreach message kit (your DM variants, banned words, checklist)
  - your ideal-customer-profile config
  - your do-not-contact list
version: 1.0
---

# lead-research

Turns a warm-pool entry into a ready-to-personalise outreach draft. The skill does the few minutes of pre-research per prospect and writes a first message in your voice. You still send manually. There is no send path.

This is the warm track: people you already have some signal toward (past clients, network introductions, people who engaged with your content, a re-engagement shortlist). It is deliberately separate from any cold, list-bought pipeline. The premise is low volume and high personalisation: the message itself is the experiment, not the system around it. Reply rate is the metric, not send rate.

The job in one line: name a specific broken workflow before naming any offer, in a message that passes your personalisation checklist.

## When to use

- You want warm outreach drafts, or you name a specific warm-pool prospect to draft for.
- A scheduled task fires to produce a small daily batch for review.

## When not to use

- Cold strangers from a bought list. That is a different, higher-volume pipeline with different rules.
- People you have an active relationship with (live threads, partners, community members). Tag them on a partnership or community track and exclude them. Cold-drafting someone you already know damages the relationship.
- Sending. This skill never sends.

## Prerequisites

You maintain four things. Adapt the paths to your layout.

1. A warm-pool file (JSON or Markdown) with one record per prospect. Suggested fields: `id`, `name`, `firm`, `title`, `segment`, `headcount`, `source`, `connection_point`, `track`, `last_contacted`, `status`.
2. An outreach message kit: your first-message templates per buyer segment, your banned-words list, your personalisation checklist, your follow-up cadence.
3. An ideal-customer-profile config: who you sell to, the size band, the verticals, the proof points.
4. A do-not-contact list: people you have explicitly decided against. This file is authoritative and overrides any pool record.

## The pattern

### 1. Select (deterministic, no judgment)

A small script loads the pool and drops anyone excluded before any model is called:

- on the do-not-contact list (name match)
- on an excluded track (partnership, community, or any active relationship)
- inside your re-contact cooldown window
- already drafted and awaiting send
- missing required fields, or outside your size band

It then selects up to a daily cap (keep it small, five to ten), assigns a message variant from the segment, and writes a research worklist. Keeping selection and exclusion in code, not prose, is what makes the safety rails reliable. The judgment work happens next, on a clean, pre-filtered shortlist.

### 2. Research each prospect

For each work order, pull public signals only: website, professional profile, recent posts, hiring activity, recent news. Read any prior notes you hold on them.

Then identify the ONE workflow most likely broken at this firm's size and shape. Do not list three. Pick one from the signals. Size-banding matters: a workflow that is painful at 30 people is invisible at 5 and already solved at 500.

### 3. Draft the message

Use the variant the worklist assigned. A useful default set of segments:

- A founder-led firm in your core vertical, owner-operator who personally feels the pain.
- A mid-market functional leader (operations, revenue, talent) where the capability is a clear lever and decisions move faster than a full leadership team.
- An adjacent professional-services firm where your core playbook ports with light changes.
- A past client you have worked with before. Here the opener leads with the relationship, not a cold intro, then names a workflow likely broken now. This is often your warmest and highest-converting segment, so keep a distinct reopener template for it.

Write in your own voice from the matching template. Name the workflow in operator language. Reference the specific connection point. One ask, one question.

Route model calls through your model-call abstraction so the model choice lives in one place: a precision tier for the draft, a fast tier for quick signal triage.

### 4. Apply the personalisation checklist

Every draft passes all of it before saving. A strong default:

- First name correct and used naturally, not as a tag
- Firm name appears at least twice
- One specific workflow named in operator language
- One specific connection point or referral source
- Single ask, single question
- No marketing-cliche banned words (transform, unlock, leverage as a verb, game-changer, next level, world-class, paradigm), plus any terms specific to your positioning you want to avoid
- No em-dashes (use commas, full stops, "to")
- No exclamation marks
- Consistent regional spelling
- Under a tight word cap for the first message
- One link maximum

Save each draft as its own file with a short header (name, firm, variant, the workflow you named, the source) above the body, so you have the context at a glance.

### 5. Register

A second deterministic pass rebuilds a dated batch review file from the draft files, so the day's drafts surface in your approval queue. You review, personalise, and send manually.

### 6. Report back

One short summary: how many drafted, the variant split, anyone flagged (off-band size, weak connection point, deliverability doubt), and the review file path.

## Cadence

A simple, humane cadence: first message, one follow-up after about a week, a final note after about two weeks, then stop. Do not chase silence. v1 of this skill drafts first messages; add follow-up drafting once you know your first-message reply rate.

## Hard rules

1. Drafts only. Never send. There is no send path.
2. Cross-check the do-not-contact list first. It overrides any pool record. If a flagged contact is read, exclude silently.
3. Exclude partnership and community track. Never cold-draft an active relationship.
4. No em-dashes anywhere in output.
5. Name a specific workflow before naming any offer. If you cannot name one, do not draft. Lead with the problem, not the product.
6. One link maximum.

## Why a script plus a prompt

The deterministic parts (loading the pool, applying exclusions, deduping, selecting, building the review file) belong in a small script so the safety rails cannot be skipped on a busy day. The judgment parts (reading signals, choosing the one workflow, writing in voice) belong in the model call. Splitting them keeps each side small and auditable. This mirrors the broader two-pass convention across this library: a mechanical pass narrows the field, a judgment pass does the work.

## Maintenance

Update this skill when your message kit revises (variants, banned words, checklist), when the warm-pool schema changes, or when you add follow-up drafting.
