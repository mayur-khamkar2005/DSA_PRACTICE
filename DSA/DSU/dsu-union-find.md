# Disjoint Set Union (DSU) / Union Find in C++ — Beginner to Advanced Interview Level

> Personal notes + optimized implementations after solving 150+ graph problems on LeetCode, Codeforces & AtCoder. From "what the hell is a parent array" to crushing Kruskal, cycle detection, and offline queries. If DSU used to confuse you — this README will make it click forever.

![C++](https://img.shields.io/badge/C%2B%2B-17%2B-blue) ![Graph](https://img.shields.io/badge/Graph-DSU%2FUFA-orange) ![Interview](https://img.shields.io/badge/Interview-Ready-brightgreen)

---

## Table of Contents

- [What is DSU / Union Find?](#what-is-dsu--union-find)
- [Real-World Intuition](#real-world-intuition)
- [Core Idea: Parent Array](#core-idea-parent-array)
- [Find Operation](#find-operation)
- [Union Operation](#union-operation)
- [Path Compression (The Magic)](#path-compression-the-magic)
- [Union by Rank vs Union by Size](#union-by-rank-vs-union-by-size)
- [Complete Optimized Implementation](#complete-optimized-implementation)
- [Applications](#applications)
- [Kruskal’s Algorithm](#kruskals-algorithm)
- [Cycle Detection in Undirected Graph](#cycle-detection-in-undirected-graph)
- [Connected Components](#connected-components)
- [DSU on Trees / Offline Queries](#dsu-on-trees--offline-queries)
- [Common Beginner Mistakes](#common-beginner-mistakes)
- [Interview Patterns & Tips](#interview-patterns--tips)
- [Practice Problems (Curated)](#practice-problems-curated)
- [Final Cheat Sheet](#final-cheat-sheet)

---

## What is DSU / Union Find?

**Disjoint Set Union (DSU)** is a data structure that keeps track of **partitioned sets** and allows you to:
- Merge two sets (Union)
- Check if two elements belong to the same set (Find)

It is incredibly fast — almost **O(1)** per operation with optimizations.

---

## Real-World Intuition

Imagine you have **friends groups** (social network):

- Initially everyone is in their own group.
- When A becomes friend with B → merge their groups.
- When someone asks "Are X and Y friends (directly or indirectly)?" → check if they have the same "leader".

DSU efficiently manages these dynamic groupings.

---

## Core Idea: Parent Array

```cpp
vector<int> parent;
```

- `parent[i] = i` means i is its own leader initially.
- Each element points to its parent.
- The **root** (leader) of a set has `parent[root] == root`.

**Visual Example (Initial State - 7 nodes)**:

```
0 1 2 3 4 5 6
↓ ↓ ↓ ↓ ↓ ↓ ↓
0 1 2 3 4 5 6   ← each is its own parent
```

---

## Find Operation

**Goal**: Find the ultimate leader (root) of a node.

### With Path Compression

```cpp
int find(int x) {
    if (parent[x] != x)
        parent[x] = find(parent[x]);   // Path Compression
    return parent[x];
}
```

---

## Path Compression — Deep Visual Explanation

**Before** (long chain):

```
0 → 1 → 2 → 3 → 4 → 5 → 6 (root)
```

**After `find(0)`**:

```
0 → 6
1 → 6  
2 → 6
3 → 6
4 → 6
5 → 6
6 → 6
```

Every find operation flattens the tree, making future finds blazing fast.

---

## Union by Rank + Path Compression (Standard)

```cpp
class DSU {
public:
    vector<int> parent, rankk;
    
    DSU(int n) {
        parent.resize(n+1);
        rankk.resize(n+1, 0);
        for(int i=0; i<=n; i++) parent[i] = i;
    }
    
    int find(int x) {
        if(parent[x] != x)
            parent[x] = find(parent[x]);
        return parent[x];
    }
    
    void unionSets(int a, int b) {
        a = find(a);
        b = find(b);
        if(a == b) return;
        
        if(rankk[a] < rankk[b]) swap(a,b);
        parent[b] = a;
        if(rankk[a] == rankk[b]) rankk[a]++;
    }
};
```

---

## Applications

- Cycle Detection
- Kruskal’s MST
- Connected Components
- Accounts Merge, Redundant Connection, etc.

---

## Practice Problems

**Easy**: Number of Provinces (547)  
**Medium**: Redundant Connection (684), Accounts Merge (721)  
**Hard**: Largest Component Size by Common Factor (952)

---

## Final Cheat Sheet

- `find(x)` → root with compression
- `unionSets(a,b)` → merge
- `find(u) == find(v)` → same component

**Time**: Nearly O(1) per operation

---

*Built while grinding Graph tag. DSU is now my superpower.*

**Star ⭐ if it helped!**
