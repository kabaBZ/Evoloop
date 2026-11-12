# Bootstrap

Everything here is done once, when this base becomes a real project. Nothing
in the base works until the placeholders are filled: an agent that reads
`{{TEST_CMD}}` will guess, and a guess is how a "verified" change ships
unverified.

## 1. Fill the placeholders

Every `{{TOKEN}}` in the repository must be replaced. `grep -rn '{{' .` lists
them. The table is the contract; if a token is not listed here, it is a bug in
the base.

| Token | Meaning | Example |
| --- | --- | --- |
| `{{PROJECT_NAME}}` | The product's name, as users see it | `Acme` |
| `{{ONE_LINE_PURPOSE}}` | One sentence: what it does, for whom | `One local gateway for every coding agent` |
| `{{LANGUAGE_AND_TOOLCHAIN}}` | Primary language, version, package manager, build tool | `Go 1.24, modules, make` |
| `{{PLATFORMS}}` | Platforms you actually ship and can run | `macOS 15+, Windows 11, Ubuntu 24.04` |
| `{{SURFACES}}` | User-facing surfaces | `GUI, TUI, CLI, HTTP API` |
| `{{LAYOUT}}` | Entry points an agent must know | `cmd/`, `internal/`, `ui/` |
| `{{MAIN_BRANCH}}` | The branch releases come from | `main` |
| `{{COMMUNITY_CHANNELS}}` | Where the project talks to users | `GitHub Discussions, Discord #support` |
| `{{SOCIAL_HANDLES}}` | Accounts and search terms loop 2 watches | `@acme`, `acme.app` |
| `{{CI_PROVIDER}}` | Where CI runs | `GitHub Actions` |
| `{{LINT_CMD}}` | Format and static analysis, one command | `make lint` |
| `{{TEST_CMD}}` | Fast tests for the packages a change touches | `make test` |
| `{{FULL_TEST_CMD}}` | The whole suite, as CI runs it | `make test-all` |
| `{{BUILD_CMD}}` | Build every shipped platform | `make build-all` |
| `{{RELEASE_CMD}}` | Cut a release from a green tag | `make release` |
| `{{TAG_PREFIX}}` | Version tag format | `v` |
| `{{VERSION_CHECK_CMD}}` | Confirm a version exists and is published | `gh release view v1.2.3` |
| `{{MERGE_HEAD_LOCKED_CMD}}` | Merge that refuses if the head moved | `gh pr merge --squash --match-head-commit <sha>` |
| `{{RELEASE_ASSET_COUNT}}` | Assets every release must have | `16` |
| `{{SANDBOX_HOME_RULE}}` | How tests get a temporary home | `HOME=$TMPDIR, XDG_* pinned` |
| `{{PRIVATE_SECURITY_CHANNEL}}` | Where security reports go | `security@acme.example` |
| `{{CANNED_PARTNERSHIP_REPLY}}` | One reply for relay/reseller ads | `Thanks for the interest — we don't add third-party relays or resellers; a partnership policy may come later.` |
| `{{MAX_REPLIES_PER_TICK}}` | Public reply cap for loop 2 | `5` |

Also fill the index in [`subsystems/README.md`](subsystems/README.md) and the
"Project-specific facts" block in [`../AGENTS.md`](../AGENTS.md).

## 2. Write the three documents only a human can write

- [`project-brief.md`](project-brief.md): what the project is, who it is for,
  what it refuses to do, the quality bar, the support boundary.
- [`decisions.md`](decisions.md): decisions already made, with the why. Every
  "we discussed this and chose X" that an agent could otherwise re-litigate.
- [`environments.md`](environments.md): the machines and sandboxes where the
  agent can reproduce real reports. If this is empty, loop 1 and loop 2 will
  "fix" things they cannot verify.

A useful test: give the three documents to an agent that has never seen the
project and ask it to decide five borderline cases. If its answers surprise
you, the documents are incomplete.

## 3. Set the repository up

- Make this repository a **template repository**, so forks start from it.
- Protect `{{MAIN_BRANCH}}`: no force-push, no deletion, required CI.
- Add the secrets the loops need (a token that can comment, merge and release;
  the community bot; the social API). Scope each token to what its loop does.
  Never give loop 2 a merge token.
- Turn on branch protection for the release tag if the platform supports it.
- Add the issue templates and the PR template from `.github/`.
- Create the environments: at least one machine per platform in
  `{{PLATFORMS}}`, plus a spare for a full suite so two loops don't run one at
  a time.
- Seed the agent's local memory with pointers, not copies: one entry per
  entry point (brief, decisions, code standards, subsystem index, release,
  environments, test command). The procedure is in
  [`agents-memory.md`](agents-memory.md#memory-and-forks).

## 4. Seed the lessons

`LESSONS.md` starts empty on purpose. Do not paste another project's history
into it. The base's [`code-standards.md`](code-standards.md) already carries
the rules those lessons were promoted into; that is the inheritance.

Before the first loop runs, add one local entry only if you have already paid
for it: a mistake this project made, with its evidence. Otherwise leave the
file empty and let the first nightly review write.

## 5. Start the loops in order

1. **Week 1 — loop 1, triage only.** It may reply and label, not merge and not
   release. Read every report. Fix the prompt, not the agent.
2. **Week 2 — loop 1, full.** Add fix, review and release. Watch the rework
   rate: a change that needs a second fix in the same week is a lesson, not an
   accident.
3. **Week 3 — loop 3.** Turn on the nightly review. It will mostly write
   lessons at first. That is the point.
4. **Week 4 — loop 2.** Add social only when the release path is boring. A
   social loop that promises fixes the project cannot ship is worse than
   silence.

Add the second loop only after the first has a week with no release tagged
before green, no merge of an unreviewed head, and no lesson repeated twice.

## 6. Verify the base itself

Before trusting it, run these checks by hand:

- `grep -rn '{{' .` returns nothing outside this table. This table is the
  only place the tokens stay, so it can be used again when the base is
  updated.
- Every link in every document resolves.
- `CLAUDE.md` imports only `AGENTS.md` and `LESSONS.md`, and both exist.
- Each loop prompt names its checkpoint, its gates and its report.
- `LESSONS.md`'s promotion rule is stated in exactly one place and referenced
  from the others.
- A new agent, given only `CLAUDE.md`, can answer: where do I read before a
  code change, how do I verify, what may I merge, and what do I never do.

## 7. Keep forks in sync

This base is meant to be forked, improved, and merged back. Treat the base as
a product:

- Universal rules go upstream: a lesson that would apply to any
  agent-maintained project belongs in this base, not only in the fork.
- Product specifics stay in the fork. If a rule names one vendor, one file or
  one language, it belongs in that fork or in an addendum under
  `docs/standards/`.
- Changes to the base get a version and a short changelog, so a fork knows
  what it is missing.
- A fork that changes a base rule says why in its `decisions.md`. A silent
  divergence becomes a bug the next time the fork merges the base.

The test for "does this belong in the base": would a project in another
language, shipping another kind of product, still need this sentence? If yes,
it is base material. If it only makes sense here, it is not.