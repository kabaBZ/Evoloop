# Extraction map: magpie → this base

This file records what was taken from
[yetone/magpie](https://github.com/yetone/magpie), what was generalized, and
what was deliberately left out. It is an audit trail, not a dependency: this
repository does not track magpie, and nothing here needs magpie to work.

Source snapshot: `main` at 797a23c, 2026-11-13, read from a full clone and
cross-checked against `raw.githubusercontent.com/yetone/magpie/main/`. The
tree had 10,206 paths and 9,686 blobs.

## Files that constrain agents

| magpie source | Size | What it actually does | In this base |
| --- | --- | --- | --- |
| `CLAUDE.md` | 23 B | Two imports: `@AGENTS.md`, `@LESSONS.md` | `CLAUDE.md`, unchanged in shape |
| `AGENTS.md` | 6,124 B | Routing table: which doc to read for which task; a short list of project facts and hard rules | `AGENTS.md`: routing table + ten invariants; product facts moved to placeholders |
| `LESSONS.md` | 75,640 B | Nightly wrong-answer ledger: rule, why, evidence, `Seen N×`; promotion at 3 distinct days; "Done right"; "Moved to the code standards"; criteria-still-missed lists | `LESSONS.md`: same mechanism, empty ledger, entry format, nightly checklist |
| `docs/code-standards.md` | 22,486 B | 9 change rules, 9 test rules, 6 acceptance criteria, review patterns, commit and merge rules | `docs/code-standards.md`: same rule classes, language-neutral wording, addenda pointers |
| `docs/contributors.md` | 6,366 B | Harm check before review; close-without-review list; credit rules; five block criteria (agent proposes, maintainer decides); blocked/extra-care/watched tables | `docs/contributors.md`: same structure, vendors and paths generalized |
| `docs/subsystems/README.md` | 4,526 B | What a subsystem reference contains; when to update it; how to describe a change; four review steps | `docs/subsystems/README.md`, plus `_template.md` |
| `docs/subsystems/*.md` (17 pages) | — | Project-specific design references (routing, plugins, memory, GUI, sync…) | Not copied. `_template.md` captures the shape; each fork writes its own |
| `.github/workflows/test.yml` | 7,220 B | 3-OS matrix; gofmt on Linux only; vet; full suite with a 30 m timeout; race shards; Windows compiles tests but runs only native checks; `concurrency` so main runs finish and PR runs cancel | Rules lifted into `code-standards.md` (acceptance 3), `environments.md` (CI parity, Windows), and the loop prompts (one suite at a time) |
| `.github/workflows/release.yml` | 10,247 B | A `v*` tag dispatches a release in a second repo; waits out API rate limits up to 65 min; version format validated | `release.md`: tag-after-green, timeouts for vendor waits, publish ordering, "a tag is not a release". The second-repo fan-out is left out |
| `.github/ui-preview/` + `ui-preview.yml` | 5 files | On a UI PR, builds the PR, seeds a fake home, plans scenes with a cheap model, records screenshots (video when needed) in Chromium with a drawn cursor, refuses to publish if a key shows, publishes to a Pages branch, edits a fixed block in the PR description. Recording job has no write token; publishing job never runs PR code | `docs/standards/gui.md` § "Show the change to a reviewer", generalized |
| `.github/workflows/privacy-filter.yml` | — | Builds the local masking model per platform; a separate `pf-v*` prerelease holds the zips whose hashes the app pins | Not copied. The general rule (a pinned artifact is checked against the vendor and trusted only as far as who can write it) is in `LESSONS.md`'s source material and `code-standards.md` |
| `.github/workflows/notes-mirror.yml` + `docker-*.yml` | — | Copies release notes back when a vendor refused them; daily and stable Docker images | Not copied; product-specific release plumbing |
| `.github/ISSUE_TEMPLATE/bug_report.yml` | 1,245 B | Version, surface, OS, setup, steps, expected/actual, logs | `.github/ISSUE_TEMPLATE/bug_report.md` |
| `.github/ISSUE_TEMPLATE/feature_request.yml` | 295 B | "What you want to do" / "What you do today instead" | `.github/ISSUE_TEMPLATE/feature_request.md` |
| `Makefile` | 171 lines | `test` = vet + full suite; `test-ui`; build and release targets | Placeholders in `bootstrap.md` (`{{TEST_CMD}}`, `{{FULL_TEST_CMD}}`, `{{BUILD_CMD}}`, `{{RELEASE_CMD}}`) |
| `README.md` | — | Product description, install, privacy | Not copied; product-specific |
| `docs/reference.md`, `provider-plugins.md`, others | — | Deep product behavior | Not copied |

## The loop model

The three `/loop` tasks are not in the repository. Their behavior is described
publicly by dotey and wquguru; the prompts themselves were never published,
and yetone's two posts were protected. The prompts in `docs/loops/` are
**reconstructed from those descriptions and generalized**, not extracted:

| Loop | Public description | This base |
| --- | --- | --- |
| 1. GitHub + Discord | Triage issues (fix or decline, reply either way); review PRs line by line and actually run them; fix → release → reply; one canned reply for relay/reseller ads; read every channel, real @ mentions | `1-github-and-community.md` |
| 2. X/Twitter | A cheap decision model filters "is this about the project / is it feedback" before the main model; fix reproducible feedback and reply to the post | `2-social-feedback.md` |
| 3. Nightly | Clean up "shit mountain" and malicious code; write lessons (which PRs shouldn't merge, which verification was thin); propose low-quality contributors for the block list | `3-nightly-review.md` |

What the public descriptions add beyond the repository, and what this base
keeps:

- **One entrance per loop.** A loop owns its queue; it does not act on another
  loop's item.
- **A cheap filter in front of an expensive model.** A product name that is a
  common word otherwise burns the main model on noise.
- **Real reproduction environments on every platform.** Precondition, not a
  nice-to-have.
- **Decisions recorded so the agent can make taste-consistent choices.**
  `docs/decisions.md`.
- **Mistakes written down and read by the next agent.** `LESSONS.md`.
- **Prompts spend their words on edge cases**, not on the happy path.
- **The human keeps UI/UX taste.**

## What was generalized, specifically

| magpie phrasing | This base |
| --- | --- |
| Go, `gofmt`, `go vet`, `go test ./...` | `{{LINT_CMD}}`, `{{TEST_CMD}}`, `{{FULL_TEST_CMD}}`; Go detail kept as an example in `docs/standards/go.md` |
| GUI engines, i18n, WebKitGTK | UI rules generalized; examples in `docs/standards/gui.md` |
| "magpie's default never overrides a value the user set" | "The project's default never overrides a value the user set" |
| Specific vendors, providers, relays, removed vendors | "a vendor the maintainer removed", "a relay or reseller address" |
| `.github/`, Dockerfile, `site/` | "CI configuration, release or publish scripts, dependency resolution, infrastructure manifests, the documentation site" |
| `gh pr merge --match-head-commit` | `{{MERGE_HEAD_LOCKED_CMD}}` |
| 16 release assets, Homebrew tap, notarization | `{{RELEASE_ASSET_COUNT}}`; "the update feed or package index" |
| Magpie's commit/PR history in `LESSONS.md` | Not copied. The base's `LESSONS.md` starts empty; the rules those lessons became are already in `code-standards.md` |

## What was deliberately not extracted

- **Product architecture and provider tables.** Useless outside magpie and
  actively misleading in another project.
- **The 17 subsystem pages.** Their *shape* is reusable; their content is not.
- **The release fan-out to a second repository.** A deployment choice.
- **The exact nightly-review trigger and prompts.** Not public; reconstructed.
- **The block list entries and the named authors.** Personal and
  project-specific; the *criteria* are kept, the names are not.
- **Go/GUI-specific guard tests.** They are excellent examples of "a guard
  lives where the rule is kept", but they are code, and this base is
  documents only.

## What to check if magpie changes

If you want to keep tracking upstream, the files worth re-reading are, in
order: `LESSONS.md` (the newest failure modes), `docs/code-standards.md` (what
got promoted), `docs/contributors.md` (community gates), and `.github/`
workflows (CI lessons encoded as configuration). The loop prompts will not
appear there; they live in the maintainer's tooling.