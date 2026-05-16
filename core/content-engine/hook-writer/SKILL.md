---
name: hook-writer
description: Generate ten candidate hooks for a piece of content across LinkedIn, X, blog headline, newsletter subject, or video title. The hook is the first line that decides whether the rest gets read. Use when the operator says "write a hook", "hook for", "opener for", "subject line for", "headline for", "first line", or "scroll stopper". One topic per invocation. Returns ten options across four pattern families, with platform fit flags and a self-critique pass. The operator picks. Does not write the body of the piece.
triggers:
  - write a hook
  - hook for
  - opener for
  - subject line for
  - headline for
  - first line
  - scroll stopper
  - give me a hook for
context_loads:
  - /memory/voice/banned-vocabulary.md
  - /memory/voice/voice-rules.md
model: precision
version: 1.0
---

# hook-writer

The hook is most of the battle. If the first line fails, the rest of the piece is invisible.

This skill produces ten hook candidates across four pattern families for one topic, flags which work best on which platform, runs a self-critique pass, and asks the operator to pick. It does not write the body of the piece. It does not select on the operator's behalf.

This skill assumes the operator maintains a banned-vocabulary list and a voice-rules file under `/memory/voice/`. Adapt the paths if your layout differs. The skill works without them, but the output drifts toward generic AI marketing copy if the constraints are missing.

## When to use

- The operator has a topic and needs the first line.
- A post draft is ready but the opener feels weak.
- A newsletter subject line, blog title, or video title is needed.
- The operator pastes a piece and says "rewrite the hook".

## When not to use

- Drafting the full post or article body. Use a post-generator skill for that.
- Writing CTAs or closing lines. Use the `cta-handraiser` skill.
- Picking which hook to ship. The operator picks. This skill presents options.
- Repurposing an existing piece across platforms. Use the `content-repurposer` skill.

## Inputs

Gather these before generating:

1. **Topic.** What is the piece about. One sentence.
2. **Audience.** Who is the reader. Operator, founder, ops lead, specific role.
3. **So what.** Why should the reader care right now. The tension, problem, or insight.
4. **Anchor.** Any number, story, case study, or moment to ground the hook in.
5. **Platform.** LinkedIn, X, blog headline, newsletter subject, video title, or multi-platform.

If any of these are missing, ask. Do not generate hooks against blank fields.

## Procedure

### 1. Confirm intake

Restate the five inputs back to the operator in one block. If they correct anything, take the correction.

### 2. Generate ten hooks across four patterns

Mix the ten across these four families. Aim for roughly two to three per family, weighted by what fits the topic.

**Pattern 1: Contrarian (pattern interrupt).**

State the opposite of what the audience believes. Challenge the default assumption.

- "X is wrong. Here is why."
- "Stop doing X. It is costing you Y."
- "Everyone tells you to X. That is backwards."

**Pattern 2: Specificity (data-backed).**

Precise numbers, exact outcomes. Show the work.

- "I did X thing Y times. Z happened."
- "Exact metric in exact timeframe. Here is the system."
- "X businesses did Y. The result: Z."

**Pattern 3: Negative bias (loss aversion).**

Mistakes, failures, things going wrong. Name what the reader loses by not paying attention.

- "The X mistake costing you Y."
- "If you are still doing X, read this."
- "Most X overlook this. They are leaving Y on the table."

**Pattern 4: Story opener (in media res).**

Start mid-action. Dramatic moment. No setup.

- "I stared at X. Unexpected observation."
- "Moment of tension. One sentence of context."
- "Specific scene. Real stakes."

### 3. Apply hard constraints

Each candidate must satisfy every rule below. Drop and replace any candidate that fails.

- No questions as openers.
- No labels. No "Here is how to...", "5 tips for...", "The ultimate guide to...".
- Under 200 characters total. Mobile fits one line.
- No hashtags, no emojis, no formatting symbols.
- No "I am excited to share" or any enthusiasm qualifier.
- Opening sentence under 12 words.
- No banned vocabulary from `/memory/voice/banned-vocabulary.md`. Default banned list if no file: delve, landscape, elevate, unlock, unleash, game-changer, foster, deep dive, disrupt, revolutionize, empower, passionate about, thought leader, cutting-edge, robust, seamless.
- Bar test: would the operator say this to a friend at a bar, unprompted, without sounding like a billboard. If no, drop it.

### 4. Flag platform fit

Add a table at the bottom marking which hooks suit which platforms.

| Platform | Constraint | Best hook types |
|---|---|---|
| LinkedIn | under 200 chars | Contrarian, Specificity, Story |
| X | under 140 chars | Contrarian, Specificity (tighter versions) |
| Blog title | under 70 chars | Contrarian, Specificity |
| Newsletter subject | under 60 chars | Negative bias, Specificity |
| Video title | under 60 chars | Story opener, Specificity |

For each of the ten hooks, list which platforms it fits (one or more).

### 5. Run self-critique

Before presenting, verify:

- [ ] No questions used as openers.
- [ ] No "Here is how..." or numbered-list labels.
- [ ] All under 200 chars (test on mobile).
- [ ] Opening sentence under 12 words.
- [ ] No banned vocabulary detected.
- [ ] Each hook passes the bar test.
- [ ] All four pattern families represented.
- [ ] At least two hooks feel risky, not safe.
- [ ] Voice matches the operator's voice file (or default calm-specific-practical if none).
- [ ] No AI marketing speak, no hype, no overstatement.

Any failures: replace, do not paper over.

### 6. Present and ask the operator to pick

Present the ten hooks in a numbered list, with platform flags. Then ask:

> Which one raises your eyebrows.

Selection guidance to share if asked:

- Options 1 to 3: usually safe, proven structures.
- Options 4 to 8: the sweet spot. Risky and credible.
- Options 9 to 10: boundary-testing. Use if it feels right.

If a hook feels risky, that is a signal to pick it. If it feels safe, that is a signal to discard it.

### 7. Iterate if asked

The operator may ask for tweaks. Refine the winner or generate a new candidate in the same pattern family. Do not regenerate the entire ten unless asked.

### 8. Return final

Output the chosen hook in clean copy-paste form. No preamble, no trailing summary.

## Hard rules

1. Ten candidates. Not eight. Not twelve.
2. Mix across all four pattern families.
3. No emdashes anywhere in the output. Hyphens, commas, full stops only.
4. No emojis, no hashtags, no formatting symbols inside any hook.
5. Operator picks. Do not select on their behalf.
6. The body of the piece is out of scope. Hooks only.

## Edge cases

- **Operator has no banned-vocabulary file.** Use the default banned list in the procedure. Note the absence at the end of the output and suggest creating `/memory/voice/banned-vocabulary.md`.
- **Operator pastes an existing draft and says "rewrite the hook".** Run intake against the draft body, treating the body as the answer to So what and Anchor. Generate ten as normal.
- **Topic is genuinely small.** If the operator's topic does not support ten distinct angles, present fewer (minimum six) and flag the shortage. Do not pad with weak candidates.
- **Multi-platform invocation.** Generate ten general-purpose hooks plus a separate one-line variant per platform (LinkedIn, X, newsletter subject, etc.) of the strongest two. The operator gets options and the platform-fit versions in one pass.

## Acceptance tests

Hand-walked during build:

1. **"Write a hook for a LinkedIn post about a recruitment firm hitting 70% margin with three people."** Returns ten hooks, two to three per pattern family, platform flags weighted to LinkedIn, no banned vocabulary, no questions as openers. Operator picks. PASS.
2. **"Subject line for a newsletter on AI redundancies hitting 30,000 jobs."** Returns ten hooks weighted toward negative bias and specificity, all under 60 chars on the newsletter-subject row. PASS.
3. **"Headline for a blog on why solopreneurs out-compete agencies."** Returns ten with contrarian and specificity weighted heavily, all under 70 chars. PASS.
4. **No emdashes anywhere across all expected outputs.** PASS.
5. **Operator with no banned-vocabulary file.** Default list applied, absence noted at the end, suggestion to create the file. PASS.

## Cross-skill boundaries

| Operator wants | Route to |
|---|---|
| First line for one piece | this skill |
| Full LinkedIn post draft | post-generator skill (e.g. `linkedin-post-template`) |
| Closing line or CTA | `cta-handraiser` skill |
| Cross-platform repurpose of an existing piece | `content-repurposer` skill |
| Weekly content plan | `content-calendar` skill |

## Maintenance

Update this skill when:

- The voice file or banned-vocabulary list changes. The default list in step 3 is the fallback.
- A new platform appears in regular use. Add a row to the platform fit table.
- Operator phrases drift and new triggers should route here.
