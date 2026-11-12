## What changed

<!-- One paragraph. The user-visible behavior before and after, and the issue
     it closes. -->

Closes #

## Subsystems touched

<!-- Link the reference page and the named symbols. Say "none" if no documented
     contract changed. -->

- Reference:
- Behavior before:
- Behavior after:
- Does this preserve a documented contract? <!-- yes / no, and which -->

## How it was verified

<!-- The command, the version, the platform, the surface. Paste the real
     output lines, not a summary. -->

- The check that fails without this change:
  ```
  <command> → FAIL: <what it printed>
  ```
- With the change:
  ```
  <command> → ok
  ```
- Full suite at this head:
  ```
  <command> → <result>
  ```
- Platforms run: <!-- list; do not list a platform you only cross-built -->

## What was not run

<!-- Explicitly list every check from the acceptance criteria that was not run,
     and why. "Nothing" is a valid answer only if everything was run. -->

-

## Siblings and related cases

<!-- The same pattern elsewhere that this change fixes or deliberately leaves
     alone. If left alone, link the issue filed for it. -->

-

## Real use

<!-- Which real account, client, vendor or configuration was exercised. If
     not possible, say so and say what a maintainer should try. -->

-

## Risks and rollback

<!-- What breaks if this is wrong, and how it is undone. A hard-to-reverse
     change needs a tested rollback. -->

- Risk:
- Rollback:
- Tested? <!-- yes / no -->

## Community PR checklist

<!-- Delete this section for maintainer PRs. -->

- [ ] The diff contains nothing outside the stated goal.
- [ ] No CI/release file, new network address, credential access, new
      dependency, binary or obfuscated code.
- [ ] No relay, reseller or affiliate address.
- [ ] The new test fails on the old code, and I pasted what it printed.
- [ ] I ran this myself on current `main`; I am not relying on the PR's CI.
- [ ] I read the project's decisions and this change does not reverse one.