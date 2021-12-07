---
title: 'rust-animation project'
date: 2021-12-05T17:20:49-08:00
draft: false
tags : [Rust,MyProject]
---

I had used GNOME Clutter project before when [I worked on Accelerated Compositing in the WebKit clutter port](https://blogs.gnome.org/joone/2013/03/22/accelerated-compositing-with-clutter/).
At that time, I implemented my own Actor by extending the existing Actor APIs. It worked well:
I managed to run some CSS 3D animation demos in the WebKit Clutter port.

Unfortunately, the Clutter project lost its way.
Now, the project is almost deprecated even though it is still used in GNOME Shell.
Anyway, I had a dream of maintaining a project like Clutter, so personally worked on a C++ implementation of scene graph engine using OpenGL so I also have a Rust implementation.
One day, I just tried to search similar projects in Rust package site(crates.io) and
found out somebody took good project name, but the owner didn't start the project.
That's why I just created [a Rust crate](https://crates.io/crates/rust-animation) in crates.io and open [my code in github](https://github.com/joone/rust-animation).
Of course, it took some time to make my Rust implementation more like API style, but it was much interesting than I thought
and I really enjoyed Rust programming and publishing my project using Cargo tool.

After opening the code and creating a crate in Rust crate site,
I felt more responsibility even if no one uses my project :-), so I continued to update my project.
In terms of code quality and functionality, the current code is much better than when we first pushed it.
Here are the major features of rust-animation:

## Animation with easing functions
![alt easing_funcitions](https://github.com/joone/rust-animation/blob/main/examples/easing_functions.gif?raw=true)
Basically, it renders a rectangle with color or a texture image on the viewport. In addition to this, it supports animations with the following easing functions:
```
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
Flex UI support is amazing because someone already implemented this for Rust projects. I just added this crate to my project. It is perfectly working well. I was able to implement the above demo in 30 min. o/


## Event handling

rust-animation supports EventHandler trait to support event handling in a rust-animation application. Basically, it works like callback functions so the user basically gets event signals from rust-animation. 

```
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
I’m adding more event handler methods.


## User defined layout
In addtion to Flex UI, rust-aniation provide a way to implement user's own layout using the ActorLayout trait.
```
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

Whenever, rust-animation needs to update the layout, layout_sub_actor is called from rust-animation, then the user can update child locations like this.

Currently, the most urgent missing feature is to support text rendering. There are many crates to support text rendering, but I want to use a native Rust implementation so I might use [rusttype](https://crates.io/crates/rusttype).

Happy Hacking!




