# Release

A release is the moment a fix reaches users. Everything before it is
rehearsal; everything after it is public. The loops release often, so the
process must be boring, checkable and safe to repeat.

## Before the tag

- [ ] The change is merged on `{{MAIN_BRANCH}}`.
- [ ] The full suite ran green **on that commit**, not on the branch before
      the last merge.
- [ ] The check that fails without the fix was run and its output recorded.
- [ ] [`CHANGELOG.md`](../CHANGELOG.md) has an entry for this version, naming
      every user-visible change with issue numbers.
- [ ] The version is free: fetch tags and check the commit is not already
      tagged. Releases can be made from more than one place.
- [ ] A hard-to-reverse change has a rollback story, tested once.
- [ ] Every platform that ships has been built and, where the change touches
      it, run.

## Version

- Tags are `{{TAG_PREFIX}}<major>.<minor>.<patch>`.
- A fix release increments the patch; a new behavior increments the minor; a
  breaking change increments the major and gets a migration note.
- A version is reserved when it is tagged, not when it is planned. Two loops
  must not pick the same version: check the remote tags immediately before
  tagging, and tag with the remote as the source of truth. If two processes
  can tag, they take a lock ref first; checking tags is a check, not a lock.

## Changelog

The changelog is [`CHANGELOG.md`](../CHANGELOG.md) at the repository root. One
entry per version, newest first: version, date, then the user-visible changes
with issue or PR numbers. Internal refactors, test-only changes and dependency
bumps with no user-visible effect get no line. The entry is written in the same
commit that bumps the version, before the tag.

A project that generates release notes from merged PRs still keeps the file:
the release body is written for the moment, the changelog is what a user reads
three versions later. The base's own changes are in
[`BASE-CHANGELOG.md`](BASE-CHANGELOG.md); that file is not the product
changelog.

## Tag

Tag only after the green run, on the commit that was tested:

```
git tag {{TAG_PREFIX}}<version> <sha>
git push origin {{TAG_PREFIX}}<version>
```

If the run turns red between the merge and the tag, do not tag. Fix, run
again, tag the fixed commit.

## Publish

The release must have, before it is announced:

- every expected asset ({{RELEASE_ASSET_COUNT}}) built from the tagged commit;
- signatures or attestations if the project ships them;
- the changelog attached, in the language(s) the project uses;
- the correct "latest" marker.

**A tag is not a release.** Check that the release exists with its assets
before naming the version to anyone. A tag whose build failed leaves users on
an older version while the changelog says otherwise.

**Publish in tag order, or mark only the highest version as latest.** A later
publish of an older version takes the latest marker with it, and with it the
update feed, the installer and any package manager that follows "latest". If a
publish step can run out of order, make the step itself decide: the highest
version gets the marker, nothing else does.

A release step that waits on an external service gets its own timeout. A job
that hangs inside the platform's overall limit holds the machine and hides the
failure.

## After the release

- [ ] `{{VERSION_CHECK_CMD}}` shows the version and its assets.
- [ ] The update feed or package index points at the new version.
- [ ] The reporter of the fixed issue is told the version, once.
- [ ] The issue is closed only if the reporter's case works; if they come
      back, reopen it.
- [ ] Any post-release smoke check on a clean machine passed.

## When it fails

- File the failure as its own issue with the run link, in the same change that
  renamed the version or moved on.
- Do not tag over a failed release. A new tag does not make the missing one
  exist.
- If a publish run stays red for one known package, check each package
  individually; the run's color hides every new failure behind the known one.
- Roll back by publishing a new version that reverts the change, not by
  deleting a release users may already have.

## Record

Put this in the release body or the tag annotation:

```
Version: <version>
Commit: <sha>
Green run: <link, attempt number>
Platforms built: <list>
Assets: <count>
What was run: <commands and results>
What was not run: <explicitly>
Rollback: <how, and whether it was tested>
```

A release with no record cannot be audited when a user reports a regression
three versions later.

## Never

- Tag before the green run finishes.
- Publish an older version after a newer one without handling "latest".
- Delete a published release or re-point a tag; publish a fix instead.
- Announce a version that is not downloadable yet.
- Release a change whose only verification is a compile or a unit test.