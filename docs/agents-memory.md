# Agent memory

This page is about what the **agents** remember between sessions. It is not
about a memory feature the product may ship: that gets its own page under
[`subsystems/`](subsystems/README.md), and the two must not be confused — one
is how the project is built, the other is what the project does.

There are two kinds of memory in this setup, and confusing them is how a
lesson gets lost.

| Kind | Lives in | Who sees it | Survives a fork | Source of truth |
| --- | --- | --- | --- | --- |
| Repository memory | This repository | Every agent, every machine | Yes | **Yes** |
| Local memory | The agent tool's memory directory | One user, one machine | No | No, it points at the repo |

The repository is the source of truth. Local memory is a shortcut for
recalling what the repository already says, plus the small number of facts
that are genuinely local (a machine's paths, a user's preference about how to
be addressed).

## What goes in the repository

- `LESSONS.md`: what the project learned from its own mistakes.
- `docs/decisions.md`: what the maintainer decided, and why.
- `docs/project-brief.md`: what the project is for and refuses.
- `docs/subsystems/`: how the system works.
- `docs/environments.md`: how to reproduce on real machines.

If an agent learns something that would change another agent's decision, it
belongs in one of these files, in the same change that learned it. A fact that
only lives in a chat is lost at the next session.

## What goes in local memory

- **The user's preferences**: tone, language, how they want reports, what they
  never want touched. These are stable and personal.
- **Machine facts**: where the Windows box is, which port the proxy uses, the
  path to the test accounts. These are true only here.
- **Pointers**: "the release checklist is `docs/release.md`", "the router
  rules are in `docs/code-standards.md`". A memory that duplicates content
  goes stale; a pointer does not.
- **A correction the user gave once** that is not yet a repo rule. Write it
  here first; if it applies to every agent, promote it to the repository.

## What never goes in local memory

- Code structure, file lists, function names. They are in the tree and change.
- Git history, past fixes, "we fixed X in commit Y". That is what `git log`
  and `LESSONS.md` are for.
- Secrets, tokens, passwords, private user data.
- Anything a fork would need. If another maintainer needs it, it is a repo
  document.
- A decision the maintainer has not made. A memory that guesses a decision is
  worse than no memory.

## Where local memory lives

Claude Code keeps a project's memory outside the repository, under the
agent's own project folder (`~/.claude/projects/<project>/memory/` on the
machine that runs the agent):

- One fact per file, with `name`, a one-line `description`, and a `type`
  (`user`, `feedback`, `project`, `reference`).
- `MEMORY.md` is the index: one line per memory. A file that is not in the
  index is invisible to a new session.
- At session start, read the index and only the entries whose description
  matches the task.
- At session end, update the file that already covers the fact instead of
  adding a near-duplicate; delete a memory that turned out wrong.
- Machine-specific facts (paths, ports, test accounts) never enter the
  repository; a rule that applies to every agent never stays only here.

If the agent tool has no memory directory, the repository documents are the
whole memory, and that is fine: the routing table in `AGENTS.md` is what a
session reads first.

## The nightly review is the durable learning

Local memory is not a substitute for `LESSONS.md`. A lesson that exists only
in one machine's memory does not reach the next agent, the next machine or the
next fork. The nightly loop writes lessons to the repository precisely because
memory does not survive.

When the nightly review finds a lesson, ask: is this about this project's
behavior (repository), or about how this user wants to be worked with (local)?
Most are the former. Write it in the former.

## Recording a design discussion

When the maintainer explains a choice in conversation — why a layout is this
way, why a default exists, what was tried and rejected — write it down before
the session ends:

1. If it is a decision, add it to `docs/decisions.md` with the why and the
   alternatives.
2. If it is a preference about the product's feel, add it under "Taste".
3. If it is only about this user's workflow, put it in local memory.
4. If it changes a rule every agent follows, update `docs/code-standards.md`.

An agent that has the decision can make a consistent choice in a new
situation. An agent that only has the outcome will re-litigate it.

## At the start and end of a session

**Start:** recall what applies, then read `AGENTS.md` and the document the
routing table names. Memory tells you where to look; it does not replace
looking.

**End:** if you learned something durable, write it in the right place. If you
changed a rule, update every document that states it, so the rule is stated
once.

## Memory and forks

A fork starts with the repository's memory, not the original author's local
memory. That is deliberate: local memory contains one person's machine and one
person's preferences. The base's job is to make the repository documents good
enough that a new maintainer's agent can work without them.

Seed a new fork's local memory in the first session, with pointers only:

1. One memory per entry point: project brief, decisions, code standards,
   subsystem index, release gate, environments, the test command.
2. Each says "the rule is `<path>`" and nothing more. A copy of the rule goes
   stale the moment the rule changes.
3. As the maintainer makes decisions, write the decision into
   `docs/decisions.md` first, then add or update the pointer.
4. Check the result: a fresh session with no chat history can name the
   documents to read before changing code and before releasing.

The test for "is the memory working": a new agent, on a new machine, with no
chat history, makes a decision the maintainer already made and does not
re-argue it. If it re-argues, the decision is not in the repository.