# Language addendum: <language>

An addendum adds the rules that only make sense for one language or toolchain.
It does not replace [`../code-standards.md`](../code-standards.md); it says how
those rules are satisfied here. A reviewer reads the base standards first, then
this page.

Keep one addendum per language. If a rule is true for every language, it
belongs in the base standards instead.

## Toolchain

- Language version, and where it is pinned: <file>.
- Package manager and lock file: <file>.
- Build tags, profiles or feature flags that change what compiles: <...>.

## Format and static analysis

- Formatter command: `{{LINT_CMD}}`.
- Files excluded from formatting, and why.
- Static analysis command and the checks it enables.
- Cross-platform analysis: which platform's build must pass even if the code
  is not run there.

## Tests

- Fast test command: `{{TEST_CMD}}`.
- Full suite command: `{{FULL_TEST_CMD}}`.
- How a single test is run: `<command> -run <name> <package>`.
- How tests get a temporary home: {{SANDBOX_HOME_RULE}}.
- Which tests cannot use the shared test helper, and what they must set
  themselves.
- How the race detector or equivalent is run, and which packages need it.
- Timeout policy: the default timeout that is too short, and the value CI
  uses.

## Concurrency

- The primitives this language makes easy to misuse.
- What "nothing blocks under a lock" means here.
- How shared state is handed off between threads or tasks.
- How a test proves a race (injected delay, repetition count, detector).

## Platform differences

- File paths, separators, case, locking and rename semantics.
- Line endings and encoding.
- Process, signal and console behavior.
- What the test runner does on each platform (for example: compiles but does
  not run).

## Dependencies

- How a dependency is added, pinned and audited.
- What needs maintainer approval.
- How a vendored or generated file is identified.

## Performance and size

- The budgets that exist, what they measure, and where they are enforced.
- What to do when a change reaches a budget (move something out, never raise
  it to fit).
- How a performance claim is measured, and on what machine.

## Adding a rule

1. State the rule as an instruction.
2. Say why, with the failure that taught it.
3. Give the command that checks it.
4. If it is universal, propose it for the base standards instead.