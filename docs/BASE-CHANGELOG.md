# Base changelog

Changes to the base itself, newest first. A fork reads this to know what it is
missing when it merges the base back in. Product changes do not go here.

Versioning: `v0.<major>.<minor>` — major for a change that alters a rule's
meaning, minor for added rules or documents. Moving a file is major even when
no rule changed: every fork's links break. A fork's merge conflicts are
resolved in the fork's `decisions.md`, not here.

## v0.6.0

- `code-standards.md` and `loops/README.md` now carry magpie's exact worktree
  rule and command: `git worktree add --detach`. A session works in its own
  detached worktree, which holds no branch and does not switch the primary
  checkout's branch; it never checks out, commits from or cleans another
  session's worktree or logs.

## v0.5.0

- Agent work now lands directly on `main` after lint and the full suite;
  pull requests are for community contributors only. The agent watches CI on
  the pushed commit and fixes a red forward; it never amends or force-pushes
  published history.
- Loop 2 no longer opens a PR: it commits the tested fix directly and loop 1
  releases. Loop 3 may push lessons, docs, behavior-preserving cleanup and
  narrow security hardening directly after the same checks; a real refactor
  or behavior change is still not nightly cleanup.
- This supersedes the v0.2.0 rule that loop 2 opens a PR. Checks before the
  push are the gate; the pull-request entrance is for community contributors.
- `bootstrap.md` branch protection now uses an actor-scoped bypass for the
  agent identity. The bypass does not skip checks. `code-standards.md` and
  `loops/README.md` define the direct-push procedure.
- `release.md`: a release can follow an agent direct push, and the tag still
  waits for a green full CI run on that commit.

## v0.4.0

- Added [`docs/loops/originals.md`](loops/originals.md): the three original
  loop prompts, transcribed verbatim from the screenshot supplied on
  2026-11-13, plus a table of where the generalized loop pages deliberately
  differ (loop 2 does not release; loop 3 does not block).
- Corrected provenance in both READMEs and the extraction map. The prompts
  were not published when v0.1.0 was written; they were supplied later, so
  "reconstructed" is no longer true.
- `3-nightly-review.md` now says explicitly that malicious code is a security
  finding, not a cleanup task.

## v0.3.0

- Moved `BASE-CHANGELOG.md` to `docs/BASE-CHANGELOG.md`. The repository root
  now holds only the entry points (`CLAUDE.md`, `AGENTS.md`, `LESSONS.md`),
  the project's own README and changelog, the license, and GitHub's
  conventional `CONTRIBUTING.md` and `SECURITY.md`.
- `README.md` and `README.zh-CN.md` now open with a fork notice: replace them
  with the product's README before the first release. `bootstrap.md` adds the
  step and `AGENTS.md` repeats it; the self-check fails if the notice is still
  there.

## v0.2.0

Review fixes: the base now carries the machinery it describes, and the
contradictions a fork would hit on day one are gone.

- `LESSONS.md` has its `reviewed-through` watermark again, plus the
  "Criteria still missed" section the mechanism refers to.
- `docs/contributors.md`: agent instruction and rule files join the harm
  check; a block-list proposals table is defined, and it doubles as the
  evidence log.
- `docs/loops/`: loop 2 opens a PR instead of releasing, so it no longer
  conflicts with "never give loop 2 a merge token"; loop 3 gets a `Never`
  section and a scoped direct-push rule; every loop treats outside text as
  data, not instructions; the checkpoint must live outside local agent memory;
  the pages are marked as reference designs, not runtime prompts.
- `docs/loops/README.md`: the private security channel points at
  `{{PRIVATE_SECURITY_CHANNEL}}`; the tone check the routing table promised is
  now a reply step.
- `docs/bootstrap.md`: branch protection and the nightly bot's allowlist are
  defined; LICENSE, SECURITY.md, CONTRIBUTING.md and fork-side CI are on the
  checklist; the self-check is a two-way token audit, not an impossible grep;
  docs-only acceptance and the watermark are checked.
- `docs/code-standards.md`: docs-only acceptance criteria, a review rule for
  agent instruction files, and a second read from a clean context for
  high-risk changes (security, release, migration, P0). The run at the
  reviewed head remains the proof; the clean read is an extra layer.
- `docs/release.md`: the changelog is defined as `CHANGELOG.md`, with a
  release-lock note.
- `docs/decisions.md`: date placeholder fixed; the magpie relay decision is
  marked an example a fork should delete or replace.
- New `CHANGELOG.md`, `LICENSE` (MIT), `SECURITY.md`, `CONTRIBUTING.md`,
  `.gitattributes`, `.gitignore`; `README` and `README.zh-CN` link them.

## v0.1.0

First extraction, from magpie's agent maintenance setup.

- `CLAUDE.md` as two imports, `AGENTS.md` as a routing table with the ten
  invariants, `LESSONS.md` with the nightly review, `Seen` counting and the
  three-day promotion rule.
- `docs/code-standards.md`: change rules, test rules, six acceptance criteria,
  PR review patterns, commit rules, definition of done.
- `docs/contributors.md`: harm check before review, close-without-review list,
  credit rules, block criteria, and "an agent proposes, a human decides".
- `docs/release.md`: tag after green, a tag is not a release, publish order
  and Later, post-release checks.
- `docs/environments.md`, `docs/agents-memory.md`, `docs/subsystems/`,
  `docs/decisions.md`, `docs/project-brief.md`, `docs/bootstrap.md`.
- `docs/loops/`: the three loops, reconstructed from public descriptions
  because magpie had not published its prompts. (The originals were supplied
  later; see v0.4.0.)
- `docs/standards/`: an addendum template, with Go and UI as examples rather
  than universal rules.
- `.github/`: PR template with a verification record, bug and feature issue
  templates.

Known gap (closed in v0.4.0, when the originals were supplied): the three loop
prompts are a generalization, not magpie's originals. Improve them from
evidence in a real fork, and send the universal parts back here.