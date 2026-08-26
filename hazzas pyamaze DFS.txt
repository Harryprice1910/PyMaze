from pyamaze import maze, agent, COLOR
from queue import Queue

# 1. Generate the maze
m = maze(10, 10)  # 10x10 maze
m.CreateMaze(x=1, y=1, loopPercent=70)  # Start at (1,1), with loops

# 2. DFS algorithm
def DFS(m, start):
    stack = [start]
    visited = set()
    parent = {}
    goal = (m.rows, m.cols)
    found = False

    while stack:
        current = stack.pop()
        if current in visited:
            continue
        visited.add(current)

        if current == goal:
            found = True
            break

        # Get accessible neighbours (no walls)
        for direction in 'ESNW':
            if m.maze_map[current][direction] == 1:
                if direction == 'E':
                    next_cell = (current[0], current[1] + 1)
                elif direction == 'W':
                    next_cell = (current[0], current[1] - 1)
                elif direction == 'N':
                    next_cell = (current[0] - 1, current[1])
                elif direction == 'S':
                    next_cell = (current[0] + 1, current[1])

                if next_cell not in visited:
                    stack.append(next_cell)
                    parent[next_cell] = current

    # Reconstruct path only if goal was found
    path = {}
    if found:
        cell = goal
        rev_path = []
        while cell != start:
            rev_path.append(cell)
            cell = parent[cell]
        rev_path.append(start)
        rev_path = rev_path[::-1]  # reverse to get path from start to goal
        for i in range(len(rev_path) - 1):
            path[rev_path[i]] = rev_path[i + 1]
    else:
        print("Goal not reachable!")
    return path, visited, parent

# 3. BFS algorithm
def BFS(m, start):
    queue = [start]
    visited = set()
    parent = {}
    goal = (m.rows, m.cols)
    found = False

    while queue:
        current = queue.pop(0)  # Dequeue the first element
        if current in visited:
            continue
        visited.add(current)

        if current == goal:
            found = True
            break

        # Get accessible neighbours (no walls)
        for direction in 'ESNW':
            if m.maze_map[current][direction] == 1:
                if direction == 'E':
                    next_cell = (current[0], current[1] + 1)
                elif direction == 'W':
                    next_cell = (current[0], current[1] - 1)
                elif direction == 'N':
                    next_cell = (current[0] - 1, current[1])
                elif direction == 'S':
                    next_cell = (current[0] + 1, current[1])

                if next_cell not in visited and next_cell not in queue:
                    queue.append(next_cell)
                    parent[next_cell] = current

    # Reconstruct path only if goal was found
    path = {}
    if found:
        cell = goal
        while cell != start:
            path[parent[cell]] = cell
            cell = parent[cell]
    else:
        print("Goal not reachable!")
    return path, visited, parent

# 4. Choose which algorithm to run
start = (1, 1)

# Uncomment one of the following lines to use DFS or BFS:
# path, visited, parent = DFS(m, start)
path, visited, parent = BFS(m, start)

print(f"Visited cells: {visited}")
print(f"Parent dictionary: {parent}")
print(f"Path: {path}")

# Only trace the path if one was found
if path:
    a = agent(m, footprints=True, color=COLOR.red)
    m.tracePath({a: path})
else:
    print("No path found, agent will not be placed.")

# Visualize the maze and agent
m.run()
