---
name: comment-strategy
description: Draft 2 to 3 candidate comments on a specific post, designed to build authority and warm a prospect before any direct outreach. Comment-first networking. 14 to 21 days of thoughtful engagement on a target account before sending a connection request. Comments are weighted roughly twice over likes by most platform algorithms. Use when the operator says "comment strategy", "draft a comment", "comment on this post", "warm prospect via comments", "LinkedIn comment", "reply to this post", or "comment thread strategy". One post per invocation. Returns 2 to 3 comment options across different taxonomies. Operator picks.
triggers:
  - comment strategy
  - draft a comment
  - comment on this post
  - warm prospect via comments
  - LinkedIn comment
  - reply to this post
  - comment thread strategy
context_loads:
  - /memory/voice/voice-rules.md
  - /memory/voice/banned-vocabulary.md
model: precision
version: 1.0
---

# comment-strategy

Strategic commenting is the cheapest warm intro. 14 to 21 days of real engagement on a target account's posts builds credibility and context before the operator ever asks for anything. The operator is not selling. They are thinking out loud, adding value, and proving they understand the prospect's world.

This skill produces 2 to 3 candidate comments on one post, across different comment taxonomies, with reasoning for each angle. The operator picks. The skill does not auto-post.

This skill assumes the operator maintains a banned-vocabulary list and a voice-rules file under `/memory/voice/`. Adapt the paths if your layout differs. The skill works without them (defaults below), but comments drift toward generic if constraints are missing.

## When to use

- The operator wants to comment on a specific post by a target prospect, partner, or peer.
- A warm-up campaign is running and the operator needs the day's comments.
- The operator pastes a post and asks "what should I say".

## When not to use

- Replying to comments on the operator's own posts. That is a separate engagement skill.
- DM-ing the prospect. Use an outreach skill (only after 14 to 21 days of comments).
- Connection requests. The operator handles those manually; this skill produces the comment substrate.
- Bulk auto-commenting. Out of scope. Auto-commenting at scale gets detected and suppressed.

## Inputs

Gather these before drafting:

1. **The post.** Full text of the post the operator is commenting on. Pasted verbatim.
2. **Author.** Name and role or title. Helps tailor relevance.
3. **Goal.** Build relationship, demonstrate expertise, start a conversation, test messaging.
4. **Angle (optional).** What unique perspective does the operator have. Boosts quality when supplied.

If any are missing or the post text is not pasted, ask. Do not draft against a description of a post.

## Procedure

### 1. Confirm intake

Restate inputs back to the operator in one block. If they correct anything, take the correction.

### 2. Pick taxonomies

Pick 2 to 3 of the five comment taxonomies below, chosen to match the operator's goal and the post's content.

**Taxonomy 1: Value-add.**

Teach something the post missed. Add data, framework, contrarian nuance, or missing context.

- Reference a specific claim and extend it.
- "You are right about X, but Y matters more because..."
- Bring a case study or different angle.

**Taxonomy 2: Agreement plus extension.**

Agree completely, then take it one step further in an unexpected direction.

- "This. And what I have noticed is..."
- Add a dimension they did not mention.
- Show the operator has taken the idea further than the author has.

**Taxonomy 3: Contrarian plus respectful.**

Challenge one specific point with evidence. Constructive, not combative.

- "I would push back on X. Here is why..."
- Cite the operator's own experience or data.
- Acknowledge what the author got right first.

**Taxonomy 4: Question.**

Ask a genuine question that makes the author think. Not softball. Not rhetorical.

- "How are you handling X when Y happens."
- Force curiosity, not cleverness.

**Taxonomy 5: Story.**

Brief personal experience that validates or challenges the post. One sentence context, real outcome.

- "I ran into this exact thing..."
- Show work, not wisdom.

### 3. Draft 2 to 3 candidates

One candidate per chosen taxonomy. Each candidate references a specific part of the post (not the whole thing). Each candidate is 2 to 5 sentences.

### 4. Apply hard constraints

Each candidate must satisfy every rule. Drop and replace any candidate that fails.

- Minimum 2 sentences, maximum 5 sentences. Comments under 2 sentences look low-effort. Comments over 5 sentences look like a post.
- References a specific part of the post (shows the operator actually read it).
- No generic affirmations. Specifically: "Great post", "Love this", "100%", "So true". These get suppressed and read as low-effort.
- No self-promotion. No pitch hiding inside the comment.
- Conversational tone. Like talking to a peer, not lecturing.
- No emojis, no hashtags, no @ tags (except author, once, if directly asking them something).
- No banned vocabulary from `/memory/voice/banned-vocabulary.md`. Default banned list if no file: delve, leverage, synergy, thought leader, deep dive, cutting-edge, disrupt, empower, passionate about, in today's world.
- No all-caps, no multiple exclamation marks, no hype language.
- Bar test: would the operator say this to a friend at a bar, unprompted, without sounding like a billboard. If no, drop it.

### 5. Self-critique

Before presenting:

- [ ] Each candidate is something the operator would actually say out loud.
- [ ] Each references a specific part of the post (not the whole thing).
- [ ] No generic affirmations.
- [ ] No hidden pitch or self-promotion.
- [ ] Conversational tone, not instructor tone.
- [ ] 2 to 5 sentences each.
- [ ] No banned vocabulary detected.
- [ ] Would each land the same way on the platform and in a text to a peer.
- [ ] Genuine perspective, not agreement-for-visibility.

### 6. Present and ask operator to pick

Output the 2 to 3 candidates as a numbered list, each tagged with the taxonomy used and one sentence of reasoning (why this angle lands on this post). Then ask:

> Which one fits the relationship best.

### 7. Iterate if asked

The operator may ask for tweaks. Refine the chosen candidate or generate a new candidate in a different taxonomy. Do not regenerate all candidates unless asked.

### 8. Return final

Output the chosen comment in clean copy-paste form. No preamble, no trailing summary.

## Daily engagement routine

This skill is one tool inside a daily routine. Include this block as a footer on the first invocation per day or per session, so the operator does not lose the surrounding context.

**Time budget.** 30 minutes, 10 thoughtful comments per day.

**Breakdown.**

- 5 comments on target accounts (prospects, partners, peers the operator wants relationships with).
- 5 comments on trending posts in the operator's niche (build authority in the feed, discover new people).

**Best timing.**

- Posts under 60 minutes old get roughly 10x more visibility.
- LinkedIn: early morning 06:00 to 09:00 local, lunchtime 12:00 to 13:00, evening 17:00 to 19:00.
- X: whenever the target is active. Check their profile timestamps.

**Target account selection.**

- Decision-makers at firms the operator wants to work with.
- Adjacent operators in the operator's space.
- Potential partners.
- Influencers in the operator's niche who get over 500 comments (signal of high-trust audience).

## Comment-first campaign structure

This is the larger arc the skill plugs into.

**Week 1 to 2: establish presence.**

- 5 comments per day on the target's posts. Mix taxonomies.
- Build a pattern of thoughtful, consistent engagement.
- They start to recognise the operator's name.

**Week 3: optional connection.**

- Send a personalised connection request. Reference a specific comment or post.
- Sample: "I have been following your work on X. Specifically liked your take on Y. Would value connecting."
- Do not send this if the operator has only liked their posts. The 14 to 21 days of comments earn the connection.

**Week 4 and on: earned right to outreach.**

- After connection accepts, wait 3 to 5 days.
- DM or comment with a specific offer, question, or insight. Not "let us connect on a call".
- The context is built. Now the operator can ask.

## Why this works

- Comments are weighted roughly twice over likes on most platform algorithms.
- Engagement pods are dead. Detection rates above 97%. Algorithms punish artificial engagement. Real comments from diverse accounts with unique text beat everything.
- Warmth compounds. By day 14, the target has seen the operator's name 10+ times in a positive context.
- Zero ask, zero rejection. The operator is not pitching, so there is no way to get a "no".
- The operator learns the target's thinking. Reading the comments, understanding what the target cares about, what problems matter to them, what language they use.

## Hard rules

1. 2 to 3 candidates per invocation. Not 1, not 5.
2. Each candidate references a specific part of the post.
3. 2 to 5 sentences per comment. Outside that range fails the constraint.
4. No generic affirmations, ever.
5. No hidden pitch.
6. No emdashes anywhere in the output. Hyphens, commas, full stops only.
7. No emojis, no hashtags.
8. Operator picks. This skill does not auto-post.

## Edge cases

- **Operator pastes a description of a post, not the post.** Push back. Ask for the post text verbatim. Comments drafted against descriptions read fake and miss the actual content.
- **Post is genuinely bad or pitchy.** Push back. Suggest skipping. Commenting on weak content does not build authority.
- **Target's post has 500+ comments already.** Lower visibility, but the comment still registers with the author. Adjust expectation. Continue.
- **Operator wants to challenge the author hard.** Use contrarian taxonomy but acknowledge what the author got right first. Pure contrarian without acknowledgment reads as combative and burns the relationship.
- **No banned-vocabulary file.** Use the default list in step 4. Note absence at the end and suggest creating `/memory/voice/banned-vocabulary.md`.

## Acceptance tests

Hand-walked during build:

1. **"Comment on this post by <target>"** with full post text pasted. Returns 2 to 3 candidates across different taxonomies, each referencing a specific claim in the post, 2 to 5 sentences each, no banned vocabulary, no emojis. PASS.
2. **"Warm prospect via comments. Target: <name>. Post: <text>."** Returns candidates weighted toward value-add and question taxonomies (relationship-building), not contrarian. PASS.
3. **"Reply to this post"** without post text. Pushes back, asks for verbatim post text. PASS.
4. **Generic affirmation slips into a draft.** Replaced before output. PASS.
5. **No emdashes across all expected outputs.** PASS.

## Cross-skill boundaries

| Operator wants | Route to |
|---|---|
| 2 to 3 candidate comments on a specific post | this skill |
| First line or hook for the operator's own post | `hook-writer` skill |
| Closing CTA for the operator's own post | `cta-handraiser` skill |
| DM following 14 to 21 days of comments | outreach skill set |
| Scan for warm leads across the feed | lead-signal-scanner skill |
| Plan the week's comment targets | `content-calendar` skill (under engagement block) |

## Maintenance

Update this skill when:

- The voice file or banned-vocabulary list changes. Default list in step 4 is the fallback.
- Platform suppression rules shift (engagement-bait detection, pod detection). Update the why-this-works block.
- A new comment taxonomy proves itself in production. Add it as Taxonomy 6 with worked example.
- Operator phrases drift and new triggers should route here.
