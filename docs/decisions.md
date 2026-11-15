# Decisions

An agent can follow a rule it can read. It cannot infer taste, so the
decisions that would otherwise be re-litigated every week are written here.
Read this before choosing a default, a behavior, a layout, a dependency or a
compatibility policy.

**A recorded decision is not reversed by a drive-by change.** If a request
needs one changed, the agent says which decision it conflicts with and asks
the maintainer. A change that silently reverses one is a review finding, not a
feature.

## What belongs here

- A choice a reasonable person could have made differently, and the reason.
- A behavior that looks like a bug but is deliberate.
- A compatibility promise, and how long it lasts.
- A taste decision: visual language, motion, density, copy tone.
- A security or privacy boundary.
- A "we tried X and it failed" that would otherwise be retried.

## What does not

- How the code works. That is a subsystem reference.
- What a command does. That is the README.
- A rule that applies to every change. That is
  [`code-standards.md`](code-standards.md).
- A task in progress. That is an issue.

## Format

```markdown
### <Short title, as a sentence>

- **Decision:** <what is true now.>
- **Why:** <the reason, including the constraint that forced it.>
- **Alternatives:** <what was considered and why it lost.>
- **Do not reverse:** <what a future change must not do.>
- **Owner:** <who decided.>
- **Date:** <YYYY-MM-DD>
- **Supersedes:** <link, if any.>
```

Keep entries in the order they were decided. When a decision changes, add a
new entry and mark the old one `Superseded by <link>`; do not rewrite history.

Entries marked **Example** are illustrations taken from the base's source
project. Delete or replace them in a fork; they are not decisions this project
made.

## Product

### Example — the project does not route user traffic through its own servers

> Example from magpie. Keep it only if the fork makes the same promise;
> otherwise delete the entry.

- **Decision:** Every request goes from the user's machine to the vendor, or
  to a server the user runs. The project ships no relay.
- **Why:** A relay would make the maintainer responsible for user traffic,
  keys and availability, and would turn a local tool into a service with a
  different legal and security surface.
- **Alternatives:** A hosted proxy (rejected: cost, liability); a paid relay
  (rejected: it changes who the user trusts).
- **Do not reverse:** Do not add a default endpoint that is not the user's.
  Do not add a reseller or affiliate address.
- **Owner:** maintainer.
- **Date:** <YYYY-MM-DD>

### A feature that cannot be undone does not ship without a way back

- **Decision:** A destructive or hard-to-reverse change needs an export, a
  backup, or a documented rollback before it reaches users.
- **Why:** Agents ship fast; a user cannot be asked to debug a migration that
  removed their data.
- **Alternatives:** "Document it in the release notes" (rejected: that is not
  a way back).
- **Do not reverse:** Do not remove the last way back in the same change that
  adds the new path.
- **Owner:** maintainer.
- **Date:** <YYYY-MM-DD>

## Defaults

### The project's default never overrides a value the user set

- **Decision:** A migration, a preset or a new version may fill an unset
  value. It never rewrites a value the user chose.
- **Why:** Users cannot tell a default from their own setting after the fact,
  and a silent override is indistinguishable from data loss.
- **Alternatives:** "Rewrite and tell them in the changelog" (rejected: it
  still overrides).
- **Do not reverse:** Do not add a bulk rewrite of a setting without naming
  the users who set it themselves and what they lose.
- **Owner:** maintainer.
- **Date:** <YYYY-MM-DD>

### A failed read means "unknown", never "empty"

- **Decision:** When a file, a parse or a hash fails, the value is unknown.
  Guards fail closed; displays say "unknown"; nothing is written over the
  unreadable source.
- **Why:** "Empty" turns a broken config into a permission to do anything.
- **Do not reverse:** Do not let a guard treat an unreadable input as an
  allow.
- **Owner:** maintainer.
- **Date:** <YYYY-MM-DD>

## Compatibility

### <The promise, e.g. "Existing configuration files keep working">

- **Decision:** <what stays readable, for how long.>
- **Why:** <who depends on it.>
- **Alternatives:** <a clean break, and why not.>
- **Do not reverse:** <what a migration must not do.>
- **Owner:** maintainer.
- **Date:** <YYYY-MM-DD>

## Security and privacy

### No telemetry by default

- **Decision:** <what is sent, if anything; where it goes; how a user turns it
  off.>
- **Why:** <the trust argument.>
- **Do not reverse:** Do not add a network call that sends user data without
  updating this entry and every disclosure.
- **Owner:** maintainer.
- **Date:** <YYYY-MM-DD>

## Taste

These are the decisions that keep the product recognizable. They are not style
guide rules; they are the ones a contributor would otherwise "fix".

### Interaction

- <e.g. A destructive action always asks once, and the safe choice is the
  default.>
- <e.g. A loading state never blocks the whole window.>
- <e.g. A scrollable inner pane always shows that it scrolls.>

### Visual

- <e.g. One accent color; no gradients on controls.>
- <e.g. Motion is under 200ms and never delays an input.>
- <e.g. Dense by default; comfortable is an option, not the default.>

### Copy

- <e.g. Sentence case everywhere.>
- <e.g. An error names the thing that failed and the next step, and never
  says "something went wrong".>
- <e.g. No exclamation marks in the product UI.>

### Things we never do

- <e.g. No modal that cannot be dismissed with Escape.>
- <e.g. No auto-play, no sound, no badge counts that never clear.>
- <e.g. No dark pattern in an upsell.>

## Technical architecture

Record only the choices that constrain future work, not the whole design.

### <e.g. "The gateway is a single local process">

- **Decision:** <...>
- **Why:** <...>
- **Alternatives:** <...>
- **Do not reverse:** <...>
- **Owner:** <...>
- **Date:** <...>

## Community

### A hostile or no-value issue gets one reply, then closes

- **Decision:** Loop 1 closes an issue after one short reply when it is abuse,
  harassment or trolling with no actionable report; an ad, relay or reseller
  pitch; a duplicate; a request that contradicts a recorded decision or a
  feature the brief refuses; pure preference or no actionable content; already
  fixed and released; or answered by the docs. Declined items close as "not
  planned", fixed or answered items as "completed", and duplicates with
  GitHub's duplicate reason.
- **Why:** An issue that will not produce a change still costs a slot in the
  queue and invites argument. One reply keeps a public record of why; closing
  keeps the queue meaning "work to do". The categories are narrow and
  pre-ruled so the agent does not have to guess at a maintainer decision.
- **Alternatives:** Leave them open (rejected: the queue fills with items
  nobody will act on); close silently (rejected: the reporter learns nothing,
  and it reads as censorship); ask the maintainer each time (rejected: it is
  the same decision every time).
- **Do not reverse:** Do not close a reproducible, in-scope bug because its
  reporter was rude; do not use this path for security reports, legal notices
  or threats; do not close without a reply.
- **Owner:** maintainer.
- **Date:** 2026-11-15

## Superseded

| Decision | Superseded by | Date |
| --- | --- | --- |
| *(none yet)* | | |