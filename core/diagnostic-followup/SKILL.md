---
name: diagnostic-followup
description: Turn a completed self-assessment submission into a bespoke report plus a routed follow-up email draft, end to end in well under an hour, drafts only. Use when a respondent finishes your diagnostic and a submission lands for processing. Grounds the report in your archetype library and routing rules; routes the call to action by the respondent's size and score. Never sends.
triggers:
  - process the diagnostic submissions
  - run the assessment follow-up
  - generate the report for
  - follow up the diagnostic
context_loads:
  - your submission inbox (one JSON per completed assessment)
  - your scoring config (dimensions, weights, layer bands)
  - your archetype library (profiles, dimension narratives, opportunities)
  - your routing rules (size tier and score to offer)
  - your moves dictionary (recommended next actions per dimension)
  - your sample report (the canonical shape and tone)
version: 1.0
---

# diagnostic-followup

A pattern for turning a completed self-assessment into a bespoke report and a routed follow-up email, fast enough to do at volume, drafts only at both ends.

If you run a diagnostic (a scored self-assessment that places a respondent on a maturity model), the assessment only earns its keep when the follow-up is fast, bespoke, and routed to the right next step. A generic "thanks for completing it" email throws away the signal the respondent just gave you. This skill is the bridge from a raw submission to a piece of follow-up that lands in their inbox: a report that reads like an experienced operator reading the numbers, and an email whose call to action matches the respondent's size and score.

It is drafts only at both ends. The report is a draft you review. The email is a draft you send manually. There is no send path.

## When to use

- A respondent completes your diagnostic and a submission lands for processing.
- You want to process a batch of submissions, or follow up one named respondent.

## When not to use

- Sending. This skill never sends. It produces a report draft and an email draft.
- A cold prospect who has not completed the diagnostic. That is an outreach pattern, not this one. This works completed submissions only.
- Editing the diagnostic itself (questions, archetypes, routing). That is a change to the assessment, not a follow-up.

## Prerequisites

You maintain six things. Adapt the paths and shapes to your layout.

1. A submission inbox: one JSON file per completed assessment, dropped by your form's webhook. A useful shape: a unique `submission_id`, an `intake` block (the respondent's size tier, sector, primary function, stated context), and an `answers` block (per-question levels).
2. A scoring config: your dimensions, the weight of each question, and the layer bands that map a 0-100 score to a maturity level.
3. An archetype library: named patterns keyed by size tier and maturity band, each with a profile, a per-dimension narrative, a list of opportunities, and the next move. This is what makes the report bespoke rather than generic. It is also your moat; keep it private.
4. Routing rules: a table that maps the respondent's size tier and score to the right call to action. The same score means a different next step at different sizes.
5. A moves dictionary: recommended next actions per dimension, at a few horizons (a thirty-day self-serve move, a sixty-day supported move, a ninety-day done-for-you move).
6. A sample report: one worked example that fixes the section order and the tone. Every generated report follows its shape.

## The pattern

### 1. Scan (deterministic, no judgment)

A small script walks the inbox, validates each submission's shape, drops anything already processed (keep an append-only processed-state index so nothing is reprocessed and nothing is deleted), and writes a worklist of new submissions. Keeping scan and dedupe in code, not prose, is what makes the volume safe.

### 2. Score (deterministic, no judgment)

For each new submission, a script computes the scaffold and writes it to a per-submission folder:

- per-dimension scores and layer band
- the composite score and its layer
- the floor placement: the weakest dimension. Treat the floor as the real diagnostic and the composite as the headline. A respondent is as capable as their weakest pillar, because the weakest pillar dictates how leveraged the operation actually is.
- the matched archetype, from your selection logic (size tier by maturity band, with a sector-specific match preferred where one exists, and any special-case overrides you define)
- the routed call to action: size tier by score band, the offer for that cell from your routing rules, and any secondary call to action your rules trigger
- the three recommended moves, drawn from the moves dictionary keyed off the three lowest-scoring dimensions, at the horizon the composite implies

Keeping the maths in code means the numbers cannot drift between submissions. The judgment work happens next, on a clean scaffold.

### 3. Write the bespoke report

Read the matched archetype's full block and the sample report for shape and tone. Then write the report in the sample's section order. A strong default order:

1. Headline: composite score, composite layer, floor placement and the dimension that drives it
2. Profile recap from the intake
3. Placement: the composite as the headline, the floor as the real diagnostic, the archetype named
4. Why the floor matters more than the average
5. Per-dimension breakdown (score, layer, a simple bar)
6. Bespoke commentary, one short paragraph per dimension, each grounded in the archetype's narrative but specialised to this respondent's scores and stated context
7. A capacity estimate: a function-by-function range of time the operation could plausibly recover, as a range, never a point estimate, with the method stated
8. The top prioritised opportunities, ranked by impact against effort, from the archetype's list specialised to the scores
9. Tool-agnostic recommendations: workflow-shaped recommendations, each with a few tool options scored on a robust-systems rubric (data ownership, exit ease, audit trail, vendor stability). Never recommend a single named product as the answer; recommend the workflow shape and let the respondent pick the tool. Pull from a maintained registry; never invent a product.
10. The three recommended moves from the scaffold
11. A short "how others got here" section: a couple of public anchors mapping where they are to the next jump
12. How you work with operations their size: the self-serve, supported, and done-for-you column for their size tier
13. Next steps: a few options in order of fit, plus any secondary call to action your rules flagged

Route the model calls through your model-call abstraction so the model choice lives in one place: a precision tier for the report prose, a fast tier for any quick classification.

Save the report to the per-submission folder. If you produce a styled document, apply your own brand tokens.

### 4. Draft the follow-up email

Write a short email in your own voice that does three things: names what the report found in one line (lead with the floor, not the composite), points at the single highest-leverage move, and routes to the call to action the scaffold chose. The call to action is not "buy something"; it is the right next step for this respondent's size and score. Low score or small size routes to a self-serve next step; mid routes to a supported one; high routes to a conversation. Where your routing rules say so, add a soft secondary call to action.

Keep it short: name the workflow before any offer, one ask, one question, no em-dashes, no exclamation marks, consistent regional spelling, one link maximum. The diagnostic exists to make the next move clear, not to sell. Say so.

Save the email as its own file with a short header (the key numbers and the chosen call to action) above the body, so you have the context at a glance.

### 5. Register

A second deterministic pass rebuilds a dated batch review file from the per-submission folders so the report and email drafts surface in your approval queue, and marks each registered submission processed in the append-only state index. You review the report, then send the email manually.

### 6. Report back

One short summary: how many processed, the size-tier and archetype split, anyone flagged (invalid shape, missing contact details, a score on a band edge worth a human eye), and the review file path.

## Hard rules

1. Drafts only. The report is a draft you review; the email is a draft you send manually. There is no send path.
2. Lead with the floor, not the composite. The weakest pillar is the real diagnostic.
3. Never invent a product in the tool-agnostic recommendations. Pick from a registry and show the rubric score so the respondent sees the trade-off.
4. Name the workflow before any offer. The diagnostic makes the next move clear; it does not sell.
5. No em-dashes anywhere in output. No exclamation marks in the email. Consistent regional spelling. One link maximum.
6. Never overwrite or delete a submission. State is append-only.

## Why a script plus a prompt

The deterministic parts (scanning the inbox, validating shape, deduping, computing scores, selecting the archetype, routing the call to action, building the review file) belong in a small script so the maths cannot drift and the safety rails cannot be skipped on a busy day. The judgment parts (writing the bespoke commentary in voice, choosing how to frame the next step) belong in the model call. Splitting them keeps each side small and auditable. This mirrors the broader two-pass convention across this library: a mechanical pass computes the scaffold, a judgment pass does the writing.

## Portability

This skill follows the Stack's portability constitution. Model calls in the body refer to tiers (`precision`, `default`, `fast`, `research`), not vendor names. The mapping from tier to vendor model id lives in exactly one place in your consuming OS: a `tools/model-call.py` shim with the contract `call_model(prompt, tier="default") -> str`. One file swap moves the whole library to a different vendor.

## Maintenance

Update this skill when your scoring config changes (dimensions, weights, bands), when the archetype library or routing rules revise, when the submission schema changes, or when the report shape evolves.
