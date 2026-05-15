# The Leanpreneur Stack

Open-source skills, patterns and scaffolding for solo operators and founder-led teams who want to run leaner, more leveraged businesses.

Maintained by [Alex Lockey](https://alexlockey.com) and the [Leanpreneur Community](https://alexlockey.com/community).

## What this is

A two-tier library of skills that plug into Claude Code (and other agentic IDEs that follow the same `SKILL.md` pattern).

Each skill is one focused capability: a `SKILL.md` describing when to invoke it, what it does, and how. Skills compose. Together they form an operating substrate you can drop into your own setup, fork, and extend.

The Stack is the public artefact behind a private system Alex runs across four businesses. Skills graduate to `/core` once they have proven themselves in real work. `/community` is where contributors ship their own.

## Who it is for

Operators, solopreneurs, and small founder-led teams who:

- Already use Claude Code, Cursor, or another agentic IDE for non-coding work
- Want patterns that scale headcount-light, not headcount-heavy
- Prefer building leverage over hiring more people
- Treat AI as a tool, not a strategy

If you are looking for a turnkey AI-agency consultancy, this is not it. The Stack is raw materials. You assemble.

## Repository structure

```
/core              Skills curated and maintained by Alex Lockey
/community         Skills contributed by Leanpreneur Community members
/docs/concepts.md  What a skill is, two-tier model, context layer
/docs/patterns/    Adoptable behavioural rules and operating patterns
LICENSE            MIT
README.md          This file
CONTRIBUTING.md    How to add a skill
```

Each skill lives in its own folder and follows the `SKILL.md` convention popularised by Anthropic's skills ecosystem. A skill folder may also contain reference files, examples, or scripts.

`/docs/patterns/` is for adoptable behavioural rules — operating conventions you slot into your own context layer, not skills that an IDE invokes. Examples: trust ladders, error-iteration discipline, broken-systems tracking. They sit alongside skills because most operators need both.

## Portable by design

The Stack ships skills, patterns, and conventions. None of them name a vendor model.

Skill bodies refer to models by tier (`precision`, `default`, `fast`, `research`), not by name (`opus`, `gpt-5`, etc). Frontmatter `model:` fields, if present, use the same tier names. The mapping from tier to vendor model id lives in exactly one place: a `tools/model-call.py` shim in the consuming OS.

That means you can run /core skills on Anthropic, OpenAI, Google, or local Llama without touching skill bodies. You change one file when you swap vendors. The Stack stays vendor-neutral as a constitutional commitment.

If you fork The Stack into your own OS, drop in a shim with the same `call_model(prompt, tier="default") -> str` contract and you inherit portability.

**Why this matters for an open-source skills library.** A skill that says "use Opus to do X" works for one vendor and one moment in time. A skill that says "use a precision model to do X" works forever, for every vendor, and stays alive as model markets churn.

## How to use a skill

1. Clone or download the repo.
2. Copy the skill folder you want into your own `/skills/` directory (Claude Code, Cursor, or wherever your agentic IDE looks for skills).
3. Read the `SKILL.md` to understand the trigger phrases and the procedure.
4. Adapt the file paths, entity references, and connector calls to your own setup. Most skills assume a private context layer (memory, entities, distilled notes) that you will need to point at your own.
5. Trigger it from your IDE.

## How to contribute

See [CONTRIBUTING.md](./CONTRIBUTING.md). Short version: open a PR with your skill under `/community/<your-handle>/<skill-name>/SKILL.md`. `/core` is a smaller curated set, gated by Alex.

## Bridge page

A guided overview, install walkthroughs, and the diagnostic that points you at the right skills for your context will live at [alexlockey.com/stack](https://alexlockey.com/stack). That page is the recommended starting point if you are new to agentic IDEs.

## Status

v1, active build. Expect rough edges and frequent additions during May to August 2026 as the first wave of skills lands.

## Credit

Built and maintained by Alex Lockey. Community contributions credited per skill folder.
