# Environments and reproduction

An agent that cannot run the failing surface will "fix" the wrong thing and
report success. This page is where the real environments are listed and how to
use them without damaging a user's machine.

## Why this exists

- **A compile is not a test.** A cross-build does not even compile the tests
  on some toolchains.
- **A cross-build is not a run on that platform.** File locking, path
  separators, symlinks, line endings, time zones and case sensitivity all
  differ, and the bug is usually in one of them.
- **A unit test is not the user's case.** The reporter's version, data,
  client and OS are the reproduction.
- **A test on the wrong toolchain is a different test.** CI's language version
  is the one that counts; reproduce there before calling a failure "the same
  on main".

## The matrix

Fill this in at bootstrap. One row per platform and surface you ship. "Yes"
means an agent can run it today, not that it should be possible.

| Platform | Version | GUI | TUI | CLI | Service/API | How the agent reaches it |
| --- | --- | --- | --- | --- | --- | --- |
| <macOS> | <15+> | <yes> | <yes> | <yes> | <yes> | <SSH / local / VM name> |
| <Windows> | <11> | <yes> | <yes> | <yes> | <yes> | <RDP / VM name> |
| <Linux> | <Ubuntu 24.04> | <yes> | <yes> | <yes> | <yes> | <VM / container> |

A surface with no environment is a surface where a fix is "not verified", and
the record says exactly that.

## Sandbox rules

Every run that touches configuration, credentials or user data uses a
temporary home, never the real one:

- Set the home directory to a fresh temporary directory.
- Set every variable the application reads: config, data, cache, state and
  temp. A missing one makes the run read the real home.
- Pin package caches so a test cannot write into a shared one another run is
  using.
- Copy the credential or config file into the sandbox; never point the
  application at the live one.
- At the end: stop every process the run started, delete the temporary home,
  and check that nothing is still running or holding a port.

A test that breaks a shared file restores the exact bytes it read, and its
cleanup checks the write. A cleanup that goes through the code under test
inherits that code's guards, so it can silently fail.

## Credentials and real accounts

Real-use verification needs a real account or key, used carefully:

- Use a dedicated test account or a disposable key where possible.
- If the reporter's account type is the point, use a copy of a real config in
  a sandbox, never the live file.
- Never print a token, cookie, session or password into a log, a commit, an
  issue or a chat. Redact before pasting.
- After the run, confirm the account is still usable and the live config was
  not modified.
- A vendor whose terms forbid automation is not exercised automatically; the
  check is done by hand and recorded as such.

## Reproducing a report

1. Read the whole thread, to its last comment. A report can be withdrawn,
   superseded or put on hold in the middle.
2. Take the exact version, platform, surface, client and configuration from
   the report. If something is missing, ask one question.
3. Rebuild the same fixture from the reporter's literal bytes, not a similar
   sample. Screenshots, logs and request bodies are evidence.
4. Reproduce the user's goal, not a step on the way: the page they wanted, the
   command they ran, the file they expected.
5. Write down the command, the output and the version. That record goes in the
   commit body.
6. Fix, then run the same reproduction again, plus the neighboring cases the
   fix could have changed.
7. If you cannot reproduce, say so and list what you tried. Do not close the
   issue as "cannot reproduce" without the maintainer's agreement.

## Platform differences to check

These are the ones that have actually caused "works on my machine" failures.
Check the ones the change touches:

| Area | What differs |
| --- | --- |
| Paths | Separators, drive letters, case sensitivity, trailing dots and spaces, reserved names |
| Links | Symlinks and junctions; one side resolved and the other not |
| Encoding | CRLF vs LF, BOM, JSON escaping of backslashes, locale-dependent formatting |
| Locking | Files that cannot be replaced while open; rename semantics |
| Permissions | Unix modes vs ACLs; executable bits that do not exist |
| Time | Local time zone, DST, UTC vs local, date rollover at midnight |
| Process | Signals, child reaping, orphan lifetime, console encoding |
| Shells | Quoting rules, globbing, `noclobber`, environment inheritance |
| Network | Winsock vs BSD errors, localhost resolution, IPv6 preference |
| GUI | Each engine's real behavior differs from the test engine's |

## Time and tests

- Never swap the process-wide local time zone in a test; give the code a zone
  of its own and test that.
- A fixed date in a test is checked against every clock the code reads. A
  fixture written for "today" is tomorrow on a CI runner in another zone.
- Wait for the state, or count what the test is about; never wait a duration.
- Reproduce a timing failure by injecting the delay, not by rerunning until it
  fails.

## Run it the way CI does

Before calling a CI failure "the same on main" or "load":

- Use CI's toolchain version and flags.
- Fetch `origin/main` and run the test at the commit you name.
- Check whether the run was cancelled, retried or run on a different attempt.
- Look for a commit that already names that run or test.
- Run the whole package, not a `-run` filter of your own tests; guard tests
  are named for other words.

## A place for the real machines

Record what exists, who owns it, and how to reset it:

| Name | Platform | Purpose | Access | Reset |
| --- | --- | --- | --- | --- |
| <mac-mini> | <macOS 15> | <GUI + real client> | <SSH> | <snapshot name> |
| <win-vm> | <Windows 11> | <installer, file locking> | <RDP> | <snapshot name> |
| <linux-vm> | <Ubuntu 24.04> | <packaging, display server> | <SSH> | <snapshot name> |

One full test suite runs on a machine at a time. Parallel suites make every
run time out, and then no record says the suite passed.