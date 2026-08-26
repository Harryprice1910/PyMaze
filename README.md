

# PyMaze — Search & Pathfinding Algorithms

PyMaze is a Python project exploring classical artificial intelligence
search algorithms through maze generation and pathfinding.

The project implements and visualises multiple search strategies for
navigating procedurally generated mazes, allowing their behaviour and
resulting paths to be explored.

## Algorithms

The project currently implements:

- Depth-First Search (DFS)
- Breadth-First Search (BFS)
- A* Search

A* uses Manhattan distance as its heuristic for estimating the remaining
distance between the current cell and the goal.

## Features

- Generates configurable mazes using `pyamaze`
- Supports mazes containing loops and multiple possible routes
- Implements DFS using stack-based traversal
- Implements BFS using queue-based traversal
- Implements A* using a priority queue and heuristic search
- Tracks visited cells during traversal
- Maintains parent relationships for path reconstruction
- Reconstructs a successful route between start and goal
- Visualises the agent traversing the resulting path

## Technologies

- Python
- pyamaze
- heapq
- Python data structures including:
  - Lists / stacks
  - Queues
  - Sets
  - Dictionaries
  - Priority queues

## How It Works

A maze is generated using the `pyamaze` library. The search begins at
cell `(1, 1)` and attempts to reach the opposite corner of the maze.

Each search algorithm explores accessible neighbouring cells while
maintaining a record of visited states and parent relationships.

Once the goal is reached, the parent relationships are used to
reconstruct the path from the starting position to the goal.

The resulting route is then displayed using a visual agent within the
generated maze.

### Depth-First Search

DFS explores deeply along a route before backtracking. A stack is used
to determine which cell should be explored next.

### Breadth-First Search

BFS explores neighbouring cells level-by-level using a queue. In an
unweighted maze, this allows BFS to find a shortest path in terms of
number of moves.

### A* Search

A* combines the cost of reaching a cell with a heuristic estimate of
the remaining distance to the goal.

This implementation uses Manhattan distance:

`|current_row - goal_row| + |current_column - goal_column|`

A priority queue is used to explore cells according to their estimated
total path cost.

## Installation

Clone the repository:

    git clone <repository-url>
    cd PyMaze

Install the required dependency:

    pip install pyamaze

## Running the Project

Run one of the Python implementations:

   python src/dfs.py

or:

   python src/bfs.py

or:

   python src/astar.py

The generated maze will open in a visual window showing the path taken
by the agent.

## Future Improvements

Possible extensions to the project include:

- Benchmarking DFS, BFS and A* across multiple randomly generated mazes
- Comparing path length between algorithms
- Comparing the number of explored cells
- Measuring execution time
- Supporting different maze sizes and loop densities
- Refactoring the algorithms into reusable modules
- Adding automated tests
- Adding additional search algorithms

## Purpose

This project was developed to strengthen my understanding of classical
AI search techniques, algorithm design and Python data structures.

It demonstrates the practical differences between uninformed search
strategies such as DFS and BFS and heuristic search using A*.
