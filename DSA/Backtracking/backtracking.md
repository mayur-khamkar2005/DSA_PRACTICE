# Backtracking in C++ — Beginner to Interview Level

> My raw notes after spending way too many nights debugging recursion stacks and forgetting to undo choices. This is the guide that finally made backtracking click for me.

---

## Table of Contents

- [What is Backtracking, Really?](#what-is-backtracking-really)
- [Why Backtracking Matters](#why-backtracking-matters)
- [Recursion vs Backtracking](#recursion-vs-backtracking)
- [Decision Tree Thinking](#decision-tree-thinking)
- [Choose → Explore → Undo Pattern](#choose-explore-undo-pattern)
- [How to Identify Backtracking Problems](#how-to-identify-backtracking-problems)
- [Base Case Understanding](#base-case-understanding)
- [State Space Exploration](#state-space-exploration)
- [Pruning Basics](#pruning-optimization-basics)
- [Subsets Problems](#subsets-problems)
- [Permutations Problems](#permutations-problems)
- [Combination Sum Problems](#combination-sum-problems)
- [N-Queens](#n-queens)
- [Sudoku Solver](#sudoku-solver-basics)
- [Comparison Table](#comparison-table)
- [Common Beginner Mistakes](#common-beginner-mistakes)
- [Interview Insights](#interview-insights)
- [Real-World Usage](#real-world-usage)
- [When NOT to Use Backtracking](#when-backtracking-should-not-be-used)
- [Quick Revision Cheat Sheet](#quick-revision-cheat-sheet)

---

## What is Backtracking, Really?

Backtracking is recursion with a twist — you try different choices, go deep, and if it doesn't work, you **backtrack** (undo your choice) and try the next option.

It's basically brute force with smart pruning.

### Simple Intuition
Imagine you're solving a maze. You go down a path, hit a dead end, go back to the last junction and try another way. That's backtracking.

---

## Why Backtracking Matters

- Solves problems where you need to find **all possible** arrangements or combinations
- Foundation for many hard LeetCode problems
- Teaches clean recursive thinking
- Shows up in interviews when they want to test recursion + state management

---

## Recursion vs Backtracking

| Aspect              | Recursion                     | Backtracking                          |
|---------------------|-------------------------------|---------------------------------------|
| Purpose             | Break problem into subproblems| Explore all possible configurations  |
| State Modification  | Usually doesn't undo          | Must undo choices (backtrack)        |
| Goal                | Compute a result              | Find valid solutions / all solutions |
| Typical Use         | Tree traversal, DP            | Subsets, Permutations, N-Queens      |

Backtracking **is** recursion, but with explicit undo steps.

---

## Decision Tree Thinking

Every backtracking problem can be visualized as a **decision tree**:

- Each level = one decision (choose or not choose, pick a number, place a queen, etc.)
- Each branch = a possible choice
- Leaves = complete solutions or dead ends

**Example (Subsets of [1,2,3]):**

```
          []
       /   |   \
     [1]   [2]   [3]
    /  \     |
  [1,2] [1,3] [2,3]
   |
 [1,2,3]
```

Understanding this tree helps you write the code naturally.

---

## Choose → Explore → Undo Pattern (The Golden Template)

```cpp
void backtrack(...) {
    // Base case
    if (isSolution()) {
        addToResult();
        return;
    }
    
    for (each possible choice) {
        // Choose
        makeChoice();
        
        // Explore
        backtrack(...);
        
        // Undo (Backtrack)
        undoChoice();
    }
}
```

This pattern is in almost every backtracking solution.

---

## How to Identify Backtracking Problems

Look for these signals:
- "Find all combinations / subsets / permutations"
- "Generate all valid..."
- Board / grid filling problems (Sudoku, N-Queens)
- "Can you partition..." with constraints
- Problems with exponential possibilities but small constraints (n ≤ 10-15 usually)

---

## Base Case Understanding

Always ask:
- When do I have a complete solution?
- When should I stop exploring this path?

**Common mistake**: Returning too early or missing valid solutions.

---

## Subsets Problems

### Core Idea
Generate all possible subsets of a set.

**Intuition**: For each element, you have two choices — include it or skip it.

### Dry Run (nums = [1,2])

```
Start: []
├── Include 1 → [1]
│   ├── Include 2 → [1,2]
│   └── Skip 2    → [1]
└── Skip 1    → []
    ├── Include 2 → [2]
    └── Skip 2    → []
```

### Implementation

```cpp
void subsets(vector<int>& nums) {
    vector<vector<int>> result;
    vector<int> current;
    
    function<void(int)> backtrack = [&](int index) {
        if (index == nums.size()) {
            result.push_back(current);
            return;
        }
        
        // Choose
        current.push_back(nums[index]);
        backtrack(index + 1);
        
        // Undo
        current.pop_back();
        backtrack(index + 1);
    };
    
    backtrack(0);
}
```

**Time**: O(2^n * n)  
**Space**: O(n) recursion depth

---

## Permutations Problems

### Core Idea
Generate all possible orderings of elements.

**Intuition**: At each position, try every unused element.

### Implementation (with used array)

```cpp
void permute(vector<int>& nums) {
    vector<vector<int>> result;
    vector<int> current;
    vector<bool> used(nums.size(), false);
    
    function<void()> backtrack = [&]() {
        if (current.size() == nums.size()) {
            result.push_back(current);
            return;
        }
        
        for (int i = 0; i < nums.size(); i++) {
            if (used[i]) continue;
            
            // Choose
            used[i] = true;
            current.push_back(nums[i]);
            
            backtrack();
            
            // Undo
            used[i] = false;
            current.pop_back();
        }
    };
    
    backtrack();
}
```

**Time**: O(n * n!)  
**Space**: O(n)

---

## Combination Sum Problems

Classic pattern: candidates with target sum, unlimited/restricted reuse.

**Key**: Sort + pruning when sum exceeds target.

---

## N-Queens

The classic board placement problem. Teaches constraint checking and board state management.

I usually use a vector of strings or bit manipulation for optimization in interviews.

---

## Sudoku Solver

Similar to N-Queens but with 3 constraints (row, col, box). Use boolean arrays or bitmasks for tracking.

---

## Pruning / Optimization Basics

- Sort the input (helps with pruning)
- Check constraints early (isSafe function)
- Stop when sum exceeds target
- Use bitmasks for faster state tracking (advanced)

---

## Common Beginner Mistakes

1. **Forgetting to backtrack** — modify state but never undo → wrong answers or crashes
2. **Pass by value** instead of reference → slow + high memory
3. **Wrong base case** — adding partial solutions or missing some
4. **Not copying result** when pushing to answer list
5. **Infinite recursion** due to bad index management
6. Modifying the input array permanently

**Pro tip**: Always draw the decision tree for small input before coding.

---

## Interview Insights

- Start with the naive recursive solution, then optimize with pruning
- Discuss time complexity honestly (usually exponential)
- Mention constraints (n ≤ 10-12 typically)
- For permutations/subsets, ask if duplicates exist
- Know how to convert between subsets ↔ combinations

---

## Real-World Usage

- Puzzle solvers (Sudoku, crossword)
- Scheduling and resource allocation
- Password cracking (ethical)
- Game AI move generation
- Compiler design (parsing)

---

## When Backtracking Should NOT Be Used

- Large input size (n > 15-20) → exponential time
- When DP or greedy gives optimal solution faster
- Problems with heavy overlapping subproblems (use memoization/DP instead)
- Simple permutations where std::next_permutation suffices

---

## Difference between Recursion and Backtracking

Recursion solves by breaking down.  
Backtracking explores **all** paths with trial and error + undo.

---

## Quick Revision Cheat Sheet

```
Golden Template:
void backtrack(state) {
    if (base case) { save solution; return; }
    
    for (each choice in choices) {
        if (valid(choice)) {
            make choice;
            backtrack(next state);
            undo choice;   // ← Most important
        }
    }
}

Common Patterns:
- Subsets: include / exclude
- Permutations: used array
- Combinations: start index to avoid duplicates
- Board: isSafe() function
```

**Remember**: "Try it, explore it, undo it."

---

*Written after too many WA verdicts on LeetCode. Hope this saves you some time.*

