# Lessons from merged work

{{PROJECT_NAME}}'s code is written, reviewed, merged and released by agents.
Each night the day's commits are reviewed for what had to be fixed again, what
shouldn't have merged and what was verified too thinly. What we learned is
kept here. **Read this before changing code.** Each lesson is a rule, then
why, then the evidence. "Seen" counts the days a review found it. A lesson
seen on 3 or more separate days moves into
[`docs/code-standards.md`](docs/code-standards.md).

## How this file works

- The HTML comment at the top (`reviewed-through:`) is the review watermark:
  the last commit a nightly review finished with, in the form
  `<short sha> (<YYYY-MM-DD HH:MM ±HHMM>)`. A review starts after that commit,
  never before it, and updates the watermark in the same commit that records
  its lessons. If it says `none`, the first review covers the last 24 hours
  and sets it.
- One entry is one rule. If two rules need two different fixes, they are two
  entries.
- Evidence is concrete: PR or issue number, commit, date, and what went
  wrong. "Be careful" is not a lesson.
- "Seen N×" counts **distinct days**, not occurrences in one review. Three
  sightings on the same day are `Seen 1×`.
- When an entry reaches `Seen 3×`, the nightly review moves the rule into
  `docs/code-standards.md`, deletes the entry here, and records the move
  under [Moved to the code standards](#moved-to-the-code-standards). A
  review checks every promoted rule.
- A "Done right" entry records a change that avoided a known trap. It is read
  as an example, not as a rule.
- Entries are append-only within the day. Removing an entry requires either a
  promotion or a note saying which entry supersedes it and why.
- A lesson that is already in the code standards but was missed again is not
  re-added here as new. The review adds a guard test or a named check, and
  records the miss under "Criteria still missed".

## Entry format

```markdown
**<The rule, as an instruction to the next agent.>**
<Why it matters, in one or two sentences. What went wrong when it was missed.>
- <Evidence: issue, PR, commit, date, the concrete failure.>
- <A second piece of evidence if the first is not enough.>
- Seen N× (YYYY-MM-DD, YYYY-MM-DD).
```

## Reading the report

*(No entries yet.)*

## Fix the class, not the sample

*(No entries yet.)*

## The user's own files and settings

*(No entries yet.)*

## Verification

*(No entries yet.)*

## Concurrency and tests

*(No entries yet.)*

## Red tests and releases

*(No entries yet.)*

## Merging and closing

*(No entries yet.)*

## Moved to the code standards

These rules were seen on three or more days. The rules and their evidence are
in [`docs/code-standards.md`](docs/code-standards.md); a review still checks
each one. The base ships with none of its own — the code standards already
contain the rules inherited from magpie. Add a line here each time a local
lesson is promoted.

- *(none yet)*

## Nightly review checklist

Run this at the end of each day. It is also the body of loop 3
([`docs/loops/3-nightly-review.md`](docs/loops/3-nightly-review.md)).

1. List the day's commits and releases.
2. For each, ask:
   - Did any fix come back within the day or the next day? What did the first
     attempt miss?
   - Did anything merge that should not have: scope creep, an unrelated file,
     a reversed decision, a community PR that skipped the harm check?
   - Was any verification thinner than the acceptance criteria: a unit test
     without the old-code check, a platform claimed but not run, a "passes
     alone" red, a suite run at an older head?
3. Add or update one entry per finding. Increment `Seen` only if today is a
   new day for that rule.
4. Promote every entry at `Seen 3×`.
5. Record what went right, when it is a model for the next agent.
6. Propose block-list changes to the maintainer; never enforce them.
7. Commit the lesson file in one commit whose subject names the lessons
   added. The body says which commits were reviewed.