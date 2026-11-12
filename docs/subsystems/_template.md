# <Subsystem name>

<One paragraph: what this subsystem is responsible for, and where its boundary
is. What is explicitly not its job.>

## Responsibilities and sources of truth

| Part | Responsibility | Source |
| --- | --- | --- |
| <part> | <what it does> | `` `<file>` `` (linked relative to this page) and `<symbol>` |
| <part> | <what it does> | <external system or repo> |

<Name the authoritative map, table or symbol. If two places list the same
thing, say which one wins and which is a lookup.>

## Runtime paths

<The main paths through the subsystem, in the order they run. Name the entry
point, the decision points, and the side effects. Link named symbols, not line
numbers.>

## State and transitions

<What state exists, where it is stored, who writes it and under which lock or
transaction. For each transition: trigger, before, after, and what observes
it. Note what is in memory only and lost on restart.>

## Contracts and constraints

<Interfaces other parts depend on; invariants; ordering requirements; limits;
compatibility rules. Explain field meanings that signatures alone do not
express.>

## Failure and compatibility behavior

<What happens when a read fails, a dependency is missing, the network is down,
the data is from an older version. What is preserved, what is dropped, what is
reported to the user. "Unknown" is not "empty".>

## Verification

<The exact commands and tests that exercise this subsystem, and which behavior
each covers. Note what cannot be tested automatically and how it is checked
by hand. Say which checks were not run.>

## Not covered

<Known gaps, open questions, behaviors that look wrong but are deliberate.
Link the issue or the decision in [`../decisions.md`](../decisions.md).>

## Change log

<Optional. Only decisions that alter a contract, with a link to the issue or
commit. Do not duplicate the feature changelog.>