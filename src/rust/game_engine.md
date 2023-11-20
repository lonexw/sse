# 游戏引擎

# Game engine Bevy

> Games are a huge part of our culture and humanity is investing *millions* of hours into the development of games.

### Bevy : data-driven game engine built-in Rust.

- **Capable**: Offer a complete 2D and 3D feature set
- **Simple**: Easy for newbies to pick up, but infinitely flexible for power users
- **Data Focused**: **Data-oriented architecture** using the Entity Component System paradigm(shortened to ECS)
- **Modular**: Use only what you need. Replace what you don't like
- **Fast**: App logic should run quickly, and when possible, in parallel
- **Productive**: Changes should compile quickly ... waiting isn't fun

[Bevy - Introduction](https://bevyengine.org/learn/book/introduction/)

[Bevy - Introducing Bevy 0.1](https://bevyengine.org/news/introducing-bevy/)

#### ECS: **Entities**, **Components**, and **Systems**.

**Entities** are unique "things" that are assigned groups of **Components**, which are then processed using **Systems**.

### epresent globally unique data using **Resources**.

[Bevy - Assets](https://bevyengine.org/assets/#learning)

[bevy/examples at latest · bevyengine/bevy](https://github.com/bevyengine/bevy/tree/latest/examples#examples)

[Bevy - A data-driven game engine built in Rust](https://bevyengine-cn.github.io/)

### RG3D

[Fyrox](https://rg3d.rs/)

### GODOT

[Introduction](https://docs.godotengine.org/en/stable/about/introduction.html#doc-about-intro)

[godot-rust](https://godot-rust.github.io/)

[ArtStation - Making Of Minimoys Procedural Wall](https://www.artstation.com/blogs/marcchevry/YMYR/making-of-minimoys-procedural-wall)

[Download the latest indie games](https://itch.io/)

[Learn OpenGL, extensive tutorial resource for learning Modern OpenGL](https://learnopengl.com/)

[Homegrown rendering with Rust](https://medium.com/embarkstudios/homegrown-rendering-with-rust-1e39068e56a7)

OpenGL

[Basics - Learn OpenGL Rust](https://rust-tutorials.github.io/learn-opengl/basics/index.html)

[Rust-and-opengl-lessons - Collection of example code for learning OpenGL in Rust | RustRepo](https://rustrepo.com/repo/Nercury-rust-and-opengl-lessons)

# 2D Level Editor

[LDtk](https://ldtk.io/)

[GitHub - katharostech/bevy_retrograde: Plugin pack for making 2D games with Bevy](https://github.com/katharostech/bevy_retrograde)

[Introduction — Tiled 1.8.2 documentation](https://doc.mapeditor.org/en/stable/manual/introduction/#getting-started)

[Tales of Yore](https://talesofyore.com/)

# Fyrox : feature-rich, general purpose game engine

**classic OOP：**complex objects in the engine can be constructed using simpler objects.

- **Base** - a node that stores hierarchical information (a handle to the parent node and a set of handles to children nodes), local and global transform, name, tag, lifetime, etc. It has self-describing name - it is used as a base node for every other scene node (via composition).
- **Mesh** - a node that represents a 3D model. This one of the most commonly used nodes in almost every game. Meshes could be easily created either programmatically, or be made in some 3D modelling software (like Blender) and loaded in your scene.
- **Light** - a node that represents a light source. There are three types of light sources:
   - **Directional** - a light source that does not have position, only direction. The closest real-world example is our Sun.
   - **Point** - a light source that emits light in every direction. Real-world example: light bulb.
   - **Spot** - a light source that emits light in a particular direction with a cone-like shape. Real-world example: flashlight.
- **Camera** - a node that allows you to see the world. You must have at least one camera in your scene to be able to see anything.
- **Sprite** - a node that represents a quad that always faced towards a camera. It can have a texture, size, it also can be rotated around the "look" axis.
- **Particle system** - a node that allows you to build visual effects using a huge set of small particles, it can be used to create smoke, sparks, blood splatters, etc. effects.
- **Terrain** - a node that allows you to create complex landscapes with minimal effort.
- **Decal** - a node that paints on other nodes using a texture. It is used to simulate cracks in concrete walls, damaged parts of the road, blood splatters, bullet holes, etc.
- **Rigid Body (2D)** - a physical entity that is responsible for dynamic of the rigid. There is a special variant for 2D - `RigidBody2D`.
- **Collider (2D)** - a physical shape for a rigid body, it is responsible for contact manifold generation, without it any rigid body will not participate in simulation correctly, so every rigid body must have at least one collider. There is a special variant for 2D - `Collider2D`.
- **Joint (2D)** - a physical entity that restricts motion between two rigid bodies, it has various amounts of degrees of freedom depending on the type of the joint. There is a special variant for 2D - `Joint2D`.
- **Rectangle** - a simple rectangle mesh that can have a texture and a color, it is a very simple version of a Mesh node, yet it uses very optimized renderer, that allows you to render dozens of rectangles simultaneously. This node is intended to be used for **2D games** only.

[fyrox - Fyrox Cheat Book](https://fyrox-book.github.io/fyrox/introduction.html)

