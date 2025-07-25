---
title: "Explainer for Delayed Messages API Published"
date: 2025-06-09T00:22:10-08:00
author: "Joone Hur"
tags: ["w3c", "web performance"]
categories: ["web"]
draft: false

---

I'm excited to share that the explainer for the **Delayed Messages API** has been published!

📄 [Explainer on GitHub](https://github.com/MicrosoftEdge/MSEdgeExplainers/blob/main/DelayedMessages/explainer.md)

This proposal introduces a new **web performance API** to help developers monitor `postMessage` events across windows, iframes, and web workers.

As you may know, web applications often use `postMessage` for communication between execution contexts. But these messages can sometimes be delayed—stuck in the queue while the receiver’s event loop is blocked by long-running tasks or overloaded with too many messages.

Such delays can make apps feel sluggish and unresponsive. And right now, it’s difficult for developers to pinpoint exactly why these delays happen—whether it’s due to a busy thread, message queue congestion, or the overhead of serialization and deserialization.

The **Delayed Messages API** aims to solve this by exposing detailed timing metrics for `postMessage` events—including queue wait time and blocking tasks—so developers can identify performance bottlenecks and make targeted improvements.

The explainer outlines both the motivation and the initial API design.
If you have feedback or ideas, please join the discussion here:

🗣 [Issue Tracker – Delayed Messages](https://github.com/MicrosoftEdge/MSEdgeExplainers/labels/DelayedMessages)

If you're interested in web performance more broadly, check out the MDN docs here:

https://developer.mozilla.org/en-US/docs/Web/Performance

Thanks for reading!
