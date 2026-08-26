from pyamaze import maze, agent, COLOR
from queue import Queue
from heapq import heappop, heappush  # For priority queue

# 1. Generate the maze
m = maze(10, 10)  # 10x10 maze
m.CreateMaze(x=1, y=1, loopPercent=70)  # Start at (1,1), with loops



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

# 4. A* algorithm
def AStar(m, start):
    def heuristic(cell, goal):
        # Manhattan distance heuristic
        return abs(cell[0] - goal[0]) + abs(cell[1] - goal[1])

    goal = (m.rows, m.cols)
    open_set = []  # Priority queue
    heappush(open_set, (0, start))  # (priority, cell)
    g_score = {start: 0}  # Cost from start to the current cell
    parent = {}
    visited = set()

    while open_set:
        _, current = heappop(open_set)

        if current in visited:
            continue
        visited.add(current)

        if current == goal:
            # Goal reached, reconstruct path
            path = {}
            cell = goal
            while cell != start:
                path[parent[cell]] = cell
                cell = parent[cell]
            return path, visited, parent

        # Get accessible neighbors (no walls)
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

                tentative_g_score = g_score[current] + 1  # Cost to move to the neighbor
                if next_cell not in g_score or tentative_g_score < g_score[next_cell]:
                    g_score[next_cell] = tentative_g_score
                    f_score = tentative_g_score + heuristic(next_cell, goal)
                    heappush(open_set, (f_score, next_cell))
                    parent[next_cell] = current

    print("Goal not reachable!")
    return {}, visited, parent

# 5. Choose which algorithm to run
start = (1, 1)

# Uncomment one of the following lines to use DFS, BFS, or A*:
# path, visited, parent = DFS(m, start)
# path, visited, parent = BFS(m, start)
path, visited, parent = AStar(m, start)

print(f"Visited cells: {visited}")
print(f"Parent dictionary: {parent}")
print(f"Path: {path}")

# Only trace the path if one was found and the goal is in the path
if path and (m.rows, m.cols) in path:
    a = agent(m, footprints=True, color=COLOR.red)
    m.tracePath({a: path})
else:
    print("No path found, agent will not be placed.")

# Visualize the maze and agent
m.run()
