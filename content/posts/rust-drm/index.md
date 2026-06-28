---
title: "Rust & DRM"
date: 2018-05-24
description: ""
tags: "graphics, Rust"
---

I was surprised to find that there are many low-level graphics projects written in Rust, so I started looking into one of them: [drm-rs](https://github.com/Smithay/drm-rs). I then added [an example](https://github.com/Smithay/drm-rs/commit/9bdf3a23f08602e33fca389602d4e81c5cd05c7a) for handling the page-flip event.

drm-rs is a subproject of Smithay, a Wayland compositor written in Rust. It allows Rust applications to access the Direct Rendering Manager (DRM), a subsystem of the Linux kernel. With it, we can paint directly into a frame buffer and render it on the display using the DRM APIs.