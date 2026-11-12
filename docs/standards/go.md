# Go addendum (example)

This is an example addendum, extracted from magpie's Go rules and generalized
only as far as Go allows. Keep it if the fork is Go; delete it otherwise. It
does not replace [`../code-standards.md`](../code-standards.md).

## Toolchain

- Pin the version in `go.mod`; CI reads it from there, so a local toolchain
  that differs from CI is a different compiler.
- Reproduce a CI-only failure on CI's version, not yours. A segfault in one
  patch release is not reproducible on another.

## Format and static analysis

- `gofmt -l` on every changed file. Run it on Linux: Windows checkouts can
  turn line endings into a false positive.
- `go vet ./...` on every supported OS, with the build tags that OS needs. A
  cross-OS build skips `_test.go`, so vet is the only thing that compiles the
  tests there.
- Do not put `regexp.MustCompile` at package level. Compile lazily, behind
  `sync.OnceValue` or an equivalent, so importing a package does not cost a
  compilation and a guard test that forbids eager package-level values stays
  green.

## Tests

- `go test ./...` is the suite. A `-run` filter of your own tests is not a run
  of the package: guard tests are named for other words and live in packages
  you did not touch.
- Run the whole package a change touches. A new route, setting, agent or
  package-level value runs the packages whose guards list them, even when the
  change is elsewhere.
- `go test`'s default 10 m timeout is too short for a large suite. Use the
  project's timeout, and give the slow package room.
- One full suite per machine at a time. Parallel full suites make every run
  time out, and then no record says the suite passed.
- `t.Setenv` forbids `t.Parallel` in the same test. Know which of your tests
  cannot be parallel before you add one.
- Run a test under `-race` before pushing if it starts a goroutine, a timer or
  a process, or if it bounds allocations: a pool drops a quarter of what is
  put in it under the detector.
- A test that frees a port can have it handed to anything next. Bind `:0` and
  keep the listener, or use a Unix socket in a temp dir.
- A test that reads a dependency's source needs it on disk. `go mod download`
  it in the test, or keep the bytes in `testdata`.

## Concurrency

- Never send on a channel while holding a lock the receiver takes.
- Set a field before the handoff that lets another goroutine read it; a
  listing route that reads a struct while a launch writes it is a race even if
  the read is "just for display".
- A struct copied by value that contains a map is not a copy. Pass pointers
  for shared state.
- Hold the clock in a variable, not `time.Local`. Swapping `time.Local` in a
  test races every `time.Now` in the process, including goroutines an earlier
  test left running.

## Platform differences

- Windows CI may compile the tests and run only native checks. A Windows-only
  failure shows nowhere until the package is run on a Windows machine.
- `$HOME` is not read on Windows; `USERPROFILE` is. A test sandbox that sets
  only `HOME` reads the real profile.
- `os.Rename` cannot replace an open file on Windows. Write to a flushed temp
  file and rename, and test the rollback path.
- Paths: separators, drive letters, junctions, and case-insensitive
  comparison. If one side of a comparison follows links, so does the other,
  in the code and in the test's expected value.
- A path written into JSON doubles its backslashes. A test that expects the
  raw path is red on Windows only.

## Dependencies and size

- A package-level value is imported by every binary that imports the package.
  Keep heavy initialization lazy.
- A size budget counts code, not comments. Do not delete the comments that
  explain why the code is as it is in order to fit.
- After a rebase, run the size budget again: two changes that each fit at
  their own base can exceed it together.