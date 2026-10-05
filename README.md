import heapq

GOAL = (1, 2, 3,
        4, 5, 6,
        7, 8, 0)


# Misplaced Tiles heuristic
def h(state):
    return sum(1 for i in range(9)
               if state[i] != 0 and state[i] != GOAL[i])


def get_neighbors(state):
    neighbors = []
    zero = state.index(0)
    row, col = divmod(zero, 3)

    moves = [
        (-1, 0),  # Up
        (1, 0),   # Down
        (0, -1),  # Left
        (0, 1)    # Right
    ]

    for dr, dc in moves:
        nr, nc = row + dr, col + dc

        if 0 <= nr < 3 and 0 <= nc < 3:
            new_pos = nr * 3 + nc
            new_state = list(state)

            new_state[zero], new_state[new_pos] = \
                new_state[new_pos], new_state[zero]

            neighbors.append(tuple(new_state))

    return neighbors


def print_puzzle(state):
    for i in range(0, 9, 3):
        print(state[i:i+3])
    print()


def a_star(start):
    # (f, g, state, path)
    pq = []
    heapq.heappush(pq, (h(start), 0, start, [start]))

    visited = set()

    while pq:
        f, g, state, path = heapq.heappop(pq)

        if state in visited:
            continue

        visited.add(state)

        # Goal reached
        if state == GOAL:
            print("Solution found!")
            print("Number of moves:", g)
            print()

            for step in path:
                print_puzzle(step)

            return path

        # Generate successors
        for neighbor in get_neighbors(state):
            if neighbor not in visited:
                new_g = g + 1
                new_h = h(neighbor)
                new_f = new_g + new_h

                heapq.heappush(
                    pq,
                    (new_f, new_g, neighbor, path + [neighbor])
                )

    print("No solution exists.")
    return None


# Example input
start = (
    1, 2, 3,
    4, 0, 6,
    7, 5, 8
)

a_star(start)
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/6cbede5c-dc3f-453c-aeef-1038afbcd4fe" />
