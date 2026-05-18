---
name: content-repurposer
description: Take one source piece (blog post, video transcript, daily write, dictated note, case study) and generate platform-specific derivatives (LinkedIn variants, X thread, blog article, newsletter section, carousel outline, quote cards, short-form video script). Use when the operator says "repurpose this", "turn this into posts", "atomise this", "derivatives", or "multi-platform". Returns a content suite with a suggested staggered distribution calendar. Composes with hook-writer (for each derivative's opener) and content-calendar (for distribution slots).
triggers:
  - repurpose this
  - repurpose
  - turn this into posts
  - derivatives
  - atomize this
  - atomise this
  - multi-platform derivatives
  - content suite
context_loads:
  - /memory/voice/banned-vocabulary.md
  - /memory/voice/voice-rules.md
  - /memory/voice/content-pillars.md
model: default
version: 1.0
---

# content-repurposer

One source piece becomes seven or more platform-native derivatives. The repurposer is the lever between writing once and distributing for two weeks.

This skill takes one source piece and produces a content suite: three to four LinkedIn variants, one X thread, one blog article, one newsletter section, one carousel outline, three to five quote cards, one short-form video script, and a staggered distribution calendar. Each derivative stands alone, matches its platform, and refers back to the source thesis without copying it.

This skill assumes the operator maintains a banned-vocabulary list, a voice-rules file, and a content-pillars file under `/memory/voice/`. Adapt the paths if your layout differs. The skill works without them, but the derivatives drift toward generic copy and become harder to attribute to the operator's voice.

## When to use

- The operator has one source piece (blog, transcript, daily write, dictated note, case study) and wants reach across multiple platforms.
- The operator has a content engine running and needs the source piece pre-atomised before scheduling.
- The operator is preparing for a launch and wants a fortnight of inventory off one anchor.

## When not to use

- Generating one piece of content from a topic, with no source piece yet. Use `hook-writer` plus a draft skill.
- A video interview transcript specifically. Use `video-to-content` instead. It handles guest amplification, clip markers, and YouTube description.
- A weekly content plan from scratch. Use `content-calendar`.
- Closing line on any single derivative. Use `cta-handraiser`.

## Inputs

Gather these before generating:

1. **Source content.** Paste directly, file path, or URL. One source per invocation.
2. **Source type.** Blog post, video transcript, daily write, dictated notes, case study.
3. **Target platforms.** Default all (LinkedIn, X, blog, newsletter, carousel outline, quote cards, short-form video). Subset allowed.
4. **Content pillar.** One of the operator's named pillars from `/memory/voice/content-pillars.md`. If no file, ask the operator to name a pillar for this piece.
5. **Audience.** Who the primary piece is for. Operator, founder, ops lead, specific role.
6. **CTA target.** Where the reader should go next: community signup, newsletter, blog, DM, calendar link, or nowhere. Pick one.
7. **Restrictions.** Don't link to X. Avoid naming competitor X. Skip the bakery story. Operator-specific.

If any of these are missing, ask. Do not generate derivatives against blank fields.

## Procedure

### 1. Confirm intake

Restate the seven inputs back to the operator in one block. If they correct anything, take the correction.

### 2. Run extraction

Read the source. Extract and write down:

- **Core thesis.** One sentence. The idea the operator would explain in a text message.
- **Three to five key insights.** Distinct, standalone. Each one can travel without the others.
- **Strongest data point or story.** The most specific, most memorable moment in the source.
- **Most quotable line.** Punchy. Reusable across platforms without context.
- **Framework or structure.** If any (three pillars, if-then pattern, before-after).
- **Implicit trade-off.** What does the operator give up by holding this view. If no trade-off, the thesis is probably not as sharp as it could be; flag this to the operator.

Hold the extraction block as the spine. Every derivative references it without copying source text.

### 3. Generate LinkedIn variants

Generate three or four variants, one per framework from the list below. Each variant under 250 words, mobile-formatted (short paragraphs, white space, no walls of text).

**Framework 1, Story.** Open in a specific moment. Middle: what changed or what was learned. Close: the principle that generalises. Conversational. No external links in the body.

**Framework 2, Framework or structure.** Headline frames the idea. Three to five short paragraphs or numbered points, each self-contained. Close with a question or invitation.

**Framework 3, Hot take.** Open with a contrarian observation (genuine, not clickbait). Middle: why most people get this wrong. Close: implication for builders.

**Framework 4, Case study or behind-the-scenes.** Open with "Here is what happened" specifics. Middle: numbers or outcomes. Close: one principle that generalises.

Apply these rules to every variant:

- Unique hook per variant. Never reuse the opener.
- No hashtags, no emojis, no engagement bait ("thoughts?", "agree or disagree?", "drop your take").
- No banned vocabulary.
- Bar test on every line.
- CTA optional and subtle, matching the operator's CTA target from intake.

### 4. Generate X thread

Eight to ten tweets total.

- **Tweet 1, Hook.** Under 240 chars. One assertion or pattern interrupt that earns the click into the thread.
- **Tweets 2 to N minus 1, Body.** Each tweet standalone (works if read in isolation). Short, punchy, one idea each.
- **Tweet N, CTA.** Link to the full piece, or to the operator's CTA target.

Rules:

- Each tweet works alone.
- No hashtags.
- No links in body tweets. The link lives only in the final CTA tweet.
- Use line breaks and indentation for readability.
- Voice: punchier than LinkedIn, more opinionated, compress the insight.

### 5. Generate blog article

800 to 2000 words.

- **Headline.** SEO-aware, answer-forward. Not "Interview with..." or "Thoughts on...".
- **Deck.** Subheading, under 40 words, sets context.
- **Body.** Two to four main sections with h2s and h3s. Subheadings are questions or assertions, not generic labels.
- **One section** carries data, the case study, or the example.
- **Closing.** Implications and next step. Not "wrap-up". Not "in conclusion".

Voice: longer form, more breathing room. Show the thinking. Acknowledge trade-offs. SEO: include two or three long-tail keywords naturally, link to related pieces on the operator's site.

Do not include: fluff paragraphs, "let us explore", generic transitions, "in this post we will".

### 6. Generate newsletter section

150 to 300 words. One section, not a full newsletter issue.

Tone: personal, conversational, like writing to a friend who runs a business. No links in the body except a single CTA link if applicable.

Structure: hook (one observation or lesson) -> middle (context, what changed, what was noticed) -> close (one CTA or invitation matching the operator's CTA target).

### 7. Generate carousel outline

Cover slide plus six to eight content slides plus CTA slide.

For each slide, write one headline (under 15 words) and one body line (under 25 words). Hand the outline to `carousel-builder` if the operator wants slide HTML.

Structure:

- Slide 1: hook from the source thesis.
- Slides 2 to N minus 1: one insight per slide.
- Slide N: CTA matching the operator's CTA target.

### 8. Generate quote cards

Three to five quotes, under 15 words each.

- Pull the most quotable lines from the source.
- Strip context. Each card stands alone.
- Punchier than the source. Remove hedging.

Format each card as: quote text, then attribution line. Attribution is the operator's name and handle, no titles.

### 9. Generate short-form video script

60 seconds.

- **Hook, 0 to 3 sec.** One question or assertion that stops the scroll.
- **Body, 3 to 48 sec.** The main insight, conversational, like talking to a friend.
- **CTA, 48 to 60 sec.** What to do next, matching the operator's CTA target.

Write as a script, not a storyboard. Include pauses and emphasis. No graphics notes, no cut directions, no jargon.

### 10. Suggest a staggered distribution calendar

Spread the derivatives over fourteen days. Default layout:

| Day | Derivative |
|---|---|
| 1 | LinkedIn variant 1 |
| 2 to 3 | X thread |
| 4 | Blog article live |
| 5 | LinkedIn variant 2 |
| 6 | Newsletter section |
| 7 to 8 | Carousel |
| 9 to 11 | Quote cards (one per day if a social suite is connected) |
| 10 | LinkedIn variant 3 if the third hook is strong |
| 12 | Short-form video |
| 14 | Recap post ("here is what landed, here is what I learned") |

Adjust if the operator's calendar already has commitments. Pass the calendar to `content-calendar` for slotting if the operator runs one.

### 11. Run self-critique

Before delivering, verify:

- [ ] Core thesis is one sentence, not vague.
- [ ] Three to five extracted insights are distinct, not repeating.
- [ ] LinkedIn variants each have a unique hook.
- [ ] X thread tweets each work alone.
- [ ] Blog has a deck, h2s, h3s, and a non-"in conclusion" closer.
- [ ] Newsletter section is one section, not a full issue.
- [ ] Carousel outline is one idea per slide.
- [ ] Quote cards strip context and stand alone.
- [ ] Video script has a hook in the first three seconds.
- [ ] No hashtags, no emojis, no engagement bait anywhere.
- [ ] No banned vocabulary.
- [ ] No emdashes.
- [ ] Distribution calendar staggers across fourteen days.

Any failures: replace, do not paper over.

### 12. Present the suite

Deliver in one document with a labelled section for each derivative, ready to copy-paste. End with the distribution calendar and the self-critique checklist with pass marks. Then ask:

> Want me to send the carousel outline straight to carousel-builder, or hold it.

### 13. Iterate if asked

The operator may ask for a different framework on one LinkedIn variant, a tighter hook on the X thread, or a different CTA. Refine the affected derivative only. Do not regenerate the whole suite unless asked.

## Hard rules

1. Seven or more derivatives off one source piece. Subset allowed if the operator restricts platforms at intake.
2. Each derivative has a unique platform-native hook. No copy-paste openers.
3. No hashtags, no emojis, no engagement bait anywhere.
4. No emdashes.
5. CTA target is set once at intake and applies across every derivative.
6. The operator picks slot dates and confirms the calendar before scheduling.

## Edge cases

- **Source piece is itself a derivative (e.g. an X thread).** Refuse. Atomise from the original blog or transcript instead. Note the rule and ask for the source.
- **Source piece is over 5000 words.** Run extraction first, present the spine to the operator, then ask which two or three insights to lift. Do not derive every platform off the whole source; output will be bloated.
- **Operator has no content-pillars file.** Ask for the pillar at intake. If the operator does not have pillars, the derivatives will lack thematic cohesion across weeks. Suggest creating `/memory/voice/content-pillars.md`.
- **Operator says "no LinkedIn variants, just the blog and the newsletter".** Honour the restriction. Skip the others. Adjust the calendar accordingly.
- **CTA target is "nowhere".** Drop the CTA from every derivative. Replace the slot with a soft attribution line.

## Acceptance tests

Hand-walked during build:

1. **"Repurpose this 1200-word blog on lean margin into a full suite."** Returns three LinkedIn variants (story, framework, hot take), one X thread of eight tweets, blog version not regenerated (since source is the blog), newsletter section under 300 words, carousel outline of seven slides, four quote cards, one video script. Distribution calendar fits fourteen days. PASS.
2. **"Turn this dictated-note transcript into derivatives. Skip the blog, skip the video script."** Returns three LinkedIn variants, X thread, newsletter section, carousel outline, quote cards. Distribution calendar adjusts. PASS.
3. **"Atomise this case study but no LinkedIn variants, just the blog and the newsletter."** Returns blog plus newsletter only. Calendar reflects two-piece spread, not fourteen days. PASS.
4. **No emdashes anywhere across all expected outputs.** PASS.
5. **Operator with no content-pillars file.** Asks for pillar at intake, suggests creating the file, proceeds when answered. PASS.

## Cross-skill boundaries

| Operator wants | Route to |
|---|---|
| Atomise one source piece into many | this skill |
| First line on any one derivative | `hook-writer` |
| Closing line on any one derivative | `cta-handraiser` |
| Slide HTML for the carousel outline | `carousel-builder` (pass the outline) |
| Video interview repurpose with guest amplification | `video-to-content` |
| Slot the distribution calendar into the operator's plan | `content-calendar` |

## Maintenance

Update this skill when:

- A new platform enters the operator's regular use. Add a derivative section and a row to the distribution calendar.
- The voice file, banned-vocabulary list, or content-pillars list changes. The extraction and self-critique steps point at those files.
- LinkedIn or X changes their length or formatting constraints. Update the per-derivative limits.
