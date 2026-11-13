# Community PRs: what is closed unreviewed, who is credited, and who is blocked

{{PROJECT_NAME}} merges community PRs through agents. Two things keep that
from wearing the project down: a PR that could harm users is caught before any
review of what it does, and an author who keeps sending work that costs more
to review than it gives is stopped.

## Look for harm first

Before running a PR, read its diff for anything that reaches past its stated
goal. Any one of these **stops the review** until the maintainer has looked:

- It changes an agent instruction or rule file: `AGENTS.md`, `CLAUDE.md`,
  `LESSONS.md`, `docs/code-standards.md`, `docs/decisions.md`, `docs/loops/`,
  or a template under `.github/`. These are the maintainer's; the change stops
  the review like any other harm hit.
- It changes CI configuration, release or publish scripts, the dependency
  manifest's resolution rules, infrastructure manifests, or the documentation
  site.
- It adds a network address, an upload, telemetry, or sends a credential,
  token, cookie or file anywhere it didn't go before.
- It reads or writes a credential file, keychain entry, environment file or
  application config the goal doesn't need.
- It adds a dependency, a dependency override, an install script, a binary, a
  minified or encoded blob, or code built from a string at run time.
- It adds a relay, reseller, affiliate address or advertisement, or brings
  back a vendor the maintainer removed.
- It weakens an existing guard: turns off a check, broadens an allow list,
  silences an error, or adds a bypass.

Say which line stopped it in the review. Don't run the PR's code on a machine
with real accounts or credentials until the maintainer has cleared it.

## Close without a full review

Close with a short reason, without running it, a PR that:

- changes what the maintainer has ruled on (see "Don't reverse deliberate
  decisions" in [`code-standards.md`](code-standards.md)), including files
  the maintainer keeps byte for byte;
- changes an agent instruction or rule file (`AGENTS.md`, `CLAUDE.md`,
  `LESSONS.md`, `docs/code-standards.md`, `docs/decisions.md`, `docs/loops/`,
  or a `.github/` template) that the maintainer did not ask for;
- is a resubmission of a PR closed for its idea, with nothing new to answer
  the reason it was closed;
- is one of several near-identical PRs opened seconds apart;
- is a drive-by layout or product-behavior change with no issue asking for it.
  The default is no.

## Credit what a reporter gave

A fix branch on the reporter's fork is merged with their commits as they are,
authored by them. The harm check above applies to it first. What has to change
goes in a commit of our own on top, and the issue says what changed and why.

A fix that uses the reporter's patch, test fixture, request body, error text or
root cause carries a co-author trailer for them. Use their noreply address
when no other address is known. A report that only says something is broken is
named in the body (`#<n>, <login>`), with no trailer.

## Blocking an author

An author is proposed for the block list when any one of these holds:

1. They have 3 or more PRs closed by a maintainer as low-value (wrong,
   untested, unrelated, machine-made filler), and fewer than 30% of their PRs
   merged.
2. They resubmitted a PR after an explicit rejection, twice.
3. They did anything deceptive: hid a change outside the PR's stated goal,
   faked a test or a verification, or anything from "Look for harm first" done
   on purpose.
4. They open PRs in machine bursts (several within a minute) that are
   low-value as in 1.
5. They re-add a removed vendor, a relay or an advertisement after being told
   no.

**An agent only proposes.** It writes the evidence (PR numbers, dates, what was
wrong) to the maintainer; the maintainer decides. A blocked author's new PRs
and issues are closed with: "Thanks — this account's contributions aren't
accepted in this repository." Nothing more is argued.

An author being watched (one or two low-value PRs, no pattern yet) gets a
normal review; the reviewer notes the PR under "Watched" if it is low-value
too.

## Block-list proposals

An agent that believes an author meets a rule above writes a proposal here.
It never blocks, closes an account or posts the block message itself. A
proposal is evidence, not a verdict; the maintainer fills the last column.

| Date | Author | Rule | Evidence | Decision |
| --- | --- | --- | --- | --- |
| *(none yet)* | | | PR/issue links, dates, what was wrong | *(maintainer)* |

This table is also the **evidence log**. A relay, reseller or affiliate ad
gets a row with "ad" in the Rule column; an author being watched gets a row
with "watch". A row with no rule and no link is a note, not a proposal. Loop 3
reads this section; loop 1 and loop 2 append to it.

## Lists

Blocked: *(none yet)*

### Reviewed with extra care

The maintainer chose not to block these authors, so their PRs are still
reviewed, but more strictly than others:

- Read every line of the diff, not only the part the description talks about.
  Compare the files it touches with its stated goal; anything outside the goal
  is a reason to close it.
- Run "Look for harm first" in full, even on a PR that looks like a typo fix.
- Run it yourself on current `main`; don't rely on the PR's claims or its CI.
  Never merge it in the same tick it was opened, and use the head-locked merge.
- A PR that repeats one already closed is closed with a link to the first.

| Author | Evidence |
| --- | --- |
| *(none yet)* | |

### Watched

Work that is useful but keeps coming back for the same gaps. Not a block and
not extra care: the review checks the named gaps before merging.

| Author | Check | Evidence |
| --- | --- | --- |
| *(none yet)* | | |