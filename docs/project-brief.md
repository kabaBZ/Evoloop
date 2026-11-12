# Project brief

This is the document an agent reads before deciding whether a request is in
scope. It is written by the maintainer, not by an agent. Keep it short enough
to be read in one sitting and specific enough to settle a borderline case.

## What it is

{{PROJECT_NAME}} is {{ONE_LINE_PURPOSE}}.

<One paragraph, in plain language: the problem, the shape of the solution, and
what a user can do after installing it that they could not do before.>

## Who it is for

- <The primary user, with the workflow they have today.>
- <The secondary user, and why they are not the primary.>

## Who it is not for

- <The user the project is deliberately not optimizing for.>
- <The use case the project will not support, and why.>

## What it refuses to do

These are product boundaries, not implementation limits. A request that
crosses one is declined with a link to this section.

- <e.g. It does not proxy traffic through a third party's servers.>
- <e.g. It does not collect usage data.>
- <e.g. It does not manage a user's credentials.>
- <e.g. It does not add reseller or affiliate addresses.>

## Quality bar

What "done" means beyond "it compiles":

- <e.g. A user's existing configuration survives an upgrade unchanged.>
- <e.g. Every failure has a message that says what to do next.>
- <e.g. No feature ships without a way to undo it.>
- <e.g. Startup stays under N seconds on a cold machine.>

## Supported environment

| Item | Supported |
| --- | --- |
| Platforms | {{PLATFORMS}} |
| Surfaces | {{SURFACES}} |
| Versions | <oldest version that still gets fixes> |
| Data formats | <what must stay readable> |

Anything outside this table is best-effort, and the reply says so.

## How this project is maintained

Code is written, reviewed, merged and released by agents. A human keeps
direction, taste and the gates. The operating rules are in
[`code-standards.md`](code-standards.md); the loops are in
[`loops/`](loops/); the decisions are in [`decisions.md`](decisions.md).

The maintainer's involvement:

- sets the brief and the decisions;
- clears anything the harm check stops;
- decides the block list;
- owns the public voice in a crisis;
- reviews the loops' weekly reports.

## Open questions

Questions the project has not settled. An agent does not answer one by
choosing; it escalates.

- <question>
- <question>