# Contributing to the Leanpreneur Stack

Two tiers, two different bars.

## /community

Lighter review. Default home for contributed skills.

**To contribute:**

1. Fork the repo.
2. Create a folder at `/community/<your-handle>/<skill-name>/`.
3. Add a `SKILL.md` following the structure below.
4. Add reference files, examples, or scripts in the same folder if needed.
5. Open a PR. Title format: `community: <skill-name> by <handle>`.

**Review criteria:**

- The skill solves a real, named operator problem.
- The `SKILL.md` is clear enough that a stranger could trigger and use it.
- No private contact details, internal company names, or personal data leaked into the skill.
- No emdashes anywhere in the skill text. Hyphens, commas, full stops only. (House style.)
- MIT-compatible. Don't paste in proprietary code.

Reviewers will leave comments. Once approved, the PR merges.

## /core

Tighter, curated set. Each `/core` skill must have proven itself in production at least one operator's setup, ideally Alex's first.

**To propose a /core graduation:**

1. The skill must already exist in `/community` and have been used by you in real work for at least 30 days.
2. Open an issue titled `core graduation: <skill-name>`. Include: what the skill does, how often you have triggered it, what changed about your work since adopting it, and what edge cases you have hit.
3. Alex reviews. May accept, may suggest revisions, may decline. `/core` stays small on purpose.

## Skill file structure

Each `SKILL.md` should include:

```yaml
---
name: <slug>
description: <one paragraph. when to invoke, what it does, what it does not do.>
triggers:
  - <phrase 1>
  - <phrase 2>
context_loads:
  - <file path or doc reference, optional>
version: <semver, start at 0.1>
---
```

Followed by:

- `# <skill name>` heading
- `## When to use` and `## When not to use`
- `## Inputs`
- `## Procedure` (numbered steps)
- `## Hard rules` (non-negotiables)
- `## Edge cases` (optional)
- `## Acceptance tests` (optional but recommended)

Look at any skill in `/core` for a worked example.

## House style

- No emdashes anywhere in any skill, ever. Use hyphens, commas, full stops.
- Procedure sections are numbered and explicit. A skill is a procedure, not an essay.
- Skills assume a private context layer (memory, entities). Reference yours generically (`/memory/entities/`) so others can swap in their own.
- Acceptance tests are strongly encouraged. They are how a skill defends itself against drift.

## Code of conduct

Be useful. Be specific. Operators are busy.

## Licence

By contributing, you agree your contribution is released under the MIT licence (see [LICENSE](./LICENSE)).
