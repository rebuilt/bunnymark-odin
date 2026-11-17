# bunnymark-odin

A port of the classic raylib [bunnymark](https://www.raylib.com/examples.html) benchmark to
[Odin](https://odin-lang.org/).

The benchmark fills the screen with a swarm of bouncing bunnies, adding 100 new ones every
frame while you hold the left mouse button. It is a simple way to measure raw 2D drawing
throughput: the more bunnies you can push, the better.

`bunnymark.c` is the original raylib C example, kept for reference. `main.odin` is the Odin
version.

## Requirements

- [Odin](https://odin-lang.org/docs/install/)
- raylib, which ships with Odin as the `vendor:raylib` package, plus the system libraries it
  needs (on Linux typically X11/GL development packages)

## Build and run

```sh
./compile
./main
```

`compile` runs:

```sh
odin build main.odin -file -o:speed -out:main
```

The working directory matters: the program loads its textures from `resources/`, so run it
from the repository root.

## Controls

| Input              | Action                       |
| ------------------ | ---------------------------- |
| Left mouse button  | Spawn 100 bunnies per frame  |
| `P`                | Pause/unpause the simulation |
| `Esc`              | Quit                         |

## What you will see

The top bar shows:

- **FPS** – the current framerate, drawn in the top-left corner
- **bunnies** – how many bunnies are currently alive
- **batched draw calls** – how many internal raylib batches are being flushed
  (`1 + bunnies / 8192`). When the internal vertex buffer limit is reached raylib issues a draw
  call and the CPU buffer is uploaded to the GPU, which is where framerate starts to drop.

## Notes on the Odin port

- `MAX_BUNNIES` is 200,000 and `MAX_BATCH_ELEMENTS` is 8192, matching the C original.
- Bunnies are stored in a slice allocated up front with `make` and released with `defer delete`.
- The simulation is time-based (`rl.GetFrameTime()`), so bunny speed is independent of framerate.
- Unlike the C example the window title bar border is not accounted for beyond the original
  `- 40` offset used for the top bar.

## License

The original bunnymark example (`bunnymark.c`) is licensed under the zlib/libpng license,
Copyright (c) 2014-2025 Ramon Santamaria (@raysan5). See the header of `bunnymark.c` for
details.
