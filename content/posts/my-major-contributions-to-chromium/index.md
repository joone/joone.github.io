---
title: "My major contributions to Chromium project"
date: 2016-11-14
description: ""
tags: "chromium, Blink"
---

I have been working on the Chromium project since 2013. Over that time I have
fixed a large number of bugs and implemented several features across the
browser, mostly in two areas: **Blink editing** (the engine behind
`contenteditable` and rich-text editors used by web apps such as Gmail) and
**Aura / Ozone / Wayland support** (the windowing and display layers that let
Chromium run natively on Linux).

This post summarizes those contributions. For each issue I include a short
description of the problem and how it was fixed, and for each change list (CL)
I add a one-line note so you can understand the work without having to open
every link.

## Blink Editing

These fixes target the editing engine behind `contenteditable` elements —
fixing wrong DOM mutations, broken whitespace handling, stray markup on
copy/paste, and incorrect `queryCommandState` results.

1. **[Issue 226941](https://bugs.chromium.org/p/chromium/issues/detail?id=226941) — Contenteditable issues related to backspace handling.** Pressing Backspace in an editable region merged or deleted nodes incorrectly, corrupting the resulting markup.
   - [CL 2064473002](https://codereview.chromium.org/2064473002): fix the node merging/deletion logic that ran on Backspace.
   - [CL 2117663002](https://codereview.chromium.org/2117663002): follow-up covering additional edge cases.
2. **[Issue 318925](https://bugs.chromium.org/p/chromium/issues/detail?id=318925) — Copy and paste sometimes removes spaces between words.** Whitespace was collapsed during paste, so words ran together. Fixed iteratively across several CLs.
   - [CL 2193033004](https://codereview.chromium.org/2193033004): preserve significant whitespace when pasting.
   - [CL 2280513004](https://codereview.chromium.org/2280513004): handle more whitespace-collapsing cases.
   - [CL 2320533002](https://codereview.chromium.org/2320533002): additional fixes and test coverage.
   - [CL 2325553002](https://codereview.chromium.org/2325553002): refinement of the previous change.
   - [CL 2336043006](https://codereview.chromium.org/2336043006): final follow-up to close remaining cases.
3. **[Issue 310149](https://bugs.chromium.org/p/chromium/issues/detail?id=310149) — `&nbsp;` is forced on SPACE between text nodes.** Typing a space between text nodes inserted a non-breaking space when a regular space should have been used.
   - [CL 2175163004](https://codereview.chromium.org/2175163004): only emit `&nbsp;` when actually required.
4. **[Issue 335955](https://bugs.chromium.org/p/chromium/issues/detail?id=335955) — Unwanted spans inserted in contentEditable elements.** Editing operations added redundant `<span>` wrappers, polluting the markup.
   - [CL 2072093002](https://codereview.chromium.org/2072093002): avoid creating unnecessary span wrappers.
5. **[Issue 571420](https://bugs.chromium.org/p/chromium/issues/detail?id=571420) — Chrome hangs when creating a bullet list in contenteditable.** Building a bulleted list could trigger an infinite loop that froze the tab.
   - [CL 2250133004](https://codereview.chromium.org/2250133004): fix the list-creation logic to prevent the hang.
6. **[Issue 634482](https://bugs.chromium.org/p/chromium/issues/detail?id=634482) — Formatting tags converted to spans with styles on cut/paste.** Semantic tags such as `<b>`/`<i>` were rewritten as styled spans during cut/paste, losing the original markup.
   - [CL 2229703004](https://codereview.chromium.org/2229703004): keep the original formatting tags instead of converting them to spans.
7. **[Issue 625802](https://bugs.chromium.org/p/chromium/issues/detail?id=625802) — Unnecessary quote appears after clicking "indent more" in the compose box.** Increasing indentation inserted a spurious blockquote.
   - [CL 2175433002](https://codereview.chromium.org/2175433002): stop adding the extra quote on indent.
8. **[Issue 582225](https://bugs.chromium.org/p/chromium/issues/detail?id=582225) — `document.queryCommandState` isn't working correctly.** The command-state query returned wrong results for certain selections.
   - [CL 1986563002](https://codereview.chromium.org/1986563002): correct the `queryCommandState` evaluation.
9. **[Issue 584939](https://bugs.chromium.org/p/chromium/issues/detail?id=584939) — `queryCommandState` returns true for bold/italic/underline/strikethrough after selecting an image.** Selecting an image incorrectly reported text styles as active.
   - [CL 1960553002](https://codereview.chromium.org/1960553002): return the correct state when the selection is an image.
10. **[Issue 385374](https://bugs.chromium.org/p/chromium/issues/detail?id=385374) — `queryCommandState` can return true for both list types.** Both ordered and unordered list states were reported as active at the same time.
    - [CL 2268843002](https://codereview.chromium.org/2268843002): report only the list type that actually applies.
11. **[Issue 232188](https://bugs.chromium.org/p/chromium/issues/detail?id=232188) — Caret color issue in contenteditable elements.** The text caret was drawn with the wrong color in some editable contexts.
    - [CL 14098003](https://codereview.chromium.org/14098003/): use the correct caret color.

## Aura and Wayland support

These changes improve how Chromium integrates with the Linux desktop and bring
up the Ozone/Wayland windowing backend.

1. **[Issue 408481](https://bugs.chromium.org/p/chromium/issues/detail?id=408481) — System dialogs (e.g. "Save As…") are not modal on Ubuntu.** Native file dialogs did not block their parent window, so users could keep interacting with the page behind them.
   - [CL 1624793002](https://codereview.chromium.org/1624793002/): make the system dialogs properly modal to their parent window.
2. **[Issue 473228](https://bugs.chromium.org/p/chromium/issues/detail?id=473228) — Make `*::ShowWithWindowState(minimized, maximized, fullscreen)` consistent across platforms.** Window show/restore behavior differed between platforms; this unified it. Landed across several CLs.
   - [CL 1125383008](https://codereview.chromium.org/1125383008): align the initial show-state handling.
   - [CL 1138383008](https://codereview.chromium.org/1138383008): follow-up for additional window states.
   - [CL 1159703006](https://codereview.chromium.org/1159703006): platform-specific adjustments.
   - [CL 1130033003](https://codereview.chromium.org/1130033003): finalize the consistent behavior and tests.
3. **[Issue 578890](https://bugs.chromium.org/p/chromium/issues/detail?id=578890) — Upstream Wayland backend for Ozone.** Contributed the Wayland platform backend so Chromium can run natively under a Wayland compositor.
   - [CL 2042503002](https://codereview.chromium.org/2042503002): upstream the initial Ozone/Wayland backend.
4. **[Issue 50485](https://bugs.chromium.org/p/chromium/issues/detail?id=50485) — Korean Hangul typing issue.** Composing Korean text produced incorrect input in some cases.
   - [CL 13818036](https://codereview.chromium.org/13818036/): fix the Hangul composition handling.
