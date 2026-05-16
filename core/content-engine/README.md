# /core/content-engine

A pattern for running a personal content engine across LinkedIn, X, blog, newsletter, and video without hiring a content team.

## What this is

Four composable skills that turn ad-hoc posting into a system. Each skill is one focused capability. They compose, but each works standalone.

| Skill | Job | Use when |
|---|---|---|
| [hook-writer](./hook-writer/) | Generate 10 candidate first lines for one piece of content. | The opener is weak or missing. |
| [content-calendar](./content-calendar/) | Build a 1 to 8 week posting calendar across platforms. | Staring at a blank week. |
| [cta-handraiser](./cta-handraiser/) | Write one earned CTA per post. | The post is drafted, the closer is missing. |
| [comment-strategy](./comment-strategy/) | Draft 2 to 3 candidate comments on a specific post for a warm-up campaign. | Doing comment-first networking, not cold outreach. |

## How they compose

A typical week using all four:

1. **Sunday or Monday:** invoke `content-calendar` for the coming week. Get a table of dated posts, platforms, pillars, and slots for interview derivatives.
2. **Tuesday morning:** for Tuesday's post, invoke `hook-writer`. Pick a hook from the ten candidates. Draft the body manually (or via your own post-generator skill, see below).
3. **Tuesday afternoon:** invoke `cta-handraiser` to close the post. Ship.
4. **Daily, throughout the week:** during a 30-minute engagement block, invoke `comment-strategy` on each target's posts. 10 comments per day across 5 target accounts and 5 trending posts.

The four skills do not invoke each other. They compose because the operator runs them in sequence. That keeps each skill simple and forkable.

## What this is not

This engine ships the **scaffolding**. It does not ship:

- The operator's voice. Voice lives in `/memory/voice/` in your own context layer. The skills reference it; they do not define it.
- The operator's offer ladder. Offers live in `/memory/offers/` in your own context layer.
- A post-generator. Drafting full post bodies is downstream of the hook and the closer. Bring your own, or use the (separately shipped) post-generator skills in `/community/`.
- A reverse-engineering tool. Studying which operators you want to write like is a one-off curation pass, not a runtime skill.

This is intentional. The voice, offers, and judgment stay yours. The mechanics get standardised.

## Setup

1. Clone or download the skills you want from this folder into your own `/skills/` directory (or wherever your agentic IDE looks for skills).
2. Read each `SKILL.md` to understand its triggers and procedure.
3. Create the supporting context files referenced in each skill's `context_loads:` block. At minimum:
   - `/memory/voice/voice-rules.md` (3 to 5 sentences of how you write)
   - `/memory/voice/banned-vocabulary.md` (words you refuse to use)
   - `/memory/voice/content-pillars.md` (your 3 to 5 named pillars)
   - `/memory/offers/offer-ladder.md` (your top, mid, bottom funnel offers)

The skills work without these files (each has a fallback default), but they drift toward generic AI marketing copy when the context files are missing. Build the context, get the leverage.

## Adapting

Each skill is short and explicit on purpose. Fork them. Adapt the platform defaults, the taxonomies, the hard rules, the offer ladder. The `SKILL.md` is the operator-readable contract. You change the file, you change the behaviour.

## Sequence and dependencies

None of these four skills depend on the others at runtime. Composition is human-orchestrated.

Two skills in the wider content engine are explicit downstream consumers:

- `carousel-builder` (separately shipped) depends on `hook-writer` for slide one.
- `content-repurposer` (separately shipped) depends on `hook-writer` and `content-calendar` for cross-platform variants.
- `video-to-content` (separately shipped) depends on `content-repurposer` for derivative posts from interviews.

If you ship the full content engine, expect to add those three over time.

## Portability

These skills follow the Stack's portability constitution. Model calls inside skill bodies refer to model tiers (`precision`, `default`, `fast`, `research`), not vendor model names. Frontmatter `model:` fields use the same tier names. The mapping from tier to vendor model id lives in exactly one place in your consuming OS: a `tools/model-call.py` shim with the contract `call_model(prompt, tier="default") -> str`.

That means you can run /core skills on Anthropic, OpenAI, Google, or local Llama without touching skill bodies. One file swap.

See the root [README](../../README.md) for the full portability rationale.

## House style

- No emdashes anywhere in any skill, ever. Hyphens, commas, full stops only.
- Procedure sections are numbered and explicit.
- Skills assume a private context layer. Generic paths so others can swap in their own.

## Contributing

Variants and improvements welcome. See the root [CONTRIBUTING.md](../../CONTRIBUTING.md) for the contribution flow. Open PRs against `/community/<your-handle>/<skill-name>/` first. Promotion to `/core` happens after 30+ days of production use.

## Status

v1, May 2026 first wave. Five more content-engine skills queued for `/core` in subsequent ships (carousel-builder, content-repurposer, video-to-content, and two others). The /community side is open for variant implementations now.
