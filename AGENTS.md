# Notes for coding agents

> **This repository is a template.** The double-brace placeholders (such as
> `PROJECT_NAME`) are intentional. Fill
> them by following [`docs/bootstrap.md`](docs/bootstrap.md); never replace one
> as a drive-by "fix", and never delete the paragraph that explains them. A
> fork also replaces `README.md` and `README.zh-CN.md` with the product's
> README; if they still describe the base, bootstrap is not finished.

This file is a **routing table**. It holds no rules of its own except the
invariants at the bottom. Before doing a kind of work, read the document
that owns it.

## Read before you act

| Before you… | Read |
| --- | --- |
| Change any code | [`LESSONS.md`](LESSONS.md), then [`docs/code-standards.md`](docs/code-standards.md) |
| Change a documented subsystem | [`docs/subsystems/README.md`](docs/subsystems/README.md), then the subsystem's page |
| Review, close or merge a community PR | [`docs/contributors.md`](docs/contributors.md) — the harm check comes first |
| Tag or release | [`docs/release.md`](docs/release.md) and acceptance criteria in the code standards |
| Choose a default, a behavior, a layout or a dependency | [`docs/decisions.md`](docs/decisions.md) and [`docs/project-brief.md`](docs/project-brief.md) |
| Reproduce a user's report | [`docs/environments.md`](docs/environments.md) |
| Open or run an autonomous loop | [`docs/loops/README.md`](docs/loops/README.md) |
| Set up a fork, fill placeholders, or start the loops | [`docs/bootstrap.md`](docs/bootstrap.md) |
| See what changed in the base itself | [`docs/BASE-CHANGELOG.md`](docs/BASE-CHANGELOG.md) |
| Record a lesson, promote a rule, or run the nightly review | [`LESSONS.md`](LESSONS.md) |

## What this repository is

{{PROJECT_NAME}} is written, reviewed, merged and released by agents. A human
maintains direction, taste and the gates listed in
[`docs/decisions.md`](docs/decisions.md). The project's purpose and non-goals
are in [`docs/project-brief.md`](docs/project-brief.md).

The build, test and release commands are in
[`docs/bootstrap.md`](docs/bootstrap.md); do not guess them.

## Hard invariants

These override convenience. A change that breaks one is not merged, no matter
how green it looks.

1. **Read `LESSONS.md` before changing code.** Every lesson there was paid for
   by a real regression. If a rule applies, say in the PR which one and how
   the change satisfies it.
2. **"Fixed" means reproduced and verified on the reporter's surface.** Their
   OS, their build, their agent or client, their data. Not a nearby case, not
   a unit test alone.
3. **A check that was not run is listed as not run.** Never omitted, never
   described as passing.
4. **A fix reaches every sibling of the bug.** Search for the same pattern
   across the repository before shipping.
5. **Merge only the head that was reviewed.** Name the reviewed commit and
   what was run in the merge record. Use the project's head-locked merge.
6. **A tag waits for a green full CI run on that commit.** A run cancelled by
   a later push is not a pass.
7. **Deliberate decisions are not reversed.** If it is written in
   `docs/decisions.md` or was asked for by the maintainer, a drive-by PR does
   not change it.
8. **A community PR gets the harm check before any quality review.** A hit
   stops the review until the maintainer clears it.
9. **Never expose, copy or transmit a credential.** No new network address, no
   upload, no telemetry, no credential file access outside the goal.
10. **When a lesson has been seen on three separate days, promote it.** Move
    it into `docs/code-standards.md` in the same nightly review, and leave a
    link in `LESSONS.md`.

## Project-specific facts

Keep this section short and factual: the names and paths an agent must know
that are not derivable from the tree. Everything longer belongs in a
subsystem page.

- Primary language and toolchain: {{LANGUAGE_AND_TOOLCHAIN}}
- Supported platforms: {{PLATFORMS}}
- User-facing surfaces: {{SURFACES}}
- Repository layout entry points: {{LAYOUT}}
- Community channels: {{COMMUNITY_CHANNELS}}