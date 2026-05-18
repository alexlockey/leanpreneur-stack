---
name: carousel-builder
description: Generate a LinkedIn carousel post (cover slide, content slides, CTA slide) with publication-ready copy and optional HTML/CSS for each slide. Use when the operator says "create a carousel", "build a carousel", "multi-slide post", "carousel for LinkedIn", or "swipeable post". One topic per invocation. Returns slide-by-slide copy, an optional HTML/CSS file per slide, a companion lead-in post, and a self-critique pass. Composes with hook-writer for slide 1 if the cover hook is weak. Does not draft the body of a separate text post.
triggers:
  - create a carousel
  - build a carousel
  - build a carousel post
  - carousel for LinkedIn
  - multi-slide post
  - carousel slides
  - PDF carousel
  - swipeable post
context_loads:
  - /memory/voice/banned-vocabulary.md
  - /memory/voice/voice-rules.md
  - /memory/brand/visual-tokens.md
model: default
version: 1.0
---

# carousel-builder

A LinkedIn carousel is one or more vertical 4:5 images, swiped left-to-right. It earns higher engagement than a single text post because each swipe is a small commitment that compounds dwell time. This skill produces the slide-by-slide copy and optional HTML/CSS for one carousel on one topic.

This skill assumes the operator maintains a banned-vocabulary list and a voice-rules file under `/memory/voice/`, and optional brand visual tokens at `/memory/brand/visual-tokens.md`. Adapt the paths if your layout differs. The skill works without them, but the output drifts toward generic colours and generic copy if the constraints are missing.

## When to use

- The operator has a topic and wants a carousel instead of, or alongside, a text post.
- The topic has a clear structure (a framework, a list, a process, a before-after).
- The operator wants slide HTML/CSS to hand to a designer, or to render directly.

## When not to use

- Single-image post. Use a graphic-design skill instead.
- Long-form blog or newsletter article. Use `content-repurposer` and then a writing skill.
- Picking the hook. Use `hook-writer` for slide 1 if the cover is weak, then return here.
- Closing line on a separate text post. Use `cta-handraiser`.

## Inputs

Gather these before generating:

1. **Topic.** What is the carousel about. One sentence.
2. **Audience.** Who swipes. Operator, founder, ops lead, specific role.
3. **Source material.** Blog post, framework, case study, numbers, customer story. Paste or path.
4. **Slide count.** Default seven (cover plus five content plus CTA). Max ten.
5. **CTA shape.** Choose one: soft follow, comment prompt, DM invite, save-for-later, or link in comments.
6. **Output mode.** Copy only, or copy plus HTML/CSS files.

If any of these are missing, ask. Do not generate slides against blank fields.

## Procedure

### 1. Confirm intake

Restate the six inputs back to the operator in one block. If they correct anything, take the correction.

### 2. Lock the slide structure

Default seven slides, adjust by operator preference. Always:

- **Slide 1, Cover.** Headline hook, under 10 words, sized large. Operator name or handle at the bottom. This is the thumbnail; it decides whether the swipe happens. If the cover hook feels weak, route to `hook-writer` first, then return here with the chosen hook.
- **Slides 2 to N minus 1, Content.** One idea per slide. Headline under 15 words plus one or two supporting sentences, under 25 words total. Swipe indicator bottom right.
- **Slide N, CTA.** Single, clear next step. Operator name or handle. Optional one-line positioning statement.

### 3. Draft slide copy

Write each slide as a single block of three fields: headline, body, swipe-indicator (or CTA on the last slide). One idea per slide. No exceptions. If a slide carries two ideas, split it.

### 4. Apply hard constraints

Each slide must satisfy every rule below. Drop and replace any slide that fails.

- Slide 1 headline under 10 words.
- Content slide headlines under 15 words.
- Content slide body under 25 words total.
- No questions as openers on slide 1 (a question as the CTA on the final slide is allowed).
- No hashtags, no emojis, no formatting symbols inside any slide copy.
- No banned vocabulary from `/memory/voice/banned-vocabulary.md`. Default banned list if no file: delve, landscape, elevate, unlock, unleash, game-changer, foster, deep dive, disrupt, revolutionize, empower, passionate about, thought leader, cutting-edge, robust, seamless.
- No emdashes anywhere. Hyphens, commas, full stops only.
- Bar test: would the operator say this to a friend at a bar. If no, drop it.

### 5. Generate optional HTML/CSS per slide

If the operator picked output mode copy-plus-HTML, generate one self-contained HTML file per slide. Use the operator's brand visual tokens from `/memory/brand/visual-tokens.md` if present. Otherwise apply the default tokens below.

Default tokens (used only when the operator has no brand tokens file):

- Canvas: 1080 wide by 1350 tall, 4:5 ratio.
- Padding: 60 each side.
- Background: solid colour or a single 135-degree linear gradient.
- Primary font: Inter (Google Fonts).
- Headline: 48 to 72 size, weight 700.
- Body: 24 to 32 size, weight 400.
- Caption: 18 to 20 size, weight 400.
- Headline line height 1.3, body line height 1.5.
- Contrast ratio minimum WCAG AA, 4.5:1 for body text.
- One subtle watermark, bottom right, the operator's domain or handle, 18 size, muted.

Default HTML template (substitute headline, body, and watermark text):

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Carousel slide [N]</title>
  <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;600;700&display=swap" rel="stylesheet">
  <style>
    * { margin: 0; padding: 0; box-sizing: border-box; }
    body { font-family: 'Inter', sans-serif; }
    .slide {
      width: 1080px; height: 1350px;
      background: linear-gradient(135deg, #1a1a2e 0%, #2e3a5e 100%);
      color: #ffffff;
      display: flex; flex-direction: column; justify-content: space-between; padding: 60px;
      position: relative;
    }
    .headline { font-size: 64px; font-weight: 700; line-height: 1.3; margin-bottom: 24px; }
    .body { font-size: 28px; font-weight: 400; line-height: 1.5; }
    .swipe { font-size: 18px; text-align: right; margin-top: 16px; }
    .watermark { font-size: 18px; position: absolute; bottom: 60px; right: 60px; opacity: 0.6; }
  </style>
</head>
<body>
  <div class="slide">
    <div>
      <h1 class="headline">[HEADLINE]</h1>
      <p class="body">[BODY]</p>
    </div>
    <div>
      <p class="swipe">Swipe right</p>
    </div>
    <p class="watermark">[WATERMARK]</p>
  </div>
</body>
</html>
```

### 6. Write the companion lead-in post

A short text post (150 to 200 words) that sits above the carousel in the LinkedIn composer. Structure:

1. Hook, one line, under 12 words.
2. Context, two or three short sentences.
3. Soft invitation to swipe, one sentence.

Same hard constraints as the slide copy. No hashtags, no emojis, no engagement bait.

### 7. Run self-critique

Before presenting, verify:

- [ ] Slide 1 headline under 10 words.
- [ ] Each content slide carries one idea, not two.
- [ ] All slide copy under the length limits.
- [ ] No banned vocabulary detected.
- [ ] No emdashes anywhere in the output.
- [ ] CTA slide has one clear next step, not three.
- [ ] Operator name or handle on cover and CTA slide.
- [ ] Companion post under 200 words.
- [ ] HTML, if generated, uses the right tokens (operator's file if present, defaults otherwise).
- [ ] Bar test passes on every slide.

Any failures: replace, do not paper over.

### 8. Present the carousel

Deliver in this order:

1. Companion post.
2. Slide-by-slide deck (headline, body, swipe indicator, in one labelled block per slide).
3. HTML files, if requested (one per slide, file names `slide-01.html` through `slide-NN.html`).
4. Self-critique checklist with pass marks.

Then ask:

> Anything you want tightened before this goes into the composer.

### 9. Iterate if asked

The operator may ask for a different angle on one slide, a different CTA, or a tone shift. Refine the affected slides only. Do not regenerate the whole carousel unless the operator asks.

## Hard rules

1. One idea per content slide. No exceptions.
2. Slide 1 is a hook, not a label. No "Here is how to" or "5 tips for".
3. No emdashes anywhere in the output.
4. No emojis, no hashtags, no formatting symbols inside slide copy.
5. The companion post is a lead-in, not a duplicate of the carousel.
6. Operator picks the final CTA shape at intake. Do not pivot mid-flow.

## Edge cases

- **Operator has no brand visual tokens file.** Use the default tokens in step 5. Note the absence at the end of the output and suggest creating `/memory/brand/visual-tokens.md`.
- **Operator pastes an existing blog post as source material.** Treat the blog as the source. Extract the strongest three to six ideas, one per content slide. Do not paraphrase the blog line by line.
- **Topic is genuinely small.** If the topic does not support five distinct content slides, present a five-slide carousel (cover plus three content plus CTA) and flag the shortage. Do not pad with weak slides.
- **Operator wants more than ten slides.** Refuse. LinkedIn caps the format and engagement falls off a cliff after ten. Suggest splitting into two carousels with a thematic link.

## Acceptance tests

Hand-walked during build:

1. **"Build a carousel on the three numbers that decide whether a recruitment firm scales."** Returns a seven-slide deck (cover, five content, CTA), companion post under 200 words, no banned vocabulary, no emdashes, slide 1 headline under 10 words. Operator picks. PASS.
2. **"Carousel for LinkedIn on why most ops fixes are reorgs in disguise. Output copy and HTML."** Returns slide copy plus seven HTML files using default tokens (no brand file present), each file under 4 KB, each file self-contained, watermark slot filled with the operator's handle. PASS.
3. **"Multi-slide post on the case study with the bakery. Soft follow CTA."** Returns deck with five content slides each carrying one idea, CTA slide is one clear next step (soft follow). PASS.
4. **No emdashes anywhere across all expected outputs.** PASS.
5. **Operator with no banned-vocabulary file.** Default list applied, absence noted, suggestion to create the file. PASS.

## Cross-skill boundaries

| Operator wants | Route to |
|---|---|
| Slide-by-slide carousel | this skill |
| Stronger slide 1 hook | `hook-writer`, then return here |
| Full text post draft | post-generator skill (e.g. `linkedin-post-template`) |
| Closing line on a text post | `cta-handraiser` |
| Cross-platform repurpose of an existing piece | `content-repurposer` |
| Weekly content plan | `content-calendar` |

## Maintenance

Update this skill when:

- LinkedIn changes the carousel format (slide count cap, ratio, file type). The hard rules in step 4 are the surface to edit.
- The operator's brand visual tokens change shape. The HTML template in step 5 should track the canonical token names in the operator's file.
- A new platform supports swipeable formats and the operator wants to cross-publish. Add a row to step 5 with the new canvas size and ratio.
