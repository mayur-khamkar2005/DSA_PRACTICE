# Heaps & Priority Queue in C++ — Beginner to Advanced Interview Level

> My personal notes after grinding heaps for weeks. Went from "this is magic" to using priority queues naturally in interviews. This is everything I wish I had when I started.

---

## Table of Contents

- [Introduction to Heaps](#introduction-to-heaps)
- [Types of Heaps](#types-of-heaps)
- [Heap Representation](#heap-representation)
- [Priority Queue in C++](#priority-queue-in-c)
- [Core Heap Operations](#core-heap-operations)
- [Heapify Deep Dive](#heapify-understanding)
- [Important Patterns & Problems](#must-cover-these-important-topics)
- [How to Identify Heap Problems](#heap-problem-recognition)
- [Common Beginner Mistakes](#common-beginner-mistakes)
- [Real-world Usage](#real-world-usage)
- [Practice Problems](#practice-problems-section)
- [Quick Revision Cheat Sheet](#final-revision-section)

---

## Introduction to Heaps

Heaps are one of those data structures that feel weird at first but become super powerful once you get the intuition.

**What is a Heap?**  
A heap is a **complete binary tree** that satisfies the **heap property**.

- **Complete Binary Tree**: All levels are fully filled except possibly the last, which is filled from left to right.
- **Heap Property**: For a min-heap, every parent is smaller than or equal to its children. For max-heap, every parent is larger.

**Real-world Intuition**  
Think of it as a priority-based line where the most important person (smallest or largest) is always at the front. Unlike a normal queue, you can efficiently get the "best" element anytime.

Why heaps are useful: You need the minimum or maximum element quickly, repeatedly, while also inserting new elements.

---

## Types of Heaps

### Min Heap
- Smallest element is always at the root.
- Parent ≤ children.

### Max Heap
- Largest element is always at the root.
- Parent ≥ children.

**Parent-Child Relationship (in 0-based indexing):**
- Parent of index `i`: `(i-1)/2`
- Left child: `2*i + 1`
- Right child: `2*i + 2`

---

## Heap Representation

We use a **vector/array** to represent the heap. This is efficient and cache-friendly.

```cpp
// Example Min Heap
vector<int> minHeap = {3, 5, 4, 10, 8, 7};
// Tree looks like:
//       3
//     /   \
//    5     4
//   / \   /
// 10   8  7
```

**Advantages of Array Representation:**
- No extra pointers needed
- Fast access using index math
- Easy to implement

---

## Priority Queue in C++

C++ STL gives us `priority_queue` which is basically a max-heap by default.

```cpp
#include <bits/stdc++.h>
using namespace std;

// Max Heap (default)
priority_queue<int> pq;

// Min Heap
priority_queue<int, vector<int>, greater<int>> min_pq;

// Custom comparator for pairs (min heap by second element)
auto comp = [](pair<int,int>& a, pair<int,int>& b) {
    return a.second > b.second; // min heap on second
};
priority_queue<pair<int,int>, vector<pair<int,int>>, decltype(comp)> custom_pq(comp);
```

**Pro Tip**: Always specify the comparator clearly in interviews.

---

## Core Heap Operations

### 1. Insertion (Heapify Up / Bubble Up)
- Add element at the end
- Compare with parent and swap up until heap property is restored

### 2. Deletion (Extract Min/Max + Heapify Down)
- Swap root with last element
- Remove last element
- Heapify down from root

### 3. Build Heap
- Start from last non-leaf node and call heapify down on each

---

## Heapify Understanding

**Why bottom-up works**: Leaf nodes are already heaps. We only need to fix internal nodes.

**Dry Run** (Building Min Heap):

Initial array: `[4, 10, 3, 5, 1]`

After buildHeap → `[1, 4, 3, 5, 10]`

I always visualize the tree while doing this — helps a lot.

---

## Must Cover These Important Topics

### 1. Heap Sort

**Core Idea**: Build a max-heap, then repeatedly extract max and place at the end.

**Time**: O(n log n)  
**Space**: O(1)

```cpp
void heapify(vector<int>& arr, int n, int i) {
    int largest = i;
    int left = 2*i + 1;
    int right = 2*i + 2;
    
    if (left < n && arr[left] > arr[largest]) largest = left;
    if (right < n && arr[right] > arr[largest]) largest = right;
    
    if (largest != i) {
        swap(arr[i], arr[largest]);
        heapify(arr, n, largest);
    }
}

void heapSort(vector<int>& arr) {
    int n = arr.size();
    for (int i = n/2 - 1; i >= 0; i--) heapify(arr, n, i);
    
    for (int i = n-1; i > 0; i--) {
        swap(arr[0], arr[i]);
        heapify(arr, i, 0);
    }
}
```

### 2. Kth Largest Element

Use min-heap of size K.

```cpp
int findKthLargest(vector<int>& nums, int k) {
    priority_queue<int, vector<int>, greater<int>> minHeap;
    for (int num : nums) {
        minHeap.push(num);
        if (minHeap.size() > k) minHeap.pop();
    }
    return minHeap.top();
}
```

**Intuition**: Keep only top K. The smallest among them is the Kth largest.

### 3. Top K Frequent Elements

Use min-heap with frequency pairs.

### 4. Merge K Sorted Arrays / Linked Lists

Classic multi-pointer + min-heap.

```cpp
// Merge K Sorted Lists example
struct Compare {
    bool operator()(ListNode* a, ListNode* b) {
        return a->val > b->val;
    }
};

ListNode* mergeKLists(vector<ListNode*>& lists) {
    priority_queue<ListNode*, vector<ListNode*>, Compare> pq;
    // ... push heads
    // ... merge logic
}
```

### 5. Median from Data Stream

Maintain one max-heap (left) and one min-heap (right).

### 6. Sliding Window Maximum

Use deque (not heap) for O(n), but heap is good for understanding.

---

## Heap Problem Recognition

Look for these clues:
- "Kth largest/smallest"
- "Top K frequent"
- "Merge K sorted"
- "Median in stream"
- "Minimum cost to merge files"
- "Task scheduling with cooldown"
- "Find running median"

If you need efficient access to extreme elements while data is changing → think Heap/Priority Queue.

---

## Comparison Tables

| Feature              | Heap                  | Sorted Array       | BST                |
|----------------------|-----------------------|--------------------|--------------------|
| Get Min/Max          | O(1)                  | O(1)               | O(log n)           |
| Insert               | O(log n)              | O(n)               | O(log n)           |
| Delete Min/Max       | O(log n)              | O(n)               | O(log n)           |
| Build                | O(n)                  | O(n log n)         | O(n log n)         |

**Min Heap vs Max Heap**: Choose based on whether you want smallest or largest on top.

---

## Common Beginner Mistakes

- Using `priority_queue` without custom comparator and getting max instead of min
- Wrong indexing in manual heap (off-by-one)
- Forgetting to heapify after swap
- Using heap when sorting would suffice (small n)
- Memory issues with large custom objects

---

## Real-world Usage

- OS Process Scheduling (priority queues)
- Dijkstra’s algorithm
- Huffman Coding for compression
- Game AI (A* pathfinding)
- Streaming data analytics
- Recommendation systems (top recommendations)

---

## Practice Problems Section

**Easy:**
- Kth Largest Element
- Merge K Sorted Lists
- Last Stone Weight

**Intermediate:**
- Top K Frequent Elements
- Find Median from Data Stream
- Task Scheduler

**Advanced:**
- Sliding Window Maximum (deque optimization)
- IPO (hard greedy + heap)
- Minimum Cost to Connect Sticks

**Must Do LeetCode:**
- 215, 23, 347, 295, 373, 502, 871, 2533

---

## Final Revision Section

### Quick Cheat Sheet

```
Min Heap: priority_queue<int, vector<int>, greater<int>>
Max Heap: priority_queue<int> (default)

Kth Largest → Min-heap of size K
Top K Frequent → Min-heap of pairs
Merge K Lists → Min-heap of nodes
Running Median → Max-heap (left) + Min-heap (right)
```

**Mental Model**:  
Heap = "Always give me the best candidate right now"

**Interview Checklist**:
- Do I need frequent min/max access?
- Is K small? → Heap of size K
- Data is streaming? → Heap
- Need custom ordering? → Comparator

---

*Built after many frustrating debugging sessions. If this helped, star the repo!*

**Happy coding!** 🚀
