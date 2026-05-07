# Dynamic Programming in C++ — Beginner to Advanced Interview Level

> These are my battle-scarred notes after grinding 100+ DP problems on LeetCode. DP used to feel like black magic. Now it feels like a reliable toolbox. This README is exactly what helped me go from "I don't get it" to solving hard DP questions in interviews.

---

## Table of Contents

- [Introduction to Dynamic Programming](#introduction-to-dynamic-programming)
- [Core DP Intuition](#core-dp-intuition)
- [How to Identify DP Problems](#how-to-identify-dp-problems)
- [DP Problem Solving Framework](#dp-problem-solving-framework)
- [Memoization (Top-Down)](#memoization-top-down)
- [Tabulation (Bottom-Up)](#tabulation-bottom-up)
- [Space Optimization](#space-optimization)
- [DP Patterns Deep Dive](#dp-patterns-deep-dive)
- [Common Mistakes & Traps](#common-mistakes--traps)
- [Comparison Tables](#comparison-tables)
- [Real-world Usage](#real-world-usage)
- [Practice Problems](#practice-problems)
- [Final Revision Cheat Sheet](#final-revision-cheat-sheet)

---

## Introduction to Dynamic Programming

Dynamic Programming is basically "smart recursion". Instead of solving the same subproblems again and again, you store the results and reuse them.

### What is Dynamic Programming?

It's an optimization technique that solves problems by breaking them into smaller overlapping subproblems and storing their solutions.

### Why DP feels difficult initially

- Recursion tree looks scary
- You keep thinking "where do I even start?"
- Too many states to track
- Hard to figure out the right DP state

I spent weeks just staring at problems before things clicked.

### Why DP is important in interviews

- Almost every company asks DP (FAANG especially)
- Shows you can optimize exponential solutions to polynomial time
- Tests both recursion understanding and optimization skills

### Recursion vs DP

**Recursion**: Solve by calling smaller versions of itself. Can be exponential time.

**DP**: Same recursion + caching = massive speedup.

### Greedy vs DP

**Greedy**: Make the locally best choice at each step. Works when optimal substructure + greedy choice property holds.

**DP**: Considers all possibilities. Safer when greedy fails.

### Divide & Conquer vs DP

Divide & Conquer: Subproblems are independent (Merge Sort, Binary Search).

DP: Subproblems overlap heavily.

---

## Core DP Intuition

### Overlapping Subproblems

The same subproblem gets solved multiple times in naive recursion. Like calculating Fibonacci(5) multiple times in the tree.

### Optimal Substructure

The optimal solution to the problem can be constructed from optimal solutions of its subproblems.

### Memoization Intuition

"Remember what you've already computed." Top-down approach. You recurse, but before computing, check if you already know the answer.

### Tabulation Intuition

Build solutions from smallest subproblems to the biggest. Bottom-up. Fill a table step by step.

### State Transition Thinking

This is the heart of DP.

A **state** is defined by the parameters that uniquely identify a subproblem.

Example: `dp[i]` = best answer for first `i` elements.

**Transition**: How do I get to `dp[i]` from previous states?

---

## How to Identify DP Problems

Experienced devs look for these signals:

- "Maximum / Minimum" something
- "Count the number of ways"
- "Best possible score / profit"
- "Can I reach / Is it possible"
- Decisions like "take or not take"
- Overlapping recursive calls when you try brute force
- Optimization on sequences, strings, grids, or decisions over time

If brute force is exponential and has overlapping subproblems → DP candidate.

---

## DP Problem Solving Framework

This framework saved me in interviews:

1. **Brute Force Recursion** - Write the recursive solution first
2. **Define State** - What parameters define the current subproblem?
3. **Base Cases** - When do I stop?
4. **Transition** - How do I move to next state?
5. **Memoization** - Add cache (map or 2D array)
6. **Convert to Tabulation** - Build bottom-up
7. **Space Optimization** - Reduce space if possible

Always start with recursion. It's easier to think recursively first.

---

## Memoization (Top-Down)

### Core Idea
Recurse with caching.

### Intuition
You're still thinking top-down like recursion, but you avoid recomputing.

**Common Structure:**

```cpp
int dp[101][101]; // or vector<vector<int>> or unordered_map

int solve(int i, int j) {
    if (i == ... || j == ...) return base;
    if (dp[i][j] != -1) return dp[i][j];
    
    // try choices
    int ans = ...;
    return dp[i][j] = ans;
}
```

**Dry Run Example**: Climbing Stairs (will cover in patterns)

**Mistakes**:
- Forgetting to initialize memo with -1
- Wrong state definition
- Stack overflow on deep recursion

---

## Tabulation (Bottom-Up)

### Core Idea
Fill table from smallest to largest subproblems.

### Intuition
You know the dependencies. Start from base cases and build up.

**Advantages over Memo**:
- No recursion stack risk
- Often faster due to better cache locality
- Easier to optimize space

---

## Space Optimization

When your DP only depends on previous 1 or 2 rows/states, you can reduce space dramatically.

Classic example: Fibonacci (O(n) → O(1)), Knapsack (O(n*W) → O(W))

---

## DP Patterns Deep Dive

### 1. Fibonacci Pattern

**Climbing Stairs**

State: `dp[i]` = number of ways to reach step `i`

Transition: `dp[i] = dp[i-1] + dp[i-2]`

```cpp
// Space Optimized
int climbStairs(int n) {
    if (n <= 2) return n;
    int a = 1, b = 2;
    for (int i = 3; i <= n; i++) {
        int c = a + b;
        a = b;
        b = c;
    }
    return b;
}
```

### 2. 1D DP

**House Robber**

State: `dp[i]` = max money robbing first i houses

Transition: `dp[i] = max(dp[i-1], dp[i-2] + nums[i])`

**Min Cost Climbing Stairs** - similar pattern.

### 3. 2D Grid DP

**Unique Paths**

State: `dp[i][j]` = number of ways to reach (i,j)

Transition: `dp[i][j] = dp[i-1][j] + dp[i][j-1]`

**Minimum Path Sum** - similar with costs.

### 4. Knapsack Pattern

**0/1 Knapsack**

State: `dp[i][w]` = max value using first i items with weight limit w

Transition:
```cpp
if (weight[i] <= w)
    dp[i][w] = max(dp[i-1][w], dp[i-1][w - weight[i]] + value[i]);
else
    dp[i][w] = dp[i-1][w];
```

**Subset Sum**, **Partition Equal Subset Sum** use same idea.

### 5. Longest Increasing Subsequence (LIS)

State: `dp[i]` = length of LIS ending at index i

Transition: `dp[i] = max(dp[j] + 1)` for all j < i where arr[j] < arr[i]

O(n²) → can be optimized to O(n log n) with patience sorting.

### 6. Longest Common Subsequence (LCS)

State: `dp[i][j]` = LCS of first i chars of s1 and first j of s2

Transition:
- If s1[i-1] == s2[j-1]: dp[i][j] = dp[i-1][j-1] + 1
- Else: max(dp[i-1][j], dp[i][j-1])

### 7. String DP

**Edit Distance** (Levenshtein Distance)

State represents minimum operations to convert first i chars of word1 to first j of word2.

**Palindrome Partitioning** - classic "DP on intervals"

### 8. Take / Not Take Pattern

Very common in subsets and knapsack-like problems.

At each index: 
- Take current element (if possible)
- Not take it

### 9. DP on Subsequences

Similar to take/not take but focused on maintaining order.

### 10. DP on Stocks

Multiple variants (cooldown, transaction limits, etc.)

State often includes: day index + transactions left + holding stock or not.

### 11. DP on Intervals

Problems like Matrix Chain Multiplication, Burst Balloons, Palindrome Partitioning.

State: `dp[i][j]` = best answer for subarray from i to j

### 12. DP on Trees

Tree diameter, house robber on tree, etc. Usually done with recursion + memo on nodes.

### 13. Bitmask DP (Introduction)

Used when you have small N (≤ 20) and need to track visited set. TSP (Travelling Salesman) classic.

---

## Common Mistakes & Traps

- Wrong state definition (most common)
- Off-by-one in indices
- Not handling all base cases
- Initializing DP table with 0 instead of -1 (for memo)
- Iteration order wrong in tabulation (dependencies)
- Stack overflow in memoization for large inputs
- Forgetting to update answer in "max/min" problems
- Using map instead of array for memo (slower)

---

## Comparison Tables

**Recursion vs Memo vs Tabulation**

| Approach     | Time     | Space      | Pros                     | Cons                  |
|--------------|----------|------------|--------------------------|-----------------------|
| Recursion    | Exponential | O(n) stack | Easy to think           | Too slow              |
| Memoization  | O(states*transitions) | O(states) + stack | Natural recursion      | Stack risk            |
| Tabulation   | Same     | O(states)  | No stack, fast          | Harder to visualize   |

**Greedy vs DP**

| Aspect       | Greedy               | DP                          |
|--------------|----------------------|-----------------------------|
| Correctness  | Sometimes            | Almost always               |
| Time         | Faster               | Slower but reliable         |
| When to use  | Optimal substructure + greedy choice | When greedy fails           |

---

## Real-world Usage

- Route optimization (shortest path variants)
- Resource allocation & scheduling
- Portfolio optimization in finance
- Text processing (spell check, diff tools)
- Game AI (minimax with memo)
- Bioinformatics (sequence alignment)
- Compiler optimization

---

## Practice Problems

### Easy
- Climbing Stairs
- House Robber
- Min Cost Climbing Stairs
- Unique Paths

### Intermediate
- 0/1 Knapsack
- Longest Increasing Subsequence
- Longest Common Subsequence
- Edit Distance
- Partition Equal Subset Sum

### Advanced
- Burst Balloons
- Regular Expression Matching
- Minimum Cost to Merge Stones
- DP on Trees (House Robber III)
- Travelling Salesman (Bitmask)

**Must-do LeetCode DP List**: 70, 198, 213, 300, 1143, 72, 312, etc.

---

## Final Revision Cheat Sheet

**Mental Models:**
- "What is my state?" (position + extra params)
- "What choices do I have at each state?"
- "How do previous states help me?"

**Quick Checklist in Interview:**
1. Can I solve it recursively?
2. Are there overlapping subproblems?
3. Define state clearly
4. Write transitions
5. Handle base cases
6. Add memo / build table
7. Optimize space if needed

**State Design Tips:**
- Index in array/string
- Previous choices (last taken, remaining capacity, etc.)
- Boolean flags (holding stock, tight constraint)

---

*Built after many frustrating nights and "aha" moments. If this helped you finally understand DP, give it a star ⭐*

Happy coding!
