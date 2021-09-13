---
layout: post
title: "Visualisations of Procedural Maze Generation Algorithms in C"
tags: programming c imperial
---

The summer term of [Imperial's first year Computing curriculum](https://www.imperial.ac.uk/computing/current-students/computing/computing-first-year/) ended with a project written in C, completed in groups of four over the span of about three weeks. We were required to implement emulator and assembler programs for a subset of the [ARM instruction set architecture](https://developer.arm.com/documentation/), as well as an open-ended extension which was also required to be written in C.

Our extension was a visualiser for a number of procedural maze generation algorithms, each producing a distinct style of maze with differing solution path characteristics. The program visually conveys the differences between each method, which could aid the understanding of students who are learning the fundamental algorithms behind these generation functions.

For our work on the project, our group was presented with the 2020-21 ARM11 "Best Overall Project" award (with the prize money, Imperial now funds my addiction to [building mechanical keyboards](https://www.reddit.com/r/mechanicalkeyboards)). Our [presentation video](#video) was also shown at the College's [Undergraduate Virtual Open Week](https://www.imperial.ac.uk/events/136819/undergraduate-virtual-open-week-imperial360-live-department-of-computing/).

Imperial retains the copyright to the project specification, so I'm not permitted to publicly share or explain our group's code for the main project. However, as the extension is fully original work, I'm allowed (and have been encouraged) to share our code on a [public git repo](https://github.com/ethanrange/c-maze-generation).

![Visualisation of Prim's algorithm for maze generation. Countless hours were wasted staring at this mesmerising display.](https://github.com/ethanrange/c-maze-generation/raw/master/images/prims.gif){:standalone}

## Algorithm implementations
The algorithms are all derivatives of classical graph algorithms that Computing students cover earlier in the [Graphs and Algorithms module](https://www.imperial.ac.uk/computing/current-students/courses/40008/). The three maze generation algorithms we implemented were:
1. Randomised Prim's algorithm
2. Randomised Kruskal's algorithm
3. Recursive backtracking (randomised depth-first search algorithm)

The Wikipedia page on [maze generation algorithms](https://en.wikipedia.org/wiki/Maze_generation_algorithm) provides a concise overview of their implementations. These were fairly trivial to implement and were quickly completed within a single evening during a caffeine-fueled code sprint.

We also implemented a visualisation of [Dijkstra's pathfinding algorithm](https://en.wikipedia.org/wiki/Dijkstra%27s_algorithm), to show how these mazes may be solved. Again, this wasn't difficult to implement, but it was still immensely satisfying to watch the algorithm explore the passageways of our generated mazes, twisting through bends and corners to eventually find a path to the goal coordinate.

![Visualisation of Dijkstra's algorithm for maze-solving.](https://github.com/ethanrange/c-maze-generation/raw/master/images/dijkstras.gif){:standalone}

## Design
### Project structure
The purposes of each source file are as follows:
- `main.c` – Entry point containing the `main` function, which sets up the GUI and runs the visualiser loop.
- `visualiser.[c|h]` – Contains all the functions for handling user input, initiation of algorithms, and drawing
the visualiser.
- `prims.[c|h] kruskals.[c|h] recursive_backtrack.[c|h] dijkstras.[c|h]` – Implements the initialisation, stepping, and running functions for each respective algorithm.
- `utils.[c|h]` – Contains type definitions for `struct`s and implementations of functions common to every algorithm.
- `constants.h` – Stores useful constants for the visualiser in a single location for easy modification.

Due to the inherent parallels between the algorithms, we were able to take many common features between them (e.g sets, bounds checking, and neighbour identification) and convert them to generic functions, minimising redundant code.

![Dependency graph for the project.](/assets/images/maze-generation-dependencies.svg){:standalone width="90%"}

- Step, run, etc -> function pointers
- Flattening nested includes
- Raylib -> pain w/ makefiles

<iframe width="560" height="315" src="https://www.youtube-nocookie.com/embed/VV_Rhtc2ekA" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
{: #video}
