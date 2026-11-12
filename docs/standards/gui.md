# UI addendum (example)

This is an example addendum for a project with a graphical interface. It
generalizes the UI rules from magpie's reviews. Keep it if the fork ships a UI;
delete it otherwise. It does not replace [`../code-standards.md`](../code-standards.md).

## Before you change a layout

A layout change is a product decision, not a code improvement. It needs an
issue asking for it, or the maintainer asking for it. The default is **no**.
A community PR that rearranges a page "for clarity" is closed with a link to
this rule, even if it looks better.

A change to the look is reviewed against the decisions in
[`../decisions.md`](../decisions.md#taste), not against the reviewer's taste.

## Interaction

- Escape closes a modal or a menu. If it does not, that is a bug.
- A destructive action asks once, and the safe choice is the default and has
  focus.
- A loading state never blocks the whole window; the rest of the UI stays
  usable.
- A button that triggers a long job says what is happening and can be
  cancelled.
- Focus is visible, and it returns to the control that opened a dialog.
- A list that can be long is searchable or filterable; a picker that cannot be
  narrowed is a bug.
- A scrollable inner pane shows that it scrolls. A pane that silently captures
  the wheel breaks every test and every user that tries to scroll the page.

## Text

- Every string is translatable; no string is built by concatenating translated
  fragments. Placeholders in every language match the source exactly.
- An error names the thing that failed and the next step. Never "something
  went wrong".
- A number, date or file size is formatted for the user's locale, not the
  developer's.
- The layout survives the longest translation, not only English. Test the
  longest string the project ships.

## Engines and widths

- Run the UI tests in **every engine the product ships**, not only the one the
  test runner defaults to. A test engine is not the product's engine: a
  transition can pass in one and freeze the real app in another.
- Run at the narrowest supported width and at a normal width. A control that
  disappears or overlaps at the narrow width is a failure.
- Run every shipped language. A missing key or a mismatched placeholder is a
  failure, not a fallback.
- If the product runs on Linux with a system webview, try motion and any new
  browser API in the real app. A headless engine on another OS proves nothing
  about the compositor.

## When to run the whole UI suite

Run all of it, not the files you think you touched, when a change:

- adds an API call the UI makes;
- adds or renames a selector, class or shared component;
- adds a layout breakpoint, a sticky/fixed element, or a pane that scrolls;
- changes a shared translation key or a placeholder;
- changes focus, keyboard handling or a modal;
- adds a browser API an older engine may lack.

A wheel event, a strict fixture, or a sticky header can turn a file you never
opened red.

## Tests

- Find a row by its label, not by a word that happens to be in it. Another row
  that says the same word breaks the test later.
- A test that scrolls the page must account for any inner pane that scrolls on
  its own: the wheel goes to the pane under the pointer, not the page.
- A screenshot comparison is checked against a known-good build, not against
  "it looks fine".
- A UI test waits for the state it asserts, not for an animation duration.
- A test that opens a window or starts the app closes it, including on
  failure, or the next test inherits a dirty state.
- A test never writes the user's real settings; it uses a temporary profile.

## Accessibility and platform

- Keyboard reachability for every action; no mouse-only control.
- Sufficient contrast in both themes; the theme follows the system by default
  and the user's choice after that.
- Reduced motion is respected; motion is short and never delays input.
- The window restores its size and position, and does not open off-screen on a
  smaller display.
- Platform conventions: menu placement, shortcuts, file dialogs, and the
  close behavior of the main window.

## Show the change to a reviewer

A reviewer who reads a diff cannot see a UI change. When a pull request
touches the UI, produce the recording before anyone reviews it:

1. **Detect.** Decide whether the PR is a UI change from the paths it touches,
   or a label a maintainer adds. Write the result in a fixed block in the PR
   description so it never fights with a human's edits.
2. **Run the PR's own build** in a fresh temporary home, never the reviewer's
   profile.
3. **Seed data first.** A change that only shows with data is invisible on an
   empty screen: accounts with quota windows, weeks of history, long lists,
   sessions, errors, several providers, a user's own settings. Seed it all
   before the app starts.
4. **Plan scenes**: what to click, hover, type, scroll and capture. Capture
   the states the change is about, and the ones next to it (empty, loading,
   error, long content, narrow width).
5. **Screenshots always; a short video when the change needs interaction or
   spans pages.** Never a GIF.
6. **If a key, token or personal value appears on screen, throw the whole
   recording away** and fix the seed or the screen.
7. **Publish** the files under a per-PR, per-commit path and link them from
   the PR description, so a re-run replaces the old ones instead of piling up.

Security, because the recording runs code nobody reviewed yet:

- The job that builds and records has **no write token**.
- The job that publishes has a write token and **never runs the PR's code**.
- Any key the recording job can read is a key a hostile PR can read: give it a
  low spending limit and nothing else.
- Do not record a real account's data; seed a fake one.

The reviewer compares the recording against the previous version's recording
on the same seed, not against a mockup.

## Reviewing a UI change

1. Run it, don't only read it. Click the thing; resize the window; change the
   language; use the keyboard.
2. Compare with the previous version on the same data, not with a mockup.
3. Check the empty, loading, error and long-content states.
4. Check the narrow width and the longest translation.
5. Check that the change did not move an unrelated page: run the whole suite
   when the change touches anything shared.
6. Say which engines, widths and languages you ran, and which you did not.