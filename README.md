# Agent-Maintained Project Base

> **This is the base's README, not a product README.** A fork replaces this
> file and [`README.zh-CN.md`](README.zh-CN.md) with the product's own README
> before the first release. The fork steps live in
> [`docs/bootstrap.md`](docs/bootstrap.md), so the replacement loses nothing.
> An agent that still sees this notice has not finished bootstrap.

This repository is a **fork-ready base for projects whose code is written,
reviewed, merged and released by coding agents**, with a human keeping
direction and taste. It is not an application. It is the operating system
around the agents: the routing table, the rules, the wrong-answer notebook,
the community gates and the three loops that keep the project moving.

The model comes from [yetone/magpie](https://github.com/yetone/magpie):
three long-running agent loops, one per entrance — GitHub/community, social
feedback, and the codebase itself — plus a `LESSONS.md` that turns repeated
mistakes into hard rules. This base keeps that mechanism and removes the
product-specific parts.

## Start here

1. Read [`docs/bootstrap.md`](docs/bootstrap.md) and fill every `{{TOKEN}}`.
2. Write [`docs/project-brief.md`](docs/project-brief.md). Agents cannot make
   product decisions they were never told about.
3. Record the decisions you have already made, and the taste you will not
   re-litigate, in [`docs/decisions.md`](docs/decisions.md).
4. Build the real reproduction environments in
   [`docs/environments.md`](docs/environments.md). An agent that cannot run
   the failing surface cannot verify a fix.
5. Choose the schedule and models for the three loops in
   [`docs/loops/README.md`](docs/loops/README.md).
6. Start with one loop, not three. Add the next when the first stops
   producing rework.

## How the repository is wired

`CLAUDE.md` contains two imports. It is deliberately thin:

```
@AGENTS.md
@LESSONS.md
```

`AGENTS.md` is a **routing table**, not a rulebook. It says which document to
read before which kind of work:

| Task | Read first |
| --- | --- |
| Any change to code | [`LESSONS.md`](LESSONS.md), then [`docs/code-standards.md`](docs/code-standards.md) |
| Change to a documented subsystem | [`docs/subsystems/README.md`](docs/subsystems/README.md) and the subsystem page |
| Review or merge a community PR | [`docs/contributors.md`](docs/contributors.md) — harm check first |
| Release or tag | [`docs/release.md`](docs/release.md) and the acceptance criteria |
| Pick a design, a default or a UI behavior | [`docs/decisions.md`](docs/decisions.md), [`docs/project-brief.md`](docs/project-brief.md) |
| Reproduce a user's report | [`docs/environments.md`](docs/environments.md) |
| Run an autonomous loop | [`docs/loops/`](docs/loops/) |
| Write or promote a lesson | [`LESSONS.md`](LESSONS.md) |

`LESSONS.md` is the **wrong-answer notebook**. Every night the day's commits
are reviewed for three things: what had to be fixed again, what should not
have merged, and what was verified too thinly. Each finding becomes one rule
with evidence. A lesson seen on **three separate days** is promoted into
`docs/code-standards.md`, where every review checks it. `LESSONS.md` is read
before any code change, so the next agent inherits the mistake.

Everything else is a reference the routing table points at.

## Directory map

```
CLAUDE.md                     entry point: imports AGENTS.md and LESSONS.md
AGENTS.md                     routing table and hard invariants
LESSONS.md                    living wrong-answer notebook (mechanism + ledger)
README.md / README.zh-CN.md   base README; a fork replaces it with the product's
CHANGELOG.md                  product changelog template; one entry per version
LICENSE                       MIT by default; replace it if you choose another
SECURITY.md                   how to report a vulnerability privately
CONTRIBUTING.md               what a human contributor needs before a PR
docs/
  BASE-CHANGELOG.md           what changed in the base; a fork reads this
  bootstrap.md                fork checklist; every placeholder token
  project-brief.md            product, users, non-goals, constraints
  decisions.md                deliberate decisions and taste; "don't reverse"
  code-standards.md           change rules, tests, acceptance, review, commits
  contributors.md             community PR harm checks, credit, block list
  release.md                  release and tag gates
  environments.md             real-run matrix; sandbox and credential rules
  agents-memory.md            how memory and repo docs divide the work
  extraction-map.md           magpie source → this base (audit trail)
  subsystems/
    README.md                 how to write and review subsystem references
    _template.md              page skeleton
  standards/
    language-addendum-template.md
    go.md                     extracted Go rules (example addendum)
    gui.md                    extracted GUI rules (example addendum)
  loops/
    README.md                 cadence, model routing, safety, metrics
    1-github-and-community.md  issue triage, PR review, release, replies
    2-social-feedback.md       social listening with a cheap filter first
    3-nightly-review.md        cleanup, lessons, block-list proposals
.github/
  pull_request_template.md
  ISSUE_TEMPLATE/bug_report.md
  ISSUE_TEMPLATE/feature_request.md
```

Direct links for the files a fork touches first:
[中文说明](README.zh-CN.md), [base changelog](docs/BASE-CHANGELOG.md),
[product changelog template](CHANGELOG.md), [license](LICENSE),
[security policy](SECURITY.md), [contributing](CONTRIBUTING.md),
[language addendum template](docs/standards/language-addendum-template.md),
[Go addendum](docs/standards/go.md), [GUI addendum](docs/standards/gui.md).

## The three loops

| Loop | Entrance | What it does | Human keeps |
| --- | --- | --- | --- |
| 1 | GitHub + community chat | Triage issues, review and merge PRs, release, reply | Security gates, product direction |
| 2 | Social media | Filter mentions, reproduce real feedback, fix, ship, reply | Tone and public crisis |
| 3 | The codebase itself | Nightly review of commits, lessons, cleanup, block-list proposal | The block-list decision |

The loops do not share a queue. Each one owns one entrance and keeps its own
checkpoint. If two loops can act on the same item, the item belongs to the
loop that owns its entrance; the other loop links to it and does not act.

## The non-negotiables

These are the parts that make autonomous maintenance safe. Do not fork them
away without replacing them with something stronger:

- **Verification is real.** The reporter's exact case, on the surface they
  use, in a real environment. A compile is not a test; a cross-build is not a
  run on that platform.
- **A check that was not run is listed as not run.** Never omitted, never
  implied.
- **A fix reaches every sibling.** Grep for the same pattern before shipping.
- **Merge only the reviewed head.** A push during review invalidates it.
- **A tag waits for a green CI run on that commit.** A cancelled run is not a
  pass.
- **Defaults never override what the user set.** Read every place the setting
  is read before writing it.
- **A failed read, parse or hash means "unknown", never "empty".**
- **Deliberate decisions are not reversed by a drive-by change.**
- **Community work gets a harm check before a quality review.**
- **The agent proposes a block; the maintainer decides.**

## What not to copy from magpie

This base intentionally leaves out:

- product-specific architecture, provider tables and built-in migrations;
- Go, GUI, i18n and release-script details — they live in optional addenda
  under `docs/standards/` as examples, not as universal rules;
- the release fan-out to a second repository, which is a magpie deployment
  choice;
- the exact loop prompts, which were never published. The prompts here are
  reconstructed from public descriptions and generalized; they are **reference
  designs, not runtime prompts** — adapt each one to the project's commands,
  surfaces and checkpoint store.

See [`docs/extraction-map.md`](docs/extraction-map.md) for the exact mapping
and what was generalized, kept, or dropped.