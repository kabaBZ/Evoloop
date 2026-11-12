# Base changelog

Changes to the base itself, newest first. A fork reads this to know what it is
missing when it merges the base back in. Product changes do not go here.

Versioning: `v0.<major>.<minor>` — major for a change that alters a rule's
meaning, minor for added rules or documents. A fork's merge conflicts are
resolved in the fork's `decisions.md`, not here.

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