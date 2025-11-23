import copy

# Define the goal state (solved puzzle)
goal_state = [
    [1, 2, 3],
    [4, 5, 6],
    [7, 8, 0]
]

# Moves the blank tile can make: (row_change, col_change)
moves = {
    "up": (-1, 0),
    "down": (1, 0),
    "left": (0, -1),
    "right": (0, 1)
}

def find_blank(state):
    """Locate the position (row, col) of the blank tile (0)."""
    for i in range(3):
        for j in range(3):
            if state[i][j] == 0:
                return i, j
    return None  # Should never happen if blank tile always exists

def is_goal(state):
    """Check if the current state matches the goal state."""
    return state == goal_state

def copy_state(state):
    """Return a deep copy of the puzzle state."""
    return copy.deepcopy(state)

def print_state(state):
    """Print the state in a readable format."""
    for row in state:
        print(row)
    print()

def dls(state, depth_limit, visited):
    """Depth-Limited Search."""
    if is_goal(state):
        print("Goal state found:")
        print_state(state)
        return True

    if depth_limit == 0:
        return False

    blank_row, blank_col = find_blank(state)

    for move, (dr, dc) in moves.items():
        new_row, new_col = blank_row + dr, blank_col + dc
        # Check boundaries
        if 0 <= new_row < 3 and 0 <= new_col < 3:
            next_state = copy_state(state)
            # Swap blank with adjacent tile
            next_state[blank_row][blank_col], next_state[new_row][new_col] = next_state[new_row][new_col], next_state[blank_row][blank_col]

            # Convert to tuple of tuples for hashing
            state_tuple = tuple(tuple(row) for row in next_state)

            if state_tuple not in visited:
                visited.add(state_tuple)
                if dls(next_state, depth_limit - 1, visited):
                    return True
                visited.remove(state_tuple)

    return False

def ids(initial_state, max_depth=20):
    """Iterative Deepening Search."""
    initial_tuple = tuple(tuple(row) for row in initial_state)

    for depth in range(max_depth + 1):
        print(f"Searching at depth: {depth}")
        visited = set()
        visited.add(initial_tuple)
        if dls(initial_state, depth, visited):
            print(f"Goal found at depth {depth}")
            return True
    print("Goal not found within max depth limit.")
    return False

if __name__ == "__main__":
    # Example initial state (can be modified)
    initial_state = [
        [1, 2, 3],
        [4, 0, 6],
        [7, 5, 8]
    ]

    ids(initial_state)
