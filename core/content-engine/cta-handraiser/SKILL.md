---
name: cta-handraiser
description: Write a single earned call to action for a post. Four types are supported: question CTA (engagement), resource CTA (lead capture via keyword), action CTA (link click), handraiser CTA (self-identifying prospect). Use when the operator says "write a CTA", "call to action", "conversion post", "lead magnet", "handraiser", "draft a CTA", or "write a closer". One CTA per invocation. Returns one CTA matched to the post's value and the operator's offer ladder. Does not write the body of the post.
triggers:
  - write a CTA
  - call to action
  - conversion post
  - lead magnet
  - handraiser
  - draft a CTA
  - write a closer
  - how should I end this post
context_loads:
  - /memory/voice/voice-rules.md
  - /memory/offers/offer-ladder.md
version: 1.0
---

# cta-handraiser

Produce one earned CTA per post. Match the CTA type to the value the post delivers, the desired reader action, and the operator's offer ladder. The CTA must feel like the natural next step in a conversation that the post already started.

This skill assumes the operator maintains an offer-ladder file under `/memory/offers/offer-ladder.md` listing three to five named offers (e.g. newsletter, community, service). Adapt the path if your layout differs. The skill works without it (defaults below), but the CTAs drift toward generic if offers are absent.

## When to use

- A post draft is ready and needs a closing line.
- The operator wants to convert a value post into a lead-capture post.
- A handraiser keyword needs to be wired into a post for inbound qualification.
- The operator says "how should I end this post".

## When not to use

- Writing the body of the post. Use a post-generator skill.
- Writing the hook or opener. Use the `hook-writer` skill.
- Drafting outreach DMs after a handraiser fires. Use an outreach skill.
- Multi-CTA posts. This skill produces one CTA. If the operator wants multiple, push back: posts with more than one CTA convert worse.

## Inputs

Gather these before writing:

1. **Post topic.** What is the idea or story.
2. **Value delivered.** What did the reader learn, feel, or realise.
3. **Desired action.** Comment, DM, click, reply, sign up, book.
4. **Offer.** Which offer from the operator's ladder this CTA points to.
5. **Keyword.** If resource or handraiser CTA, the all-caps keyword the reader comments to trigger it.

If any are missing, ask. Do not write CTAs against blank fields.

## Procedure

### 1. Confirm intake

Restate the five inputs back to the operator in one block. If they correct anything, take the correction.

### 2. Pick the CTA type

Match the CTA type to the post and the desired action.

**Type 1: Question CTA.**

- Best for: reach, engagement, starting conversations.
- How it works: genuine question that invites opinion. Avoids bait.
- Why: comments are weighted roughly twice over likes on most algorithms.

Example shape:

> How does your team balance autonomy with accountability. Genuinely curious what is working.

**Type 2: Resource CTA.**

- Best for: building qualified lead lists, DM-ing warm prospects.
- How it works: offer a specific resource (framework, template, guide) in exchange for a comment keyword.
- Pattern: "I put together X. Comment KEYWORD and I will DM it."

Example shape:

> I built a one-page diagnostic for <topic>. Comment DIAGNOSTIC and I will send it over.

**Type 3: Action CTA.**

- Best for: driving clicks (newsletter, landing page, booking).
- How it works: link in the first comment, not in the post body. Body links dilute engagement signals on most platforms.
- Pattern: "(First comment) Full breakdown plus link here."

Example shape:

> (First comment) Full breakdown and the form to register: <url>

**Type 4: Handraiser CTA.**

- Best for: self-identification. Reader comments a keyword and becomes a warm prospect.
- How it works: same mechanic as resource CTA, but outcome is lead capture, not just content delivery.
- Pattern: "Working on X. Comment YES and let us talk."

Example shape:

> If you are running <specific situation>, comment <KEYWORD> and I will share the details.

### 3. Apply the earned-ask rule

The CTA must be earned by the value in the post. Never tack on an unrelated CTA.

- Post taught a framework: CTA offers the template.
- Post told a founding story: CTA offers the lessons document.
- Post challenged conventional wisdom: CTA invites a conversation.
- Post proved a thesis with data: CTA offers the deeper analysis.

If the CTA does not earn its ask, replace it.

### 4. Match to the offer ladder

Read the operator's offer ladder from `/memory/offers/offer-ladder.md`. Default ladder if no file:

- Top of funnel: newsletter sign-up. No friction.
- Mid funnel: community or paid resource. Recurring or one-time low commitment.
- Bottom of funnel: service conversation. Sales call.

Pick the rung that matches the post's energy and audience. A heavy-thinking post on a frontier topic ladders to mid or bottom funnel. A light story post ladders to top of funnel.

### 5. Apply hard constraints

- One CTA per post. Period.
- No engagement-bait triggers. Specifically: "Like for Part 2", "Tag someone who...", "Comment YES if you agree", "Double tap", emoji-triggered engagement. These get suppressed by most platforms.
- CTA length: 1 to 3 lines maximum.
- Keyword in ALL CAPS if using resource or handraiser CTA.
- No fluff. Every word earns its place.
- No "please" or over-softening. Strong ask, strong offer.
- No emojis, no hashtags, no formatting symbols.
- Voice matches the operator's voice file. Default: calm, credible, direct.

### 6. Self-critique

Before returning:

- [ ] CTA matches the post's value.
- [ ] One of the four types selected explicitly.
- [ ] No engagement-bait patterns.
- [ ] 1 to 3 lines max.
- [ ] One keyword (if applicable) in ALL CAPS.
- [ ] CTA points to one rung of the offer ladder.
- [ ] Reader would know exactly what to do.
- [ ] Voice aligned with the post.
- [ ] No "please", no fluff, no double softening.

Any failures: rewrite, do not paper over.

### 7. Return

Output one CTA in copy-paste form. State which type was chosen (Question, Resource, Action, Handraiser) and which rung of the offer ladder it points to. No preamble, no trailing summary.

If the operator's next message is "draft the DM follow-up", route to an outreach skill.

## Hard rules

1. One CTA per post. Push back on requests for multiple.
2. CTA must be earned by post value. Drop any CTA that fails the earned-ask rule.
3. No engagement-bait. Suppression risk outweighs short-term comment lift.
4. No emdashes anywhere in the output. Hyphens, commas, full stops only.
5. No emojis, no hashtags, no formatting symbols.
6. 1 to 3 lines maximum.
7. Operator picks the offer. This skill matches CTA mechanics to the operator's pick, not the other way around.

## Edge cases

- **Operator has no offer-ladder file.** Use the default ladder in the procedure. Note the absence at the end and suggest creating `/memory/offers/offer-ladder.md`.
- **Post value is genuinely thin.** Push back. A CTA cannot earn its ask if the post does not deliver value. Suggest reworking the post before adding a CTA.
- **Operator asks for two CTAs in one post.** Push back. Offer one strong CTA plus a follow-up post that opens with the second CTA.
- **Operator wants the link in the post body, not the first comment.** Note the engagement-signal cost on most platforms. Let the operator decide. Default behaviour: link in first comment.
- **Keyword collides with a common word ("YES", "OK", "HELP").** Pick a more specific keyword. Common words attract noise comments.

## Acceptance tests

Hand-walked during build:

1. **"Write a CTA for a post that taught a margin framework. Desired action: comment for template. Offer: paid community."** Returns one resource-type CTA with keyword in ALL CAPS, 1 to 3 lines, no bait. Points to mid-funnel community. PASS.
2. **"Closing line for a contrarian post on AI agencies. Desired action: start conversation."** Returns one question-type CTA, no keyword, opens a thread. PASS.
3. **"Handraiser for an audit-first post. Desired action: book a call. Offer: service conversation."** Returns one handraiser CTA, keyword in ALL CAPS, points to bottom-funnel service. PASS.
4. **No offer file, no operator-provided ladder.** Returns CTA against default ladder, notes the absence at the end. PASS.
5. **No emdashes anywhere in expected outputs.** PASS.

## Cross-skill boundaries

| Operator wants | Route to |
|---|---|
| One earned CTA for a finished post | this skill |
| First line or scroll-stopping hook | `hook-writer` skill |
| Full LinkedIn post draft | post-generator skill |
| Outreach DM following a handraiser | outreach skill set |
| Multi-CTA campaign across posts | `content-calendar` skill (sequence them) |

## Maintenance

Update this skill when:

- The operator's offer ladder changes. The default ladder in step 4 is the fallback.
- Platform engagement-bait suppression patterns change. Update the constraint list.
- A new CTA type proves itself in production. Add it as Type 5 with worked example.
- Operator phrases drift and new triggers should route here.
