---
title: 'Rust & DRM'
date: 2018-05-24T16:06:00.003-07:00
categories: ["graphics"]
draft: false
aliases: [ "/2018/05/rust-drm.html" ]
tags : [graphics, Rust]
---

I was surprised that there are many low-level graphics projects written in Rust and I started looking into one of them: [drm-rs](https://github.com/Smithay/drm-rs). Then, I added [an example](https://github.com/Smithay/drm-rs/commit/9bdf3a23f08602e33fca389602d4e81c5cd05c7a) for handling page-flip event.  
  
drm-rs is a subproject of Smithay that is a wayland compositor written in Rust. It allows Rust applications to access the Direct Rendering Manger(DRM), a subsystem of the Linux Kernel. So, we could directly paint something into a frame buffer and render it on the display using DRM APIs.