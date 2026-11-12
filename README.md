# brewphase-life-path

A cross-agent skill for thinking about life direction through the **verified life
paths of real people** — from ancient history to people still alive — instead of
through motivational quotes.

BrewPhase Life Path is not a collection of inspiring stories. It is a method:
abstract your situation into a searchable pattern, retrieve comparable lives
across regions and eras, verify what actually happened, separate what transfers
from what does not, and end with two or three routes and one thing to do next.

> 中文说明见 [README.zh-CN.md](README.zh-CN.md)

---

## Why this exists

Most "life advice" tooling fails in the same three ways:

1. **It draws from a narrow sample.** A handful of American founders, read as
   universal.
2. **It drops the conditions.** A success is described as effort plus belief,
   with the family money, the timing and the institution quietly removed.
3. **It predicts.** A date is attached to a life, and the person measures
   themselves against it.

This skill is built to avoid all three. Coverage is explicitly global; every
trajectory must include enabling conditions and costs; and time statements are
always ranges with stated assumptions, never dates or probabilities.

## What it does

- Reads your own confirmed direction and current work context from **BrewPhase
  Desktop** over MCP, if you have it running and authorized
- Searches widely for people whose *constraints* resemble yours — not whose
  fame resembles your ambition
- Verifies claims against primary and scholarly sources, and labels what is fact,
  what is the person's own account, and what is inference
- Produces two or three paths that fit your real situation, with costs and
  failure signals
- Ends with 1–3 next actions and an explicit list of what not to do now

## What it does not do

- No personality tests, no similarity percentages ("you are 80% like X")
- No life-outcome predictions, no success probabilities
- No bundled biography database — every case is retrieved and verified at use
- No changes to your BrewPhase data: it can only *propose* drafts, which you
  confirm in the app
- No motivational content, and no anxiety manufactured from other people's
  achievements

## Install

The canonical skill lives at `skills/brewphase-life-path/SKILL.md`.

```bash
npx skills add banmu123/brewphase-life-path
```

Other layouts:

| Agent | Path | Notes |
|---|---|---|
| Codex / `.agents` convention | `.agents/skills/brewphase-life-path` (symlink in this repo) | See [adapters/codex](adapters/codex/README.md) |
| Claude Code / compatible | copy or symlink `skills/brewphase-life-path` into your skills directory | — |
| WorkBuddy | see [adapters/workbuddy](adapters/workbuddy/README.md) | Import path is **not** verified; do not assume the CLI above installs it |

Once installed, you should not need to invoke it by name. The `description` in
`SKILL.md` is written so that agents matching on user intent will select it when
the conversation is about direction, transition, feeling stuck, or learning from
someone else's life.

## Optional: connect BrewPhase

The skill works without BrewPhase, using only what you say in conversation. If
you also run [BrewPhase Desktop](https://github.com/banmu123/BrewPhase), you can
authorize its local MCP bridge so the analysis can read your confirmed current
state, directions, stages and work context.

Requirements:

- BrewPhase Desktop running on the same machine
- MCP bridge enabled, with a client authorized for `direction:read` (and
  `work_context:read` for work context)
- Scopes are granted by you in the app; nothing is read without them

The MCP server is local. It is not a source about the wider world — world
biography always comes from the agent's own search. See
[references/brewphase-mcp.md](skills/brewphase-life-path/references/brewphase-mcp.md).

## Structure

```
skills/brewphase-life-path/
├── SKILL.md                          # the only canonical entry point
└── references/
    ├── coverage-map.md               # global coverage matrix + starting anchors
    ├── historical-pattern-matching.md# the five-step research procedure
    ├── source-evaluation.md          # four-tier evidence grading
    ├── personal-analysis.md          # matching on transferable constraints
    ├── path-planning.md              # routes, sequencing, next actions
    ├── forecast-guidelines.md        # ranges, scenarios, what not to predict
    ├── brewphase-mcp.md              # reading the user's own context
    └── response-template.md          # three output sizes
```

`SKILL.md` is deliberately short: it decides *whether* to act and *in what
order*. The references carry the method. Agents load them selectively, which
keeps the common case cheap.

## Design rules

These are the constraints that keep the output honest, and they are enforced in
the reference files:

1. **Never invent a life.** Missing facts are acceptable; fabricated ones are not.
2. **Always name conditions and costs**, not just outcomes.
3. **Give differences the same weight as similarities.**
4. **At least one counter-case** in every full analysis — someone who tried and
   did not arrive, or arrived and paid a price.
5. **Coverage floor**: ≥2 historical, ≥2 modern/contemporary, ≥3 cultural
   backgrounds, ≥3 path types. Not four founders from one country.
6. **Ranges, not dates.** No success probabilities.
7. **Four labels, never blended**: fact / their account / scholarly view /
   inference.
8. **Privacy boundary**: the user's material never enters a public search; the
   search uses an abstracted pattern only.

## Tests

The `tests/` directory holds scenario specifications used to check the skill
manually and in review:

- `trigger-scenarios.md` — when it should and should not fire
- `coverage-scenarios.md` — global and domain coverage behaviour
- `evaluation-scenarios.md` — 14 analysis scenarios with expected properties

These are specifications for judgement, not automated assertions. See each file
for how to run a check.

## License

MIT — see [LICENSE](LICENSE).

## Contributing

Corrections to the method, better source standards, and additional coverage
anchors are welcome. Read [CONTRIBUTING.md](CONTRIBUTING.md) first — in
particular the rule that **no person, event or source may be added from memory
without a traceable citation**.
