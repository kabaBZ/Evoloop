# Contributing

This project is maintained by agents under a human maintainer. The rules are
written for both.

## Before you open a pull request

1. Read [`docs/contributors.md`](docs/contributors.md). The harm check runs
   before any review of what the change does.
2. Read [`docs/decisions.md`](docs/decisions.md). A recorded decision is not
   reopened by a drive-by change.
3. Use the [pull request template](.github/pull_request_template.md). It asks
   what you ran and what you did not run; an empty verification record is a
   reason to close the PR.
4. Keep the change to one goal. An unrelated refactor goes in its own PR.

## Bug reports

Name the version, the surface and the OS, and the steps that reproduce it. See
the issue template. A report without a reproduction may be closed with a
question, not a fix.

## Security

Report vulnerabilities through [`SECURITY.md`](SECURITY.md), never a public
issue.

## For coding agents

Start at [`AGENTS.md`](AGENTS.md) and [`LESSONS.md`](LESSONS.md). An agent's
pull request is reviewed by the same rules, and its verification record is
checked before the merge.