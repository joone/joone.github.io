---
title: "Fixing the issue where sourceCharPosition always returns -1 for resolve-promise invoker type in LoAF script entries"
date: 2026-03-01
description: ""
tags: "chromium, LoAF, performance, V8"
---

In Long Animation Frames (LoAF) script entries, the `sourceCharPosition` value for
the `resolve-promise` invoker type is consistently `-1` when the promise originates
from APIs like `fetch` or `scheduler.yield`. This issue limits precise attribution
of the source location for these promises, making it difficult to debug performance
problems involving such APIs.

## Chromium Issues

- [sourceCharPosition always returns -1 for resolve-promise Invoker type in LoAF script entries (381529126)](https://issues.chromium.org/issues/381529126)
- [\[LoAF\] scheduler.yield() resolver sourceLocation seems broken (40940947)](https://issues.chromium.org/issues/40940947)

This was not actually a bug, but a missing implementation: getting `sourceCharPosition`
is an expensive operation that requires looking into call stacks in V8. When I
implemented this missing feature, I had to add a new interface to V8.

## Change List

The patches listed below resolved this issue.

- Chromium: [\[LoAF\] Set sourceCharPosition for the invoke-type resolve-promise (6060398)](https://chromium-review.googlesource.com/c/chromium/src/+/6060398)
- V8: [Add StackFrame::GetSourcePosition API (6060340)](https://chromium-review.googlesource.com/c/v8/v8/+/6060340)

However, the feature remained behind the Blink runtime flag
(`LongAnimationFrameSourceCharPosition`) for a year because the LoAF API owner had
concerns about the potential performance impact. To address this, I optimized the
retrieval of script URLs and source locations from V8:

- [LoAF: Optimize script URL and source location retrieval (387412856)](https://issues.chromium.org/issues/387412856)

## Performance Optimization

The feature remained behind a flag for over a year due to **performance regressions**
observed in early Finch tests. In particular, V8 performance regressions prompted
caution and required multiple rounds of optimization work.

### Implement lazy conversion for SourceLocation::Url()

Previously, the URL string for a `SourceLocation` captured from the V8 stack was
immediately converted from a `v8::String` to a Blink `String` in
`CapturePartialSourceLocationFromStack`. This conversion involves allocation and
copying, which may be unnecessary if the `SourceLocation::Url()` method is never
called.

This change modifies `SourceLocation` to perform this conversion lazily, improving
the overall performance of getting a source URL for the `resolve-promise` invoker
type in LoAF script entries.

- [Implement lazy conversion for SourceLocation::Url() (6470587)](https://chromium-review.googlesource.com/c/chromium/src/+/6470587)

### Optimize retrieval of script URL and source location in CapturePartialSourceLocationFromStack

This change streamlines the retrieval of the script URL and source offset by
leveraging `v8::StackFrame` directly, removing the dependency on `StackTrace`. This
optimization reduces redundant stack traversal and improves performance.

- [Optimize retrieval of script URL and source location in CapturePartialSourceLocationFromStack (6139347)](https://chromium-review.googlesource.com/c/chromium/src/+/6139347)

### Convert SourceLocation to an Oilpan GC class

`SourceLocation` was previously managed via `std::unique_ptr`, while
`ScriptPromiseResolver` is managed by the V8 garbage collector. Making
`SourceLocation` a GC-managed class can potentially improve allocation performance
by using the GC's infrastructure. More importantly, a GC-collected `SourceLocation`
can more easily hold onto V8 objects (such as `v8::Local<v8::StackFrame>` or
`v8::Local<v8::String>`) needed for the lazy approach, as the GC helps manage the
V8 handles' lifetimes correctly.

- [Convert SourceLocation to Oilpan GC class (6485451)](https://chromium-review.googlesource.com/c/chromium/src/+/6485451)

## Why This Matters

Previously, many LoAF entries — especially those triggered by promise-based invokers
such as `resolve-promise`, `fetch()`, and `scheduler.yield()` — reported
`sourceCharPosition = -1`, making it extremely difficult to understand what actually
caused a long animation frame.

With this feature now enabled, LoAF entries provide accurate script attribution,
making it much easier to diagnose main-thread congestion and identify the true root
causes of long animation frames.

## Feature Rollout

[Finch](https://chromium.googlesource.com/chromium/src/+/refs/heads/main/docs/finch_best_practices.md)
is Google's infrastructure for running experiments and rolling out features gradually
to Chrome users. It allows developers to enable a change for a small percentage of
the user base (e.g., 1% of Chrome Beta users) and monitor its effects. Due to all
the performance optimization efforts, no performance regression was observed with
this change, and Google finally decided to enable this feature by default.

- [Enable LongAnimationFrameSourceCharPosition by default (7209141)](https://chromium-review.googlesource.com/c/chromium/src/+/7209141)
