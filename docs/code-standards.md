# Code standards

These rules come from what reviews keep asking for. A reviewer checks each one
that applies, and a change that misses one gets a comment saying which. How to
describe and review a change to a subsystem is in
[`subsystems/README.md`](subsystems/README.md).

Commands named here are the placeholders defined in
[`bootstrap.md`](bootstrap.md). Language-specific rules live in optional
addenda under [`standards/`](standards/); this file is language-neutral.

## What a change contains

| Rule | What a reviewer looks at |
| --- | --- |
| **A change does only what its goal needs.** | Every change in behavior has a reason tied to the goal. Unrelated refactors, extra features and policy changes go in a change of their own. |
| **A fix covers exactly the cases it means to.** | Which cases change, and which neighboring ones stay the same. Different errors are not handled as one. |
| **A change holds on the whole path.** | Follow it from the user's input to the end result, through callers, state and side effects. One correct function does not make the operation correct. A value saved under one id but read under another, or a button that updates something other than the binary that runs, is the classic failure. |
| **A fix reaches every sibling of the bug.** | Search for the same pattern: every agent, command, guard of the same shape and caller of a changed predicate gets the fix in the same change. Include the other branches and the other messages that name the same endpoint or rule. A fix that patched only the case in the screenshot came back four times. |
| **A change re-derives everything built on what it changes.** | When a change alters a formula, a list, a layout breakpoint or the timing of an async step, find every caller, saved copy and test that assumed the old meaning, and run it. A stale clamp under a new limit, or a breakpoint move that leaves an unrelated page red, is this rule missed. |
| **A failed read, parse or hash means "unknown".** | It never means "empty", "none" or "equal". Keep what was read before, copy aside a file that does not parse before writing over it, and fix the shared reader rather than writing a strict one beside it. A list that guards access (an allow list, a key limit) fails closed when its file cannot be read. Test with the file unreadable. |
| **Another system's values come from that system.** | When the project writes another application's file or speaks for it, take each field's type and value from that application's own data, binary, merge code or documentation — not only its field names. A timestamp written in the wrong unit or a guessed enum value is this rule missed. |
| **Nothing that can block runs under a lock its reader needs.** | Never send on a channel or call anything that can block while holding a lock the receiver takes first. Set shared state before the handoff that lets another thread look at it. A struct copied by value that holds a map is not a copy. Any change that starts a thread or timer runs the race detector before it is pushed. |
| **The project's default never overrides a value the user set.** | Before writing a key, list everywhere the setting is read from, and test with a user-set value present. A migration that rewrites a setting names the users who set it themselves, and leaves their value alone or says in the notes why not. |
| **A size budget is met, not raised.** | A change that reaches a budget moves something out first: to a plugin, a lazily loaded file, or code it replaces. A raise names what the owner agreed to, not "nothing left to move". The budget counts what it is about: code, not comments. Re-run the budget after a rebase. |
| **Checks cover the edges the change touches.** | For numbers, check boundaries. For paths, check each platform and both link-following and raw comparison. For concurrency, check races and stale state. Pick checks by risk. |

## Tests

| Rule | What a reviewer looks at |
| --- | --- |
| **A fix comes with a test that fails without it.** | Put the old code back and run the new test. It must fail with a real failure that names the behavior. A compile error or a skipped test does not count. The pull request says what it failed with. |
| **A test never reaches the network or a live service.** | A vendor's page or API is served from a local test server built from bytes saved from the real one. A package comes from a lock file, not a registry's latest. Break the change and watch where the test's requests go. |
| **Tests use real inputs.** | Fixtures look like real requests, real serialized output and real files on disk. Include missing fields, empty values and defaults. Build fixtures from the reporter's literal bytes, not a similar sample you made up. |
| **Tests check behavior, not implementation.** | Assert what the user or caller needs. Formatting changes and internal refactors should not break a test, unless exact bytes are the requirement. |
| **A test that fails on the main branch is a bug to fix now.** | "Fails the same on main" is not a baseline, and neither is "fails under the full run, passes alone". Fix it at its cause, or open an issue naming the cause, in a change of its own, before shipping on top of it. A red under load is a timing assumption a user will meet on a busy machine. |
| **A red's issue is read to its last comment, and a patch on it is taken first.** | Before filing, commenting on or shipping past a red, search the test's name and read every comment. "Seen again" is not work on it: take it, or say who has it. A patch posted there is reviewed and ported before anyone writes another. |
| **Tests stay out of the real machine.** | Run them under a temporary home directory with caches pinned (see [`environments.md`](environments.md)). Never touch a real agent config, a real credential store or the user's project config. |
| **Whatever a test starts ends when the test does.** | A fake process, a thread or an async refresh a test sets going is stopped or waited for before the test returns, or it writes into the next test's folder or spins for days. A package that isolates itself in its own setup instead of the shared helper sets everything the helper sets. |
| **A timing test leaves room on both sides.** | Never bound a test by wall-clock time a loaded runner can exceed, and keep a threshold classifier's fixture far from every threshold it has. Reproduce a timing failure by injecting the delay (a held write, a late first arrival), not by rerunning until it fails. List every timing assumption when fixing one. |
| **A test waits for the state it checks, not for an earlier step of the same start.** | Before waiting on a "ready" sign, read the code that sets it and check that what you assert is done before that sign, not after it. |

## Acceptance criteria

A change is merged or released only when it shows each of the following. Say
which ones were run and what they printed. **A check that wasn't run is listed
as not run, never left out.**

1. **A test that fails without the change.** Put the old code back and run the
   new test. It must fail with a real `FAIL` that names the behavior. Say which
   test fails and what it failed with. Then restore the change and check the
   test passes.
2. **Tests in a sandbox.** Run the suite under a temporary home with caches
   pinned, per [`environments.md`](environments.md). No test reads or writes a
   live application config or the user's project config. Remove the temporary
   home afterwards, including read-only caches.
3. **The build and the full suite.** Run the formatter check on changed files,
   the static analysis, the build for every supported platform, and the whole
   suite — not the packages you think you touched. Guard tests live in
   packages a change doesn't touch. In a chain of commands, stop on the first
   failure. A cross-OS build does not compile the tests; the static analysis
   does. After a rebase, run the suite again at the pushed head.
4. **UI tests in every engine and language** (if the project has a UI). A
   change to UI code runs the UI tests it touches in every supported engine.
   The suite must pass in every shipped locale, with matching placeholders.
   Run the whole suite when a change adds an API the UI calls, a selector or
   class other pages share, a platform feature an older engine lacks, or a
   layout breakpoint.
5. **Real use where possible.** A change to an integration, provider,
   subscription or client is also checked against the real thing: a real
   account or key, the real config format, the vendor's real reply. Do this in
   a sandbox home with copies of the credentials. Never touch the live service
   or a running instance. If a real check isn't possible, say so. **What wasn't
   tried with the real thing isn't built on and isn't told to users as
   working.** A platform report is checked on that platform; a cross-build is
   not a test.
6. **The project's own UI and interaction rules** hold. See
   [`standards/gui.md`](standards/gui.md) when present.

A change is merged only after a maintainer (or the loop's reviewer) has pulled
it, merged it onto current `main` and run the checks above on the result. The
merge names the commit that was reviewed:
`{{MERGE_HEAD_LOCKED_CMD}}`. A push made during the review then stops the
merge, rather than shipping code nobody ran. A head pushed after the review is
reviewed again, and the record names the new head. Write on the pull request
what was run, at which head, and then merge: a merge with no record of what
was run cannot be checked afterwards. A draft pull request cannot be merged
until it is marked ready.

A release counts only when all of these hold:

- A full CI run on the tagged commit, or a later one containing it, finished
  green. A run cancelled by the next push is not a pass.
- The tag is on `main`, an ancestor of `origin/main`.
- The release has every expected asset.
- The release is marked Latest, the highest version.

Check `git tag` for the next free version first, because releases can be made
from more than one place. Fetch tags and check the commit isn't tagged
already. **A tag is not a release:** check the release exists with its assets
before naming the version to anyone, and file a release whose build failed
instead of letting the next tag cover it. Publish in tag order, or pass the
"latest" flag only to the newest version; a later publish of an older version
takes Latest with it. See [`release.md`](release.md).

## Docs-only changes

A change that touches only documentation — `README*`, `docs/`, `LESSONS.md`,
`AGENTS.md` or a template — has its own acceptance. Build and test commands
are not evidence for it.

1. **Links resolve.** Every relative link points at an existing file, and
   every `#anchor` names a heading that exists. Check the Markdown links in
   the whole change, not only the new ones.
2. **Placeholders are consistent.** Every `{{TOKEN}}` used is defined in
   [`bootstrap.md`](bootstrap.md), and a token the change introduces is added
   to that table.
3. **One rule, one canonical statement.** A rule lives in one owning document;
   other documents link to it or restate it inside a prompt. A change that
   edits the rule edits every restatement in the same commit.
4. **No stale reference.** A removed file, section or command is removed from
   every document that named it. Search for its name before shipping.
5. **Tables and fences render.** A table row has the same number of cells as
   its header, every fence is closed, and no line breaks the table in the
   repository's renderer.
6. **A new file is reachable.** It is linked from at least one document a
   reader starts from; an orphan file is invisible.

Say in the PR which checks were run and what they printed. A docs change that
was not link-checked is not verified.

## Reviewing a pull request

These patterns come from recent reviews. A reviewer checks each one that
applies:

- **Run it.** Pull the head, merge it onto current `main` (resolving conflicts
  if needed) and run the acceptance checks. Reading the diff alone isn't a
  review.
- **A high-risk change gets a second read from a context that did not write
  it.** Security fixes, release or migration tooling, data deletion and a P0
  fix get one review from a fresh session or subagent that receives only the
  issue, the diff, these standards and the test output — not the author's
  notes or the conversation that produced the change. It judges the artifact
  only. This is a second layer on top of "Run it", not a replacement: a clean
  context cannot see what was never written down, and a run at the reviewed
  head is still the proof.
- **Break it on purpose.** Revert the fix, or break the code it guards in
  several ways, and check that the new tests catch each break. A reviewer may
  add review tests of their own.
- **Check the source, not the PR's account of it.** Read the upstream source
  for a vendor's behavior. Try the change against a real upstream when
  possible.
- **Check each platform and architecture.** A conversion that is safe on one
  word size can wrap on another. A platform error code may not match the
  portable constant.
- **Follow the whole path.** Check that a pick is saved and read under the
  same id, that a button works on the thing that actually runs, and that
  names and user agents don't collide with another application's.
- **Classify narrowly.** A mark or error class covers only the case it
  describes.
- **A preview writes nothing.** A dry run, plan or preview has no side
  effects.
- **Don't reverse deliberate decisions.** Product decisions recorded in
  [`decisions.md`](decisions.md) are the maintainer's to change. A PR that
  changes one is left for the maintainer.
- **Rule and instruction files are not normal PRs.** A change to `AGENTS.md`,
  `CLAUDE.md`, `LESSONS.md`, `docs/code-standards.md`, `docs/decisions.md` or
  `docs/loops/` is the maintainer's call, even when it looks like a typo fix.
- **Compare UI pass counts with `main`.** A test that fails on `main` too is
  not a baseline (see [Tests](#tests)). A PR that says its failures "fail the
  same on main" gets that checked, and gets them fixed or filed before the
  merge.
- **Docs match the source.** A reference is checked against the code it links,
  and every link must resolve.
- **Keep the PR to its goal.** Unrelated tests and refactors go in a PR of
  their own. Comments name functions, not line numbers. Fixture names match
  what they stand for.
- **Check each citation.** A cited issue or commit is opened and read; a
  citation that names the wrong thing is a finding.

## Commits

A commit message names the area it changes, then says what the user now sees,
written as plain behavior:

```
<area>: <what changed, in user-visible behavior>
```

Give the issue number or the reporter. The body says how it was verified, and
which test fails without the change.

- **The record of what was run is in the commit body**, where a later review
  can read it: not in push notes, a chat, or only on the pull request. A large
  commit with no body cannot be reviewed afterwards. A pointer to "the push"
  is not a record.
- **One commit has one goal.** Two reports fixed together are two commits,
  each with its own record, so a revert takes back one and keeps the other.
  A fix found on the way goes in a commit of its own.
- **A commit's file list is checked against its goal before pushing.** Run
  `git show --stat HEAD` after the rebase. A deleted file, or a hunk in a
  package the goal never names, means the tree is stale: rebuild the change on
  `origin/main`, don't push the old tree. Deletions in a file you meant to
  create are someone else's tests.
- **A checkout, worktree and its logs belong to the session that made them.**
  Work in a worktree of your own, never check out, commit from or clean
  another session's, and after committing check that `git show --stat HEAD`
  lists your files and only yours.
- **A commit saved early stays local until it is finished.** "wip" never
  reaches `main`. Before pushing, `git log origin/main..HEAD --format=%s` names
  no "wip", "TODO" or template placeholder.
- **A commit message file is made fresh for this commit, and its subject is
  read back before the push.** A stale message file once shipped one change
  under another change's subject.
- **A squash merge's message is written, not the platform's default.** Give
  the squash a subject and a body holding the change and what was run at the
  reviewed head. Check that `git log -1 --format=%B | wc -c` is a few KB at
  most. The subject is one short line; the rest goes in the body.

## A definition of done

A change is ready when a reviewer can answer all of these from the pull
request and the commit:

- What user-visible behavior changed, and what did not?
- Which test fails without the change, and what did it print?
- What was run at the pushed head, and what was not run?
- Which siblings were searched for and either fixed or named as separate?
- Which platform, surface and real service were exercised?
- Which subsystem reference was updated, and do its links resolve?
- If it is reverted, what breaks, and is a release needed?