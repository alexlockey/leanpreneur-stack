---
name: video-to-content
description: Turn a recorded video or podcast interview transcript into a full content suite (blog article, LinkedIn posts, X thread, newsletter section, short-form clip markers, quote cards, YouTube description, guest amplification assets). Use when the operator says "repurpose video", "video transcript", "interview content", "podcast transcript", "turn this interview into posts", or names a recording tool ("Riverside", "Streamyard", "Squadcast"). One transcript per invocation. Returns clip markers with timestamps, derivative posts in the operator's voice with quoted guest lines, and a guest-share pack. Composes with content-repurposer for derivative shaping.
triggers:
  - repurpose video
  - video transcript
  - interview content
  - podcast transcript
  - video repurposing
  - turn this interview into posts
  - repurpose interview
  - Riverside transcript
  - Streamyard transcript
  - Squadcast transcript
context_loads:
  - /memory/voice/banned-vocabulary.md
  - /memory/voice/voice-rules.md
  - /memory/voice/content-pillars.md
model: default
version: 1.0
---

# video-to-content

A 45 to 60 minute interview is the source piece with the highest leverage in any content engine. One conversation produces a blog article, a short-form video reel set, social posts, a newsletter section, and a guest-share pack. The guest extends the operator's reach into their audience; the operator extends the guest's reach into theirs.

This skill takes one transcript and produces the full interview content suite plus clip markers with timestamps. It is the interview-specific specialisation of `content-repurposer`. Use this when the source is a recorded conversation. Use `content-repurposer` for everything else.

This skill assumes the operator maintains a banned-vocabulary list, a voice-rules file, and a content-pillars file under `/memory/voice/`. Adapt the paths if your layout differs. The skill works without them, but the derivatives drift toward generic copy and become harder to attribute to the operator's voice.

## When to use

- The operator has a transcript of a recorded interview, podcast, or long-form conversation.
- The transcript carries timestamps (preferred) or the operator can supply them on request.
- The operator wants the conversation to live across blog, social, short-form video, and a guest-share pack.

## When not to use

- The source is the operator's own monologue, not an interview. Use `content-repurposer`.
- The source is a blog or written piece. Use `content-repurposer`.
- The operator wants only the clip markers, not the derivatives. Stop after step 4.
- The operator wants to plan a future interview, not repurpose one that ran. Use `content-calendar`.

## Inputs

Gather these before generating:

1. **Full transcript.** Paste, file path, or URL. Timestamped if available.
2. **Guest name and business.** Name plus one-line role. ("Sarah Chen, founder of TinyOps.")
3. **Guest's core insight or story.** The one thing the guest is known for, or the dominant theme of this conversation.
4. **Content pillar.** One of the operator's pillars from `/memory/voice/content-pillars.md`.
5. **Audience.** Who reads the operator's content. Operator, founder, ops lead, specific role.
6. **CTA target.** Where the reader should go: community, newsletter, full video, blog, or nowhere.
7. **Restrictions.** Guest-side sensitivities (a topic they asked to be cut, a competitor not to name, a number to round). Operator-side guard rails.

If any of these are missing, ask. Do not generate derivatives against blank fields.

## Procedure

### 1. Confirm intake

Restate the seven inputs back to the operator in one block. If they correct anything, take the correction.

### 2. Transcript analysis

Read the full transcript. Extract:

- **Five strongest moments.** Quotable, surprising, emotional, or insightful. Mark each with a timestamp (or a clear quoted phrase if no timestamps).
- **Core story arc.** Before state -> transformation or insight -> after state.
- **Three to five standalone insights.** Each one portable: it works without context, attributed to the guest.
- **Metrics and numbers** mentioned. Revenue, headcount, time saved, efficiency, conversion. Cite exact figures.
- **Clip markers.** Three to five segments, each 30 to 60 seconds, that work as short-form video. For each, note: in-point, out-point, why this clips well (emotional hook, insight, pattern-break, story moment).

Hold the analysis as the spine. Every derivative references it without copying transcript text.

### 3. Draft blog article

1000 to 2000 words.

- **Title.** SEO-aware, insight-style or news-style. Not "Interview with Sarah Chen". An assertion or specific outcome from the conversation.
- **Intro hook.** One specific moment, one strong line, or one number that earns the next paragraph.
- **Guest's story arc.** Before -> transformation -> after, in the operator's voice with the guest's actual words in quotes.
- **Three key insights** with at least one direct quote per insight.
- **"Why this matters" section.** The principle that generalises beyond the guest's specific case.
- **CTA.** Matches the operator's CTA target from intake.

Voice: the operator's voice carries the prose; the guest's voice lives inside the quote marks. Acknowledge trade-offs.

Do not produce: Q-and-A dump, transcript-style verbatim, generic summary, "in this interview we discussed".

### 4. Draft LinkedIn posts

Three or four posts, each between 100 and 250 words. Each post around one insight, using one framework per post (story, hot take, case study, framework).

For each post:

- Frame the insight through the operator's lens, then attribute the guest in one line near the close.
- Use the guest's actual words in quotes for at most one line per post.
- Close with a subtle CTA matching the operator's CTA target.

Then, for each LinkedIn post, draft a **guest comment** the guest can leave on their own share of the operator's post. The comment is 50 to 75 words, adds context or builds on the post, written in the guest's likely voice (calibrated from the transcript).

### 5. Draft X thread

Hook tweet plus five to eight body tweets.

- **Tweet 1.** Compresses the guest's story into one hookable line. Under 240 chars.
- **Tweets 2 to N minus 1.** One insight per tweet. Each under 280 chars. Each works alone.
- **Tweet N.** CTA, link to the full video.

Voice: tighter than the blog. Compress the insight. Honour the guest's specifics.

### 6. Draft newsletter section

200 to 300 words. The single insight from the interview most useful to the newsletter audience.

Tone: personal, candid, like writing to a friend. Attribute the guest. Frame through the operator's lens. One CTA matching the CTA target.

### 7. Output short-form video scripts

For each clip marker from step 2, produce:

- **Suggested title** (under 12 words).
- **Text overlay** for the clip (one line, under 10 words).
- **In and out timestamps** lifted from the analysis.
- **Why this clips well** (one line, the reason for selection).

Hand the markers to whatever editing flow the operator uses (Riverside clip export, Descript, manual editing in Premiere or DaVinci).

### 8. Draft quote cards

Three to five cards. Each card:

- Guest's best one or two sentence line.
- Properly attributed: guest name and role, no titles.
- Punchier than the source. Strip hedging if it does not change the meaning.

### 9. Draft YouTube description

- **Title.** Guest name plus core insight plus the operator's interview-series name. ("Sarah Chen on the three numbers behind lean margin, on the Operator Show.")
- **Description.** 150 word summary, then timestamps for key moments lifted from step 2, then tags, then a link to the blog article and the operator's primary CTA target.

### 10. Build the guest amplification pack

For each LinkedIn post (from step 4), assemble:

- The post itself.
- The pre-drafted guest comment for them to use.
- A casual DM the operator can send to the guest pointing them at the post ("here is your version, comment ready, share if it feels right").

Tone: low-friction. The guest should be able to share with one click and a paste.

### 11. Suggest a staggered distribution calendar

Default layout, fourteen days:

| Day | Asset |
|---|---|
| 1 | Full video live (operator's platform plus YouTube) and blog article published |
| 3 | LinkedIn post 1 (story framework) |
| 4 | LinkedIn post 2 (hot take or framework) |
| 5 | LinkedIn post 3 (case study) |
| 7 | X thread |
| 10 | Newsletter section |
| Week 2 | Short-form clips, one per day, across the operator's chosen short-form platforms |

Adjust if the operator's calendar already has commitments. Hand to `content-calendar` for slotting if used.

### 12. Run self-critique

Before delivering, verify:

- [ ] Blog article tells a story; it is not a transcript dump.
- [ ] All quotes are accurate to the transcript (operator can verify against timestamps).
- [ ] LinkedIn posts each work alone.
- [ ] X thread is mobile-readable (short lines, line breaks).
- [ ] Clip markers have clear in and out points and a stated reason for selection.
- [ ] Guest amplification pack is low-friction (post plus comment plus DM, ready to send).
- [ ] Every derivative links back to the full video or the operator's CTA target.
- [ ] Newsletter section is one section, not a full issue.
- [ ] No hashtags, no emojis, no engagement bait.
- [ ] No banned vocabulary.
- [ ] No emdashes.
- [ ] Restrictions from intake (cut topic, competitor name, rounded number) are honoured everywhere.

Any failures: replace, do not paper over.

### 13. Present the content suite

Deliver in one document, labelled sections for each derivative, then the clip markers, then the guest amplification pack, then the distribution calendar, then the self-critique checklist with pass marks. Then ask:

> Want me to hand the clip markers straight to your editor and the guest pack straight to your DM drafts.

### 14. Iterate if asked

Refine the affected derivative only. Do not regenerate the whole suite unless the operator asks.

## Hard rules

1. The operator's voice carries the prose; the guest's voice lives inside the quote marks.
2. All quotes traceable to the transcript by timestamp or by exact phrase.
3. Restrictions from intake are absolute, not advisory.
4. No hashtags, no emojis, no engagement bait.
5. No emdashes.
6. CTA target is set once at intake and applies across every derivative.

## Edge cases

- **Transcript has no timestamps.** Ask the operator to supply timestamps for the five strongest moments, or to point at the video so the clip markers can use scene descriptions instead.
- **Guest spoke for under twenty percent of the recording.** Flag this. The interview is closer to a monologue and `content-repurposer` is the better skill.
- **Guest asked for an edit after recording.** Honour it absolutely. Strip the topic from every derivative. Do not surface it in clips or in the YouTube description.
- **Operator has no interview-series name.** Use a placeholder like "interview" in step 9 and flag the gap. Suggest creating a series name once the operator has three interviews shipped.
- **Operator does not have a content-pillars file.** Ask for the pillar at intake. Suggest creating `/memory/voice/content-pillars.md`.
- **Operator wants to skip the guest amplification pack.** Honour the restriction. Output everything else.

## Acceptance tests

Hand-walked during build:

1. **"Repurpose this 55-minute Riverside transcript with founder Sarah Chen. Full suite, CTA to newsletter."** Returns blog article around 1500 words, three LinkedIn posts each under 250 words with a guest comment per post, X thread of seven tweets, newsletter section around 250 words, four clip markers with timestamps and reasons, four quote cards, YouTube description with timestamps. Guest amplification pack ready. PASS.
2. **"Just clip markers from this transcript, nothing else."** Returns five clip markers with in and out points and selection rationale. Stops there. PASS.
3. **"Repurpose this podcast but the guest asked us to cut the part where she talked about the funding round."** Honours the restriction. The funding round does not appear in any derivative, clip marker, or YouTube description. PASS.
4. **No emdashes anywhere across all expected outputs.** PASS.
5. **Transcript with no timestamps.** Asks the operator for timestamps on the five strongest moments before producing clip markers. PASS.

## Cross-skill boundaries

| Operator wants | Route to |
|---|---|
| Interview transcript repurpose with clip markers | this skill |
| Non-interview repurpose (blog, daily write, dictated note) | `content-repurposer` |
| First line for the blog or for one LinkedIn post | `hook-writer` |
| Slide HTML for a carousel pulled from the interview | `carousel-builder` (pass the outline section) |
| Closing line on any one derivative | `cta-handraiser` |
| Slot the distribution calendar | `content-calendar` |

## Maintenance

Update this skill when:

- A new recording or editing platform enters the operator's stack and the workflow changes (e.g. clip export shape, transcript shape). The intake and step 7 should track the platform's output.
- The operator's interview series adopts a series name. Replace the placeholder in step 9.
- Guest amplification mechanics change (e.g. LinkedIn removes pre-drafted comments). Update step 10 and the pack format.
- The voice file, banned-vocabulary list, or content-pillars list changes. Step 12 self-critique points at those files.
