# Concepts

Short reference for the patterns the Leanpreneur Stack is built on. Read this if you are new to agentic IDEs or skills.

## Skill

A self-contained capability described by a `SKILL.md`. The IDE reads the skill's description and triggers, decides when to invoke it, and follows the procedure. Skills compose: one skill can route to another.

## Two tiers

`/core` is a small, curated, maintained set. Each `/core` skill has proven itself in production. They will be kept current.

`/community` is the long tail. Contributed by users. Lighter review. The interesting experiments live here. Some will graduate to `/core`.

## Context layer

Most skills assume a private context layer. That is your memory, your entity files, your distilled notes, your project history. The Stack does not ship anyone else's context. You bring your own.

Skills reference the context layer with generic paths (`/memory/entities/`, `/memory/distilled/`, etc.) that you map to your own structure.

## Triggers

Each skill declares phrases that should invoke it. The IDE matches user input to triggers. Phrases are imperative and human ("prep me for", "draft outreach to", "lookup"). Avoid abstract triggers like "help with X".

## Hard rules

A skill defends its own boundaries. Read-only skills must never write. Drafts-only skills must never autosend. Skills with strict output formats must not improvise. The `Hard rules` section in a `SKILL.md` is where the skill author commits the procedure to the bar of trust required to run it on autopilot.

## Acceptance tests

A skill that lasts is a skill that has tests. Hand-walked or automated. The `Acceptance tests` section gives a future maintainer the confidence to refactor without breaking expectations.
