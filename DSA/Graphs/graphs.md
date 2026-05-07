# Graphs in C++ — Beginner to Advanced Interview Level

> My notes after grinding through 100+ graph problems on LeetCode. Graphs used to scare me — all those edges and components. Now they feel like one of the most powerful tools in my DSA kit. This is the guide I wish I had when I was stuck on "Number of Islands" for hours.

---

## Table of Contents

- [Introduction to Graphs](#introduction-to-graphs)
- [Graph Representation](#graph-representation)
- [Graph Traversal](#graph-traversal)
- [How to Identify Graph Problems](#how-to-identify-graph-problems)
- [Core Graph Topics](#core-graph-topics)
- [Graph Thinking Mental Models](#graph-thinking-mental-models)
- [Common Beginner Mistakes](#common-beginner-mistakes)
- [Real-world Usage](#real-world-usage)
- [Practice Problems](#practice-problems)
- [Quick Revision Cheat Sheet](#quick-revision-cheat-sheet)

---

## Introduction to Graphs

Graphs are everywhere once you start seeing them. At its core, a graph is just a set of **nodes** (vertices) connected by **edges**.

Think of it as a social network: people are nodes, friendships are edges. Or road maps: cities are nodes, roads are edges.

### Key Variations

- **Directed vs Undirected**: One-way streets vs two-way roads
- **Weighted vs Unweighted**: Roads with distances vs just connections
- **Cyclic vs Acyclic**: Has loops or not (huge for topological sort)
- **Connected vs Disconnected**: One big component or multiple islands

**Practical observation**: Most interview problems use undirected unweighted graphs or grids (which are implicit graphs).

---

## Graph Representation

### 1. Adjacency Matrix

2D array where `matrix[i][j] = 1` means edge from i to j.

```cpp
vector<vector<int>> adj(n, vector<int>(n, 0));
// Add edge
adj[u][v] = 1;
```

**Pros**: Fast edge check (O(1))  
**Cons**: O(n²) space — bad for sparse graphs

### 2. Adjacency List (Most Common in Interviews)

```cpp
vector<vector<int>> adj(n);
adj[u].push_back(v);  // for directed
adj[v].push_back(u);  // for undirected
```

**Why I prefer this**: 
- Space efficient for sparse graphs (real-world networks)
- Easy to traverse
- Interviewers expect this

**Tip**: For weighted graphs, use `vector<vector<pair<int,int>>>` (node, weight).

---

## Graph Traversal

### Breadth First Search (BFS)

**Intuition**: Explore level by level, like ripples in a pond. Perfect for shortest path in unweighted graphs.

**Queue + Visited** is the key.

```cpp
vector<bool> visited(n, false);
queue<int> q;

q.push(start);
visited[start] = true;

while (!q.empty()) {
    int node = q.front(); q.pop();
    // process node
    
    for (int nei : adj[node]) {
        if (!visited[nei]) {
            visited[nei] = true;
            q.push(nei);
        }
    }
}
```

**Dry Run Example**: Imagine a simple line graph 0-1-2-3. BFS visits level by level.

**Time**: O(V + E)  
**Space**: O(V)

**Best for**: Shortest path, level order, minimum steps.

### Depth First Search (DFS)

**Intuition**: Go as deep as possible down one path before backtracking. Like exploring a maze by always going forward until stuck.

**Recursive version** (cleaner in most cases):

```cpp
void dfs(int node, vector<vector<int>>& adj, vector<bool>& visited) {
    visited[node] = true;
    // process node
    
    for (int nei : adj[node]) {
        if (!visited[nei]) {
            dfs(nei, adj, visited);
        }
    }
}
```

**Iterative DFS** uses stack (same as recursive call stack).

**Time**: O(V + E)  
**Space**: O(V) + recursion stack

**When to choose**:
- BFS: shortest path, levels
- DFS: connectivity, cycles, topological sort

---

## How to Identify Graph Problems

Look for these signals:

- "Connected components", "islands", "networks"
- "Shortest path", "minimum steps"
- "Cycle detection", "dependencies"
- Grid problems with 4-directional movement
- "Can I reach from A to B?"
- Scheduling, prerequisites (topological sort)

**Pro tip**: Any time you see "nodes" and "edges" or movement on grid → model it as graph.

---

## Core Graph Topics

### 1. Connected Components

Count how many separate groups exist.

Use DFS or BFS on every unvisited node.

### 2. Cycle Detection

**Undirected**: If you visit a node that's already visited and not parent → cycle.

**Directed**: Use recursion stack (coloring: white-gray-black) or topological sort failure.

### 3. Topological Sort

For DAGs (Directed Acyclic Graphs).

- **DFS**: Post-order finishing times
- **BFS (Kahn’s)**: Use indegree + queue

**Kahn’s Algorithm** (very common):

```cpp
vector<int> topoSort(int n, vector<vector<int>>& adj) {
    vector<int> indegree(n, 0);
    for (int i = 0; i < n; i++) {
        for (int nei : adj[i]) indegree[nei]++;
    }
    
    queue<int> q;
    for (int i = 0; i < n; i++) {
        if (indegree[i] == 0) q.push(i);
    }
    
    vector<int> order;
    while (!q.empty()) {
        int node = q.front(); q.pop();
        order.push_back(node);
        
        for (int nei : adj[node]) {
            indegree[nei]--;
            if (indegree[nei] == 0) q.push(nei);
        }
    }
    return (order.size() == n) ? order : vector<int>{}; // cycle if not all nodes
}
```

### 4. Shortest Path

- **Unweighted**: BFS
- **Weighted (non-negative)**: Dijkstra
- **Negative weights**: Bellman-Ford
- **All pairs**: Floyd-Warshall

**Dijkstra** (priority_queue version):

```cpp
vector<int> dijkstra(int src, vector<vector<pair<int,int>>>& adj) {
    vector<int> dist(n, INT_MAX);
    dist[src] = 0;
    priority_queue<pair<int,int>, vector<pair<int,int>>, greater<>> pq;
    pq.push({0, src});
    
    while (!pq.empty()) {
        auto [cost, u] = pq.top(); pq.pop();
        if (cost > dist[u]) continue;
        
        for (auto [v, w] : adj[u]) {
            if (dist[u] + w < dist[v]) {
                dist[v] = dist[u] + w;
                pq.push({dist[v], v});
            }
        }
    }
    return dist;
}
```

### 5. Minimum Spanning Tree

**Kruskal** (with DSU) — sort edges + union

**Prim** — grow tree from one vertex using priority queue

### 6. Union Find (DSU)

Super useful for cycle detection, MST, connected components.

```cpp
class DSU {
public:
    vector<int> parent, rank;
    DSU(int n) {
        parent.resize(n);
        rank.resize(n, 0);
        for (int i = 0; i < n; i++) parent[i] = i;
    }
    int find(int x) {
        if (parent[x] != x) parent[x] = find(parent[x]);
        return parent[x];
    }
    bool unite(int x, int y) {
        int px = find(x), py = find(y);
        if (px == py) return false;
        if (rank[px] < rank[py]) swap(px, py);
        parent[py] = px;
        if (rank[px] == rank[py]) rank[px]++;
        return true;
    }
};
```

### 7. Grid as Graph

Treat each cell as node, 4/8 directions as edges.

Classic: Number of Islands (`dfs` to mark visited).

---

## Graph Thinking Mental Models

- **Everything is a graph** — grids, prerequisites, social connections
- **Visited array is sacred** — forgetting it causes infinite loops
- BFS = shortest / level order
- DFS = deep exploration + backtracking
- Parent tracking helps avoid going back immediately in undirected graphs

---

## Common Beginner Mistakes

- Forgetting visited → infinite loop in DFS/BFS
- Not handling disconnected graphs (must loop over all nodes)
- Wrong direction in directed graphs
- Using queue for DFS or stack for BFS by mistake
- Not using parent in undirected cycle detection
- TLE from not using fast input or bad priority_queue in Dijkstra

---

## Real-world Usage

- GPS & navigation (shortest path)
- Social networks (recommendations, friend suggestions)
- Build systems & task scheduling (topological sort)
- Internet routing
- Game AI (pathfinding)
- Recommendation engines
- Circuit design

---

## Practice Problems

**Easy**:
- Number of Islands
- Flood Fill
- Clone Graph

**Intermediate**:
- Course Schedule (Topological + Cycle)
- Rotting Oranges
- Word Ladder

**Advanced**:
- Word Ladder II
- Shortest Path in Binary Matrix
- Alien Dictionary
- Network Delay Time
- Minimum Cost to Connect All Points (MST)

**Must Do LeetCode**:
- 200, 207, 210, 133, 994, 127, 785, 743, 1584

---

## Quick Revision Cheat Sheet

```
Traversal:
- BFS → Queue + shortest path
- DFS → Recursion/Stack + components & cycles

Algorithms:
- Topological Sort → Kahn or DFS
- Shortest Path → BFS (unweighted) / Dijkstra
- MST → Kruskal (DSU) or Prim
- Cycle → DFS colors or Kahn failure

DSU Tips:
- Path compression + Union by rank
```

**Interview Checklist**:
- Is the graph directed/undirected/weighted?
- Handle disconnected components
- Clear visited array
- Discuss time & space (O(V+E))

---

*Built after many late-night debugging sessions. If this helped, star the repo!*

Happy Graphing! 🕸️
