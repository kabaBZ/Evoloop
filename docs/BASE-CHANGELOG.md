# Base changelog

Changes to the base itself, newest first. A fork reads this to know what it is
missing when it merges the base back in. Product changes do not go here.

Versioning: `v0.<major>.<minor>` — major for a change that alters a rule's
meaning, minor for added rules or documents. Moving a file is major even when
no rule changed: every fork's links break. A fork's merge conflicts are
resolved in the fork's `decisions.md`, not here.

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
  because magpie has not published its prompts.
- `docs/standards/`: an addendum template, with Go and UI as examples rather
  than universal rules.
- `.github/`: PR template with a verification record, bug and feature issue
  templates.

Known gap: the three loop prompts are a generalization, not magpie's originals.
Improve them from evidence in a real fork, and send the universal parts back
here.