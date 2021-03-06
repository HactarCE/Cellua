## Cellua has been discontinued in favor of [NDCell](https://github.com/HactarCE/NDCell).

# Cellua

[![Discord](https://img.shields.io/badge/chat-on%20Discord-7289da.svg?logo=discord&logoWidth=17&logoColor=white)](https://discord.gg/vdJwHQF)

There are a vast number of cellular automaton simulators already, but very few support rules with an arbitrary number of states and transitions. [Golly](http://golly.sourceforge.net/Help/index.html) is the most popular, and is _very_ fast with Conway's Game of Life and similar automata, but has measley 3D and large-neighborhood support. Additionally, all rules must be defined in Golly's own ruletable format; while this works well for some rules, it is incredibily tedious and inefficient for others. Due to the lack of other modern, configurable cellular automaton simulators, CA communities tend to focus their efforts on creating and exploring only those automata that Golly is capable of simulating.

Cellua aims to resolve this problem by providing a Lua- and Python-extensible cellular automaton simulator, supporting both 2D and 3D rules specified by Lua transition functions.

## Why Python?

1. Python is easy. I already know it. Everyone knows it, or can learn it in a week!
2. [Numpy](https://www.numpy.org) is blazingly fast and easy-to-use. I take every opportunity I can to use it.
3. Wide range of 3D rendering opportunities, from high-level premade engines to low-level OpenGL bindings. If rendering performance becomes an issue in Cellua, I can just switch to a lower-level rendering library.

## Why Lua? Why not a custom ruletable format?

1. Lua is easy. I already know it. Everyone knows it, or can learn it in a day!
2. I can [sandbox it](http://lua-users.org/wiki/SandBoxes).
3. [Lupa](https://github.com/scoder/lupa) uses LuaJIT2, which is _fast_. Much faster than Python, at least.
4. Custom ruletable formats are limiting, annoying to parse, and annoying to write.
5. People write ruletable generators for Golly. If your rule definition language is Turing-complete, you can use the same system for rule definition and rule generation.

## Installation

In the future, I plan to distribute binaries of stable versions using [PyInstaller](https://www.pyinstaller.org/). Once Cellua is ready, you'll find those on the [releases page](https://github.com/HactarCE/Cellua/releases). For now, see **Development** to build the program from source.

## Usage

There's nothing to use right now.

## Development

These instructions assume that you are using Linux, but the process is similar on other operating systems.

There are three options for installing Cellua:

* [Lua 5.2+](#install-with-normal-lua) — Easy to install, has all features, runs somewhat slow
* [LuaJIT](#install-with-luajit) — Slightly more complicated to install, missing a few features, runs fast
* [LuaJIT with compat for Lua 5.2](#install-with-52-compatible-luajit) — Tricky to install, has all features, runs fast

There are still instructions for all of these processes, and if you run into issues compiling on *nix I'm happy to help. If you're using Windows, I'd recommend sticking to the first option since that's simplest.

When I make binary releases, I'll use the third option (LuaJIT with 5.2+ compatibility).

### Install with normal Lua

1. Install [Python 3.7](https://www.python.org/downloads/) or later.
2. Download the repository: `git clone https://github.com/HactarCE/Cellua.git && cd Cellua`.
3. Set up a virtualenv: `python -m venv .env.lua53 && source .env.lua53/bin/activate`
3. Install Cellua: `pip install '.[dev]'`.

### Install with LuaJIT

**Warning: Using LuaJIT not compiled with `-DLUAJIT_ENABLE_LUA52COMPAT` will prevent access to certain features (e.g. `#pattern` and `pairs(pattern)`). Because of this, using LuaJIT without `-DLUAJIT_ENABLE_LUA52COMPAT` is _not_ officially supported.** See [**Install with 5.2-compatible LuaJIT**](#install-with-52-compatible-luajit).

To improve performance (or for testing purposes), build Lupa using LuaJIT:

1. Install [Python 3.7](https://www.python.org/downloads/) or later.
2. Download the repository: `git clone https://github.com/HactarCE/Cellua.git && cd cellua`.
3. Set up a virtualenv: `python -m venv .env.luajit205 && source .env.luajit205/bin/activate`
4. Install [Cython](https://cython.readthedocs.io/en/latest/src/quickstart/install.html), which is required in order to build Lupa with LuaJIT: `pip install Cython`.
6. Download and install [Lupa](https://github.com/scoder/lupa) elsewhere: `cd .. && git clone https://github.com/scoder/lupa.git && cd lupa && pip install .`.
7. Return to the original Cellua folder and install Cellua: `cd .. && pip install '.[dev]'`.

### Install with 5.2-compatible LuaJIT

While technically this doesn't give full Lua 5.2 compatibility, it's enough for now.

1. Install [Python 3.7](https://www.python.org/downloads/) or later.
2. Download the repository: `git clone https://github.com/HactarCE/Cellua.git && cd cellua`.
3. Set up a virtualenv: `python -m venv .env.luajit205 && source .env.luajit205/bin/activate`
2. Install [Cython](https://cython.readthedocs.io/en/latest/src/quickstart/install.html), which is required in order to build Lupa with LuaJIT: `pip install Cython`.
3. Download the [Lupa](https://github.com/scoder/lupa) repository: `git clone https://github.com/scoder/lupa.git && cd lupa`.
4. Download LuaJIT: `git clone https://github.com/LuaJIT/LuaJIT && cd LuaJIT`. (Do this inside of the `lupa` folder.)
5. Compile LuaJIT with Lua 5.2 compatibility: `make clean && make XCFLAGS=-DLUAJIT_ENABLE_LUA52COMPAT CFLAGS=-fPIC`.
6. Compile Lupa: `cd .. && make clean && make`.
7. Install Lupa: `pip install .`
8. Return to the original Cellua folder and install Cellua: `cd .. && pip install '.[dev]'`.

### Testing

Run `pytest` to run all tests.

### Building

1. Install Cellua: `pip install '.[dev, build]'`.
2. Run the build script: `./build.sh`.

## Known Issues

- Lua transition function can manipulate global state using closures.

## Planned Features

Most of these are not yet implemented. Not everything on this list will be implemented; `(?)` denotes those that are difficult, questionable, or at least low-priority, but anything on this list may be changed or removed.

### Editor

- [ ] Toggle mouse capture
- [ ] First-person movement
    - [ ] Vertical lock to axis X, Y, Z, or none (?)
        - [ ] I.e. yaw/pitch Euler angle control vs. free space rotation
    - [ ] Adjustable movement speed
    - [ ] "Rotate around point" movement (?)
    - [ ] Teleport to origin (?)
- [ ] 2D "layer" view (?)
- [ ] Cursor
    - [ ] Cursor positioning modes:
        - [ ] First nonzero cell
        - [ ] Distance-based
            - [ ] Adjustable cursor distance
        - [ ] Hybrid (default)
        - [ ] Adjacent to nonzero cell face
        - [ ] Adjacent to nonzero cell face/edge/vertex
- [ ] Basic controls
    - [ ] Place/remove cells
    - [ ] Pick cell with middle click
    - [ ] Undo/redo
    - [ ] Step/Run/"Run for X gens"/Stop
        - [ ] Step/Run/"Run for X gens" backwards for reversible automata (?)
    - [ ] Set generation count
    - [ ] Reset to start
- [ ] XYZ compass (corner of screen? cursor?)
- [ ] Show info about cell at cursor
    - [ ] Current state
    - [ ] Next state
    - [ ] Previous state(s?) (?)
    - [ ] Initial state
    - [ ] Show neighborhood
- [ ] Select cells
    - [ ] Shrink/expand/move/rotate rectangular selection
    - [ ] Copy & paste
        - [ ] Paste modes (like Golly)
        - [ ] Rotate when pasting
    - [ ] Shrink/expand/move/rotate cells (tile pattern when expanding)
    - [ ] Arbitrary selection (?)
        - [ ] Select by cell type (?)
        - [ ] Ellipsoid (?)
        - [ ] Combine prisms/ellipsoids (?)

### Renderer

- [ ] Optimized cubic rendering
- [ ] Render limit (fog?)
- [ ] Colors
    - [ ] Defined per side of cube (?)
    - [ ] Defined per state in rule
    - [ ] Adjustable during runtime (?)
- [ ] Textures on cube (?)
- [ ] Sphere rendering
    - [ ] Adjustable poly count / quality
- [ ] Arbitrary 3D models (?)
    - [ ] Defined per state in rule (?)
    - [ ] Adjustable during runtime (?)
- [ ] Shrink models
- [ ] Partial transparency (?)
- [ ] Text labels
- [ ] Grid visualization
    - [ ] Edges / vertices
    - [ ] Near cursor
    - [ ] Near cells
    - [ ] On cell faces
    - [ ] Everywhere
- [ ] Hide cells
- [ ] Looped universes (how? convenient fog?)
    - [ ] Render _everything_ in the looped universe, including cursor
- [ ] Custom background color/skybox
- [ ] Debug
    - [ ] Show chunk boundaries

### Simulator

- [ ] Basic controls (see **Editor**)
- [ ] Recenter pattern
- [ ] Save/load
- [ ] Arbitrary state count / cell size
    - [ ] 1 bit (2 states)
    - [ ] 8 bits (<=256 states)
    - [ ] 16 bits (<=65536 states) (?)
    - [ ] 32 bits (<=2^32 states) (?)
    - [x] 64 bits (<=2^64 states)
    - [ ] Lua table (?)
- [x] Arbitrary dimension count
    - [x] 1D
    - [x] 2D
    - [x] 3D
    - [x] 4D+ (?)
    - [ ] Time axis expressed as spatial dimension (?)
- [ ] Lua transition function
    - [ ] Neighborhoods
        - [x] Arbitrary range
        - [ ] Arbitrary bitmask
    - [ ] Symmetries (?)
        - [ ] Rotation
        - [ ] Rotation + reflection
        - [ ] X/Y/Z reflection
        - [ ] Rotation (180)
        - [ ] Rotation (180) + X/Y/Z reflection
        - [ ] Rotation (180) + reflection
        - [ ] Rotation (180) + two axes (?)
        - [ ] Rotation (90) around only one axis (?), with or without reflections (?)
        - [ ] Diagonal reflections (?)
        - [ ] Other?
        - [ ] User defined (?)
    - [ ] Error handling
    - [ ] Absolute time/space position dependency (?)
        - [ ] e.g. `B0` emulation (temporal parity)
        - [ ] e.g. Margolus neighborhood (spatiotemporal parity for X/Y)
    - [ ] Special support for time-reversable automata (?)
- [ ] Bounded grids
    - [ ] For each axis:
        - [x] Infinite
        - [ ] Finite
        - [ ] Loop
            - [ ] Reflect perpendicular axis #1
            - [ ] Reflect perpendicular axis #2
            - [ ] ???
    - [ ] Presets: infinite space, finite space, hypertorus
- [ ] Alternate tessellations (?)
    - [ ] Sphere packing
    - [ ] Rhombic dodecahedral (equivalent to checkerboard cubes)
    - [ ] Arbitrary 2D extruded (?)
- [ ] Optimizations
    - [ ] Multithreading (?)
    - [ ] Do not simulate cells with no nonzero neighbors
        - [ ] Verify that transition obeys this rule (?)
    - [ ] Result caching (?)
        - [ ] HashLife (?)

### Lua API

_For transition function features, see **Simulator**._

- [ ] Basic information
    - [ ] Simulation information
- [ ] Logging / display info to user
- [ ] Error handling
- [ ] GUI support (?)
- [ ] Keybind support (?)
- [ ] File save/load (how?)

## Non-features

These are things that Cellua will probably never support. Lua extensions may try to emulate them, however.

- "`B0`" rules (where a cell with no live neighbors becomes live)
- Infinite grids with nonzero default state
