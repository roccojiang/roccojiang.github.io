---
layout: post
title: "Visualisations of Procedural Maze Generation Algorithms in C"
tags: programming c imperial
---

The summer term of [Imperial's first year Computing curriculum](https://www.imperial.ac.uk/computing/current-students/computing/computing-first-year/) ended with a project written in C, completed in groups of four over the span of about three weeks. We were required to implement emulator and assembler programs for a subset of the [ARM instruction set architecture](https://developer.arm.com/documentation/), as well as an open-ended extension which was also required to be written in C.

Our extension was a visualiser for a number of procedural maze generation algorithms, each producing a distinct style of maze with differing solution path characteristics. The program visually conveys the differences between each method, which could aid the understanding of students who are learning the fundamental algorithms behind these generation functions.

For our work on the project, our group was presented with the 2020-21 ARM11 "Best Overall Project" award (with the prize money, Imperial now funds my addiction to building mechanical keyboards). Our [presentation video](#video) was also shown at the College's [Undergraduate Virtual Open Week](https://www.imperial.ac.uk/events/136819/undergraduate-virtual-open-week-imperial360-live-department-of-computing/).

<!-- excerpt-end -->

Imperial retains the copyright to the project specification, so I'm not permitted to publicly share or explain our group's code for the main project. However, as the extension is fully original work, I'm allowed to share our code on a [public git repo](https://github.com/ethanrange/c-maze-generation).

![Visualisation of Prim's algorithm for maze generation. Countless hours were wasted staring at this mesmerising display.](https://github.com/ethanrange/c-maze-generation/raw/master/images/prims.gif){:standalone}

## Algorithm implementations
The algorithms are all derivatives of classical graph algorithms that Computing students cover earlier in the [Graphs and Algorithms module](https://www.imperial.ac.uk/computing/current-students/courses/40008/). The three maze generation algorithms we implemented were:
1. Randomised Prim's algorithm
2. Randomised Kruskal's algorithm
3. Recursive backtracking (randomised depth-first search algorithm)

The Wikipedia page on [maze generation algorithms](https://en.wikipedia.org/wiki/Maze_generation_algorithm) provides a concise overview of their implementations. These were fairly trivial to implement and were quickly completed within a single evening during a caffeine-fueled code sprint.

We also implemented a visualisation of [Dijkstra's pathfinding algorithm](https://en.wikipedia.org/wiki/Dijkstra%27s_algorithm), to show how these mazes may be solved. Again, this wasn't difficult to implement, but it was still immensely satisfying to watch the algorithm explore the passageways of our generated mazes, twisting through bends and corners in search of a path to the goal coordinate.

![Visualisation of Dijkstra's algorithm for maze-solving.](https://github.com/ethanrange/c-maze-generation/raw/master/images/dijkstras.gif){:standalone}

## Project structure
The purposes of each source file are as follows:
- `main.c` – Entry point containing the `main` function, which sets up the GUI and runs the visualiser loop.
- `visualiser.[c|h]` – Contains all the functions for handling user input, initiation of algorithms, and drawing
the visualiser.
- `prims.[c|h] kruskals.[c|h] recursive_backtrack.[c|h] dijkstras.[c|h]` – Implements the initialisation, stepping, and running functions for each respective algorithm.
- `utils.[c|h]` – Contains type definitions for `struct`s and implementations of functions common to every algorithm.
- `constants.h` – Stores useful constants for the visualiser in a single location for easy modification.

Due to the inherent parallels between the algorithms, we were able to take many common features between them (e.g sets, bounds checking, and neighbour identification) and convert them to generic functions, minimising redundant code.

![Dependency graph for the project.](/assets/images/maze-generation-dependencies.svg){:standalone width="90%"}

### Use of function pointers
If you were to look at the source files for each algorithm, you'd notice that they mostly follow a very similar structure containing `init`, `step`, and `run` functions. Take `prims.h` for example (I've omitted some of the documentation for brevity):

```c
/* Initialises the required variables for Prim's algorithm. */
void init_prims(Tile tiles[ROWS][COLS], Player player);

/* Performs a single iteration of Prim's algorithm. */
int step_prims(Tile tiles[ROWS][COLS]);

/* Runs the randomised Prim's algorithm. */
bool run_prims(Tile tiles[ROWS][COLS]);
```

The only algorithm that deviates from this format is the recursive backtrack, which doesn't have `init` and `step` functions since the implementation uses a recursive approach, rather than the iterative approach which allowed step-by-step visualisation for the other algorithms.

This kind of regularity meant that we could take advantage of [function pointers](https://www.cprogramming.com/tutorial/function-pointers.html) – one of the features of C that I find really cool and elegant. If we take a look at `visualiser.h` and `visualiser.c`, we declare three arrays containing function pointers to each algorithm's `init`, `step`, and `run` functions:

```c
typedef void (*InitFuncPtr)(Tile[ROWS][COLS], Player);
typedef int  (*StepFuncPtr)(Tile[ROWS][COLS]);
typedef bool (*RunFuncPtr)(Tile[ROWS][COLS]);

/* Function pointers for stepping, initialising, and running algorithms. */
static const InitFuncPtr init_functions[5] = {NULL, init_prims, init_kruskals,
                                              NULL, init_dijkstras};
static const StepFuncPtr step_functions[5] = {NULL, step_prims, step_kruskals,
                                              NULL, step_dijkstras};
static const RunFuncPtr run_functions[5]   = {NULL, run_prims, run_kruskals, 
                                              NULL, run_dijkstras};
```

These arrays store the corresponding function pointer for each algorithm, indexed by the five possible visualiser states as seen in `utils.h`:

```c
/** Represents the current state (halted or an algorithm) of the visualiser. */
typedef enum RunState {
  HALT,
  PRIMS,
  KRUSKALS,
  RECURSIVE_BACKTRACK,
  DIJKSTRAS
} RunState;
```

The handling of multiple algorithms essentially boils down to a simple lookup from an array of function pointers. We see this in the delightfully succinct handling of the `run` functions in `visualiser.c` (`init` and `step` functions are handled in a similar manner):

```c
/** Runs the given algorithm. */
static bool run_algorithm(RunState type) {
  // Look up run function pointer from the given algorithm type
  if (run_functions[type]) {
    SetWindowTitle(run_messages[type]);
    return (*run_functions[type])(tiles);
  }

  // Return if unsuccessful
  return false;
}
```

If we were to add more algorithms to the visualiser, we'd merely need to update the function pointer arrays, rather than combing through ugly switch statements in multiple sections of `visualiser.c`.

#### One slight hitch
The recursive backtracking algorithm, being the black sheep of the family, unfortunately complicates things a little as it doesn't fall into that neat `init`-`step`-`run` pattern. Thus it leaves a blemish on our otherwise beautifully abstracted code:

```c
/** Initialises and runs the given algorithm. */
static bool handle_algorithm(RunState type) {
  // Initialise algorithm via function pointers
  init_algorithm(type);

  // Handle recursive backtrack separately
  if (type == RECURSIVE_BACKTRACK) {
    SetWindowTitle(run_messages[type]);
    run_recursive_backtrack(tiles, player);
  } else {
    // Some other code handling visualiser state
    ...

    // Run algorithm via function pointers
    return run_algorithm(type);
  }

  // Return unsucessful
  return false;
}
```

But, it's still much nicer than if we had to handle every algorithm separately with long and painfully verbose switch statements.

## Noteworthy challenges
By the time we started work on the extension, our group was well acquainted with C as we had already completed most of the main project by then. Thus, work on the visualiser went rather smoothly, but there are still some challenges we ran into which I felt was worth mentioning (as well as some small annoyances which I just wanted to complain about).

### Flattening nested includes

### Pain with Makefiles

### We kept messing up with git


<iframe width="560" height="315" src="https://www.youtube-nocookie.com/embed/VV_Rhtc2ekA" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
{: #video}
