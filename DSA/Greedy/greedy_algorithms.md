# Greedy Algorithms in C++ — Beginner to Advanced Interview Level

> Personal notes from grinding LeetCode greedy problems. I used to think greedy was just "pick the best looking option and pray" — until I realized when it actually works and when it completely falls apart.

---

## Table of Contents

- [Introduction to Greedy](#introduction-to-greedy)
- [Greedy Intuition & Mental Model](#greedy-intuition--mental-model)
- [Local Optimum vs Global Optimum](#local-optimum-vs-global-optimum)
- [How to Identify Greedy Problems](#how-to-identify-greedy-problems)
- [Greedy + Sorting Connection](#greedy--sorting-connection)
- [Classic Greedy Problems](#classic-greedy-problems)
- [Greedy Proof Thinking](#greedy-proof-thinking)
- [Greedy vs Dynamic Programming](#greedy-vs-dynamic-programming)
- [Common Beginner Mistakes](#common-beginner-mistakes)
- [Interview Tips & Tricks](#interview-tips--tricks)
- [Practice Problems](#practice-problems)
- [Quick Revision Cheat Sheet](#quick-revision-cheat-sheet)

---

## Introduction to Greedy

Greedy algorithms make the **locally optimal choice** at each step, hoping it leads to a globally optimal solution.

The key point: It doesn't explore all possibilities like DP or Backtracking. It just goes for what looks best right now.

This makes it super fast (usually O(n log n) due to sorting), but also risky if you pick the wrong greedy choice.

## Greedy Intuition & Mental Model

Think of it like climbing a mountain where you always take the steepest step upward. Sometimes you reach the top. Sometimes you get stuck on a smaller peak.

**Real-world examples:**
- Buying the cheapest item available right now (without thinking about future deals)
- Always taking the shortest queue at the supermarket

## Local Optimum vs Global Optimum

- **Local Optimum**: Best choice at the current moment
- **Global Optimum**: Best possible answer overall

Greedy works when the problem has **optimal substructure** and the greedy choice property (making the local best doesn't ruin future choices).

## How to Identify Greedy Problems

Look for these signals:
- "Minimum number of..." or "Maximum number of..."
- "Select maximum items under constraint"
- Interval scheduling / activity selection
- "Lexicographically smallest/largest"
- Problems where sorting helps a lot
- You need an optimal answer but no overlapping subproblems (unlike DP)

If the problem feels like it has "safe" choices that won't affect future decisions negatively — greedy might work.

## Greedy + Sorting Connection

Sorting is the secret sauce in most greedy problems. Once sorted, decisions become much simpler.

**Observation**: Many greedy problems become obvious after sorting by start time, end time, value, etc.

## Classic Greedy Problems

### 1. Activity Selection / Interval Scheduling

**Core Idea**: Select maximum number of non-overlapping intervals.

**Intuition**: Always pick the activity that finishes earliest. This leaves maximum room for others.

```cpp
int activitySelection(vector<pair<int,int>>& activities) {
    sort(activities.begin(), activities.end(), [](auto& a, auto& b){
        return a.second < b.second; // sort by end time
    });
    
    int count = 1;
    int lastEnd = activities[0].second;
    
    for(int i = 1; i < activities.size(); i++) {
        if(activities[i].first >= lastEnd) {
            count++;
            lastEnd = activities[i].second;
        }
    }
    return count;
}
```

**Time**: O(n log n)  
**Why it works**: Earliest finish leaves most space.

### 2. Fractional Knapsack

**Core Idea**: Maximize value with weight constraint. Can take fractions.

**Greedy Choice**: Sort by value/weight ratio (highest first).

### 3. Jump Game (I & II)

**Jump Game I**: Can you reach the last index?

**Greedy**: Keep track of the farthest you can reach.

```cpp
bool canJump(vector<int>& nums) {
    int maxReach = 0;
    for(int i = 0; i <= maxReach && i < nums.size(); i++) {
        maxReach = max(maxReach, i + nums[i]);
        if(maxReach >= nums.size() - 1) return true;
    }
    return false;
}
```

**Jump Game II**: Minimum jumps to reach end → greedy track of current end and farthest.

### 4. Gas Station

**Core Idea**: Find starting point to complete circuit.

**Intuition**: If total gas < total cost → impossible. Track the lowest tank level.

### 5. Huffman Coding (Intro)

Used in compression. Always merge two smallest frequency nodes.

## Greedy Proof Thinking

To be confident in greedy:
1. Show that the greedy choice is safe (doesn't eliminate optimal solution)
2. Show optimal substructure

In interviews, you don't always need full proof — but be ready to explain why your greedy choice is correct.

## Greedy vs Dynamic Programming

| Aspect                | Greedy                     | Dynamic Programming             |
|-----------------------|----------------------------|---------------------------------|
| Choice                | Local best                 | Considers all possibilities     |
| Speed                 | Usually faster             | Slower (has overlapping work)   |
| Correctness           | Not always optimal         | Always optimal                  |
| Memory                | O(1) or O(n)               | Usually O(n) or O(n²)           |
| Use when              | Safe local choices         | Overlapping subproblems         |

**When Greedy Fails**: Coin change with weird denominations (e.g., coins [1,3,4], make 6 → greedy takes 4+1+1 but optimal is 3+3).

## Common Beginner Mistakes

- Assuming greedy always works after seeing one example
- Wrong sorting key (sort by start instead of end time)
- Not handling ties properly
- Forgetting to check if solution is possible (like in Gas Station)
- Applying greedy to problems that need DP (0/1 Knapsack)

## Interview Tips & Tricks

1. Ask if fractions are allowed (Fractional vs 0/1 Knapsack)
2. Try greedy first — it's fast to implement and test
3. If greedy fails on some test cases → switch to DP
4. Sorting is your friend
5. Think about "safe choice" — will this decision hurt me later?

## Practice Problems

**Easy:**
- Assign Cookies
- Lemonade Change
- Maximum Subarray (Kadane's — borderline greedy)

**Intermediate:**
- Jump Game I & II
- Gas Station
- Non-overlapping Intervals
- Minimum Number of Arrows to Burst Balloons

**Advanced:**
- Task Scheduler
- IPO (Max Profit)
- Car Pooling
- Reorganize String

**Most Important LeetCode:**
- 435. Non-overlapping Intervals
- 452. Minimum Number of Arrows to Burst Balloons
- 55/45. Jump Game
- 134. Gas Station
- 253. Meeting Rooms II

## Quick Revision Cheat Sheet

```
Greedy Template:
1. Sort the input (by end time, ratio, etc.)
2. Initialize result + current state
3. Iterate and make locally optimal choice
4. Return result

When to suspect Greedy:
- "Maximum number of non-overlapping"
- "Minimum cost to achieve X"
- "Lex smallest/largest"
- After sorting, simple decisions

Red Flag: If later choices depend heavily on previous specific selections → probably DP.
```

---

*Built after many wrong greedy submissions. If this helped, star the repo!*

