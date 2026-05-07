# DSA in C++ — Interview Preparation Repository

> A personal, structured collection of Data Structures and Algorithms built for coding interview prep. Everything here is written from scratch with real explanations — not copy-pasted definitions. If you're grinding for placements or just want to actually *understand* DSA, this is the right place.

---

## Why This Repo Exists

Most DSA resources are either too theoretical (textbook-style) or too shallow (just code, no explanation). This repo tries to hit the middle ground — clean implementations with the kind of explanation you'd give a friend who's also preparing for interviews.

The focus is on **pattern recognition**, not memorization. Once you understand why an algorithm works, you can reconstruct it under pressure.

---

## Learning Roadmap

Follow this progression if you're starting from scratch. Don't skip ahead — each topic builds on the previous.

```
Phase 1 — Foundations (Week 1–2)
├── Sorting           → understand comparisons, complexity basics
├── Binary Search     → master the template, stop guessing loop conditions
└── Two Pointers      → array manipulation without extra space

Phase 2 — Core Patterns (Week 3–4)
├── Sliding Window    → optimize nested loops to linear time
├── Recursion         → think in subproblems before anything else
└── Linked List       → pointer manipulation, no shortcuts

Phase 3 — Data Structures (Week 5–6)
├── Stack and Queue   → monotonic stacks, BFS foundations
├── Trees             → traversals, recursion on trees, BST
└── Heap              → priority queues, top-K problems

Phase 4 — Advanced (Week 7–9)
├── Backtracking      → generate all possibilities, prune smartly
├── Graphs            → BFS, DFS, shortest paths, cycle detection
└── Dynamic Programming → the hardest and most rewarding topic
```

---

## Topics Covered

| # | Topic | Difficulty | Key Patterns |
|---|-------|-----------|--------------|
| 01 | [Sorting](./Sorting/README.md) | Beginner | Comparison sorts, STL sort |
| 02 | [Binary Search](./Binary%20Search/README.md) | Beginner–Mid | Search space reduction |
| 03 | [Sliding Window](./Sliding%20Window/README.md) | Beginner–Mid | Variable/fixed window |
| 04 | [Two Pointers](./Two%20Pointers/README.md) | Beginner–Mid | Opposite ends, fast-slow |
| 05 | [Recursion](./Recursion/README.md) | Mid | Base case + recurrence |
| 06 | [Backtracking](./Backtracking/README.md) | Mid–Hard | Choose, explore, unchoose |
| 07 | [Linked List](./Linked%20List/README.md) | Mid | Pointer tricks, two pointers |
| 08 | [Stack and Queue](./Stack%20and%20Queue/README.md) | Mid | Monotonic, BFS/DFS |
| 09 | [Trees](./Trees/README.md) | Mid–Hard | Traversals, recursion |
| 10 | [Heap](./Heap/README.md) | Mid–Hard | Priority queue, top-K |
| 11 | [Graphs](./Graphs/README.md) | Hard | BFS, DFS, Dijkstra |
| 12 | [Dynamic Programming](./Dynamic%20Programming/README.md) | Hard | Memoization, tabulation |

---

## Repository Structure

```
DSA/
│
├── Sorting/
│   └── README.md          # Bubble, Selection, Insertion, Merge, Quick, STL
│
├── Binary Search/
│   └── README.md          # Templates, search space, rotated arrays
│
├── Sliding Window/
│   └── README.md          # Fixed window, variable window patterns
│
├── Two Pointers/
│   └── README.md          # Opposite ends, fast-slow, sorted arrays
│
├── Recursion/
│   └── README.md          # Tree of calls, memoization intro
│
├── Backtracking/
│   └── README.md          # Subsets, permutations, N-Queens
│
├── Linked List/
│   └── README.md          # Traversal, reversal, cycle detection
│
├── Stack and Queue/
│   └── README.md          # Monotonic stack, deque, BFS
│
├── Trees/
│   └── README.md          # BST, traversals, LCA, path problems
│
├── Heap/
│   └── README.md          # Min/max heap, top-K, merge K lists
│
├── Graphs/
│   └── README.md          # BFS, DFS, Dijkstra, Union-Find
│
├── Dynamic Programming/
│   └── README.md          # 1D, 2D DP, classic problems
│
└── README.md              # This file
```

---

## Interview Preparation Focus

This repo is structured around how interviews actually work — not how textbooks teach DSA.

**What interviewers actually test:**
- Can you recognize *which pattern* to apply?
- Can you explain your approach before coding?
- Do you know the time/space complexity and can you justify it?
- Can you handle edge cases without being prompted?

**What this repo emphasizes:**
- Pattern recognition over problem memorization
- Complexity analysis baked into every explanation
- Edge cases called out explicitly
- Interview-specific tips in every topic

---

## How to Use This Repo

1. Follow the roadmap phases in order
2. Read the README for a topic first — understand the pattern
3. Try implementing from memory after reading
4. Look at the code only when stuck
5. Revisit the quick revision notes before interviews

---

## Language

All code is in **C++**. STL is used where appropriate (`vector`, `queue`, `priority_queue`, `unordered_map`, etc.) — same as what you'd use in actual interviews.

---

*If this helped your prep, consider starring ⭐ — it keeps me motivated to keep improving this.*