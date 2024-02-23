# Assignment 1 : Question 6
|Std ID|Name|
|------|-|
|K228708|Arman Faisal|
|K224102|Rayan Farhan|

#Q6

from queue import PriorityQueue

romania_map = {
    'Arad': {'Zerind': 75, 'Sibiu': 140, 'Timisoara': 118},
    'Zerind': {'Arad': 75, 'Oradea': 71},
    'Oradea': {'Zerind': 71, 'Sibiu': 151},
    'Sibiu': {'Arad': 140, 'Oradea': 151, 'Fagaras': 99, 'Rimnicu Vilcea': 80},
    'Timisoara': {'Arad': 118, 'Lugoj': 111},
    'Lugoj': {'Timisoara': 111, 'Mehadia': 70},
    'Mehadia': {'Lugoj': 70, 'Drobeta': 75},
    'Drobeta': {'Mehadia': 75, 'Craiova': 120},
    'Craiova': {'Drobeta': 120, 'Rimnicu Vilcea': 146, 'Pitesti': 138},
    'Rimnicu Vilcea': {'Sibiu': 80, 'Craiova': 146, 'Pitesti': 97},
    'Fagaras': {'Sibiu': 99, 'Bucharest': 211},
    'Pitesti': {'Rimnicu Vilcea': 97, 'Craiova': 138, 'Bucharest': 101},
    'Bucharest': {'Fagaras': 211, 'Pitesti': 101, 'Giurgiu': 90, 'Urziceni': 85},
    'Giurgiu': {'Bucharest': 90},
    'Urziceni': {'Bucharest': 85, 'Vaslui': 142, 'Hirsova': 98},
    'Vaslui': {'Urziceni': 142, 'Iasi': 92},
    'Iasi': {'Vaslui': 92, 'Neamt': 87},
    'Neamt': {'Iasi': 87}
}

heuristics = {
    'Arad': 366,
    'Zerind': 374,
    'Oradea': 380,
    'Sibiu': 253,
    'Timisoara': 329,
    'Lugoj': 244,
    'Mehadia': 241,
    'Drobeta': 242,
    'Craiova': 160,
    'Rimnicu Vilcea': 193,
    'Fagaras': 178,
    'Pitesti': 98,
    'Bucharest': 0,
    'Giurgiu': 77,
    'Urziceni': 80,
    'Vaslui': 199,
    'Iasi': 226,
    'Neamt': 234
}

def breadth_first_search(graph, start, goal):
    visited = set()
    queue = [[start]]
    while queue:
        path = queue.pop(0)
        node = path[-1]
        if node not in visited:
            neighbors = graph[node]
            for neighbor in neighbors:
                new_path = list(path)
                new_path.append(neighbor)
                queue.append(new_path)
                if neighbor == goal:
                    return new_path
            visited.add(node)

def uniform_cost_search(graph, start, goal):
    pq = PriorityQueue()
    pq.put((0, [start]))
    while not pq.empty():
        cost, path = pq.get()
        node = path[-1]
        if node == goal:
            return path
        for neighbor in graph[node]:
            new_cost = cost + graph[node][neighbor]
            new_path = list(path)
            new_path.append(neighbor)
            pq.put((new_cost, new_path))

def greedy_best_first_search(graph, start, goal, heuristic):
    visited = set()
    pq = PriorityQueue()
    pq.put((heuristic[start], [start]))
    while not pq.empty():
        _, path = pq.get()
        node = path[-1]
        if node not in visited:
            visited.add(node)
            if node == goal:
                return path
            for neighbor in graph[node]:
                if neighbor not in visited:
                    new_path = list(path)
                    new_path.append(neighbor)
                    pq.put((heuristic[neighbor], new_path))

def iterative_deepening_dfs(graph, start, goal, depth_limit):
    for depth in range(depth_limit):
        visited = set()
        stack = [(start, [start])]
        while stack:
            node, path = stack.pop()
            if node not in visited:
                visited.add(node)
                if node == goal:
                    return path
                if len(path) <= depth:
                    for neighbor in graph[node]:
                        stack.append((neighbor, path + [neighbor]))

source = 'Arad'
destination = 'Bucharest'

bfs_path = breadth_first_search(romania_map, source, destination)
print("Breadth First Search Path:", bfs_path)

ucs_path = uniform_cost_search(romania_map, source, destination)
print("Uniform Cost Search Path:", ucs_path)

gbfs_path = greedy_best_first_search(romania_map, source, destination, heuristics)
print("Greedy Best First Search Path:", gbfs_path)

iddfs_path = iterative_deepening_dfs(romania_map, source, destination, 10)
print("Iterative Deepening Depth First Search Path:", iddfs_path)

