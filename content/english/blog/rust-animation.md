---
title: 'rust-animation project'
date: 2021-12-05T17:20:49-08:00
draft: false
tags : [Rust,MyProject]
images:
  - "https://github.com/joone/rust-animation/blob/main/examples/easing_functions.gif?raw=true"
author: "Joone Hur"
---
I had used the GNOME Clutter project in the past while [working on Accelerated Compositing in the WebKit clutter port](https://blogs.gnome.org/joone/2013/03/22/accelerated-compositing-with-clutter/). 
During that time, I extended the existing Clutter Actor APIs and implemented my own Actor to support CSS 3D transforms, which worked well. I even managed to run some impressive CSS 3D animation demos in the WebKit Clutter port.

However, the Clutter project has lost its direction and is now almost deprecated, even though it is still used in GNOME Shell.

Despite this, I had always envisioned maintaining a project similar to Clutter. As a result, I personally worked on a C++ implementation of a scene graph engine using OpenGL and even created a Rust implementation.

One day, while searching for similar projects on the Rust package site (crates.io), I discovered that someone had taken the name I wanted for my project, but the owner had not yet started the project. As a result, I decided to create [a Rust crate](https://crates.io/crates/rust-animation) on crates.io and open [my code in github](https://github.com/joone/rust-animation).

It took some time to develop my Rust implementation to be more API-style, but the experience was much more interesting than I had anticipated, and I really enjoyed programming in Rust and publishing my project using the Cargo tool. 

Once I had opened my code and created a crate on Rust crate site, I felt a greater sense of responsibility, even if nobody ended up using my project. Consequently, I continued to update my project and, in terms of code quality and functionality, the current code is much better than when it was first released. Here are the major features of rust-animation:

## Animation with easing functions
![alt easing_funcitions](https://github.com/joone/rust-animation/blob/main/examples/easing_functions.gif?raw=true)
This library essentially enables the rendering of a rectangle with a specified color or texture image on the viewport. Furthermore, it also offers support for animations, utilizing a range of easing functions, including:
```rust
      EasingFunction::EaseIn,
      EasingFunction::EaseInCubic,
      EasingFunction::EaseInOut,
      EasingFunction::EaseInOutCubic,
      EasingFunction::EaseInOutQuad,
      EasingFunction::EaseInOutQuart,
      EasingFunction::EaseInOutQuint,
      EasingFunction::EaseInQuad,
      EasingFunction::EaseInQuart,
      EasingFunction::EaseInQuint,
      EasingFunction::EaseOut,
      EasingFunction::EaseOutCubic,
      EasingFunction::EaseOutQuad,
      EasingFunction::EaseOutQuart,
      EasingFunction::EaseOutQuint,
      EasingFunction::Linear,
      EasingFunction::Step
```
## Flex UI
![alt flex_ui](https://github.com/joone/rust-animation/blob/main/examples/flex_ui.png?raw=true)
The support for Flex UI is fantastic, as someone has already implemented it for Rust projects. I simply added this crate to my project, and it works perfectly. In fact, I was able to implement the above demo in just 30 minutes. o/

## Event handling
rust-animation offers support for the EventHandler trait to enable event handling in rust-animation applications. Essentially, it works like callback functions, allowing users to receive event signals from rust-animation.

```rust
pub struct ActorEvent {
  name: String,
}

impl ActorEvent {
 pub fn new() -> Self {
    ActorEvent {
      name: "actor_event".to_string()
    }
 }
}

impl EventHandler for ActorEvent {
  fn key_focus_in(&mut self, actor: &mut Actor) {
     println!("key_focus_in: {} {}", self.name, actor.name);
     actor.apply_scale_animation(1.0, 1.1, 0.3, EasingFunction::EaseInOut);
  }

  fn key_focus_out(&mut self, actor: &mut Actor) {
    println!("key_focus_out: {} {}", self.name, actor.name);
    actor.scale_x = 1.0;
    actor.scale_y = 1.0;
  }

  fn key_down(&mut self, key: usize, actor: &mut Actor) {
     println!("key_down: {}  {}  {}", self.name, key, actor.name);

    if key == 262 {  // right cursor
       actor.select_next_sub_actor();
    } else if key == 263 { // left cursor 
       actor.select_prev_sub_actor();
    }
  }
}
```
I am currently working on adding additional event handler methods.


## User-defined layout
In addition to Flex UI, rust-animation offers the ActorLayout trait as a means of implementing custom layouts.
```rust
pub struct ActorLayout {
  name: String,
  cur_x: i32,
}

impl ActorLayout {
 pub fn new() -> Self {
    ActorLayout {
      name: "actor_layout".to_string(),
      cur_x: 0
    }
 }
}
```
When rust-animation requires an update to the layout, it calls layout_sub_actor, which then enables the user to update the locations of the child elements as follows:

```rust
impl Layout for ActorLayout {
  fn layout_sub_actors(&mut self, sub_actor_list: &mut Vec<Actor>) {
    println!("layout_sub_layer {}", self.name);
    let mut index : i32 = 0;
    for sub_actor in sub_actor_list.iter_mut() {
      self.cur_x += sub_actor.width as i32;
      sub_actor.x = index % 5 * IMAGE_WIDTH as i32;
      let col = index  / 5;
      sub_actor.y =  col * IMAGE_HEIGHT as i32;
      index +=1;
    }
  }
}
```

Currently, the most pressing missing feature is support for text rendering. Although there are various crates available to support text rendering, I would like to opt for a native Rust implementation, such as [rusttype](https://crates.io/crates/rusttype).

Happy Hacking!




