---
name: entity-lookup
description: Look up a person or company from your canonical entity layer at /memory/entities/. Returns the entity file contents plus a deterministic briefing summary and cross-links to related entities. Use when the operator says "who is X", "what do we know about [name or company]", "lookup [name]", "tell me about [company]", "entity for [X]", or "who is this contact". Read-only. Does not enrich from external sources and does not create new entity files.
triggers:
  - who is
  - what do we know about
  - lookup
  - tell me about
  - entity for
  - who is this contact
  - pull the entity
  - pull the file on
  - rolodex lookup
context_loads:
  - /context/filing-rules.md
  - /memory/entities/README.md
  - /memory/entities/ENTITY-INDEX.md
version: 1.0
---

# entity-lookup

Read-only lookup against your canonical entity layer. One person or one company per invocation. Returns a structured briefing, not prose narrative.

This skill assumes you maintain an entity layer under `/memory/entities/` with `people/` and `companies/` subfolders, each holding one Markdown file per entity, plus an `ENTITY-INDEX.md`. Adapt the paths if your layout differs.

## When to use

- The operator asks "who is [name]" or "what do we know about [name or company]".
- A scheduled task or another skill needs the canonical record for a person or company (pre-call prep, draft outreach, daily briefing).
- An email or LinkedIn URL needs to be resolved to a known contact before drafting a reply.

## When not to use

- Creating a new entity. That is a separate `create-entity` skill.
- Updating an entity file. Edit the file directly.
- Enriching a contact with external data (Apollo, LinkedIn, web). That is the enrichment skill set.
- Bulk queries ("everyone at <company>", "all hot prospects"). Out of scope for v1.

## Inputs

One of:

- Person name (full, partial, or nickname)
- Company name (formal or informal)
- Email address
- LinkedIn URL

If the operator says "who is this" with no name, ask which person or company before proceeding.

## Procedure

### 1. Normalise the input

Determine input type:

- Contains `@` -> email
- Contains `linkedin.com` -> LinkedIn URL
- Otherwise -> name (person or company)

For a name input, decide person vs company:

- Two title-cased tokens -> person first, fall back to company.
- One token, or contains `Group`, `Ltd`, `Limited`, `Inc`, `LLC`, `PLC` -> company first, fall back to person.
- Ambiguous -> run both paths and disambiguate if both return hits.

### 2. Compute the slug

Apply your filing rules. Default rules:

- Lowercase, hyphenated, ASCII.
- Person default: `{firstname}-{lastname}`.
- Person with company disambiguator: `{firstname}-{lastname}-{company}`.
- Company: lowercase full name, spaces to hyphens, strip `Ltd`, `Limited`, `Inc`, `LLC`, `PLC`.
- Strip apostrophes, ampersands, slashes, dots.

### 3. Direct read

Try the direct path in this order:

1. Person: `/memory/entities/people/<slug>.md`
2. Company: `/memory/entities/companies/<slug>.md`

If one file exists, go to step 5. If both exist, prefer the one matching the input-type guess from step 1, and note the other in the output.

### 4. Fallback grep

If direct read misses, grep across `/memory/entities/`. Order of passes:

1. Email input: grep the email string in `/memory/entities/people/` and `/memory/entities/companies/` frontmatter and body.
2. LinkedIn URL input: grep the URL (normalise trailing slash off).
3. Name input: grep for the `name:` frontmatter value, case-insensitive. Then grep for the name in `# <Name>` H1 headings. Then anywhere in `/memory/entities/`.
4. Partial name: grep on the token as a substring of slug filenames.
5. Also grep `/memory/entities/ENTITY-INDEX.md`. The index typically carries warmth and company columns and can disambiguate.

Result handling:

- Zero hits -> step 6.
- One hit -> read the file, go to step 5.
- Multiple hits -> list up to 5 candidates with `<slug>.md`, name, company, warmth, last_interaction. Ask which one. Do not guess.

### 5. Compose the briefing

Deterministic block. Same shape every call. No LLM narrative.

**For a person:**

```
# <Full Name>

<slug>.md <last_interaction> <warmth> <relationship>

## At a glance
- Role: <role or null>
- Company: <company or null>  (link if company entity file exists)
- Email: <email or null>
- LinkedIn: <linkedin or null>
- First contact: <first_contact>
- Last interaction: <last_interaction>
- Source: <source>
- Tags: <tags>

## Current status
<Current Status body from file. One line.>

## Next action
<Next Action body from file. One line.>

## Recent relationship history (last 3)
- <most recent item verbatim>
- <second item>
- <third item>

## Cross-links
- Company: [<company>](../companies/<slug>.md) - <company warmth>, <company Next Action first line>  (if company entity exists)

## Full file
<entire body of the entity file, unchanged>
```

**For a company:**

```
# <Company Name>

<slug>.md <last_interaction> <warmth> <relationship>

## At a glance
- Sector: <sector or null>
- Size: <size or null>
- Website: <website or null>
- First contact: <first_contact>
- Last interaction: <last_interaction>
- Source: <source>
- Tags: <tags>

## Current status
<Current Status body from file. One line.>

## Next action
<Next Action body from file. One line.>

## Key people
<parsed table of Key People from file. For each row, if the person's entity file exists, include warmth and last_interaction next to the name.>

## Recent relationship history (last 3)
- <most recent item>
- <second item>
- <third item>

## Full file
<entire body of the entity file, unchanged>
```

Formatting rules:

- No emdashes. Hyphens, commas, full stops only.
- Missing fields render as `null`, not empty.
- If `Recent relationship history` has fewer than three items, render whatever exists.
- Keep `## Key Context` or `## Notes` sections in the Full file dump. Do not elevate.

### 6. Missing-entity path

If neither direct read nor grep returned a hit, respond:

```
No entity file found for "<input>" at /memory/entities/people/ or /memory/entities/companies/.

Options:
1. Run a corpus-wide research pass for mentions of "<input>".
2. Create a new entity file. I will ask for the required fields.

If the name was a typo or nickname, try again with the canonical form.
```

Then, as a best-effort fallback, grep once across `/memory/distilled/`, `/wiki/live/`, `/memory/projects/`, and `/memory/decisions/` for the name. List up to 5 file paths with one-line context. Do not dump full file contents.

### 7. Report back

Return the briefing block from step 5 (or the missing-entity block from step 6). No preamble, no trailing summary.

If the next operator message is "update it" or "edit the next action to X", do not attempt the edit. Tell the operator updates go direct-to-file, or route to a future `update-entity` skill. This skill is read-only.

## Cross-skill boundaries

| Operator wants | Route to |
|---|---|
| Canonical entity record plus briefing | this skill |
| Create a new entity | `create-entity` skill |
| Corpus-wide search (not entity-centric) | `os-research` skill |
| Warm-up for a call with this person or company | `meeting-prep` skill |
| External enrichment (Apollo, web) | enrichment skill set |
| Update an entity field | direct file edit |

## Hard rules

1. Read-only. Never write to `/memory/entities/`.
2. Never invent contact details. Missing field renders `null`.
3. Never overwrite a file. This skill does not Write.
4. No emdashes anywhere in output.
5. On ambiguous matches, list and ask. Do not guess.
6. The briefing block shape is fixed. Do not improvise new sections.

## Acceptance tests

- Given `"Jane Smith"`, returns `/memory/entities/people/jane-smith.md` briefing with cross-link to her company entity if one exists.
- Given `"Acme Corp"`, disambiguates to `acme-corp.md` company briefing with parsed Key People table.
- Given `"jane@acme.com"`, resolves via email grep.
- Given a name with no matching file, returns the missing-entity block plus best-effort corpus mentions.
- Where a company file and a person file share a slug, flags the collision and asks which.
- Output contains no emdashes and no emdash substitutes (double hyphen).

## Maintenance

Update this skill when:

- The entity schema changes (new frontmatter field, renamed section). Align with your filing rules first.
- Heavy-use entities start using a multi-file entity folder pattern. Add a step that includes folder contents when present.
- Operator phrases drift and new triggers should route here.
