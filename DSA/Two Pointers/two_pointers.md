# Two Pointers in C++ — Beginner to Advanced Interview Level

> My personal notes after grinding dozens of two pointer problems. Went from getting confused with pointer movements to spotting these patterns instantly in interviews.

---

## Table of Contents

- [Introduction to Two Pointers](#introduction-to-two-pointers)
- [Core Intuition](#core-two-pointer-intuition)
- [Types of Two Pointer Patterns](#types-of-two-pointer-patterns)
- [How to Identify Two Pointer Problems](#how-to-identify-two-pointer-problems)
- [Opposite Direction Pointers](#opposite-direction-pointer-technique)
- [Same Direction Pointers](#same-direction-pointer-technique)
- [Fast and Slow Pointers](#fast-and-slow-pointer-technique)
- [Key Problems with Full Solutions](#must-cover-these-important-interview-topics)
- [Common Mistakes](#common-beginner-mistakes)
- [Comparison Tables](#comparison-tables)
- [Real-world Usage](#real-world-usage)
- [Practice Problems](#practice-problems-section)
- [Quick Revision Cheat Sheet](#final-revision-section)

---

## Introduction to Two Pointers

Two Pointers is not just a technique — it's a way to traverse arrays or strings with two moving indices instead of one. It turns O(n²) brute force solutions into O(n) or O(n log n) magic in many cases.

**Real-world intuition**: Imagine two people walking on a number line or reading a book from both ends. They can meet in the middle or maintain some condition while moving efficiently.

### Brute Force vs Two Pointers

| Approach       | Time Complexity | Why Two Pointers Wins |
|----------------|-----------------|-----------------------|
| Brute Force    | O(n²)           | Checking every pair   |
| Two Pointers   | O(n)            | Smart movement        |

---

## Core Two Pointer Intuition

The beauty lies in **sorted data** or **monotonic properties**. By moving pointers based on conditions, you eliminate large portions of the search space without checking every combination.

**Key Mental Model**: One pointer handles the "left/good" part, the other the "right/exploration" part. They move towards each other or in the same direction depending on the problem.

---

## Types of Two Pointer Patterns

1. **Opposite Direction** — Start from both ends, move towards center (Container With Most Water, Two Sum II)
2. **Same Direction** — Both move right, one maintains a window (Remove Duplicates, Move Zeroes)
3. **Fast & Slow** — Different speeds for cycle detection or middle finding
4. **Partition Style** — Dutch National Flag, Quick Sort partition

---

## How to Identify Two Pointer Problems

Look for these clues:
- Sorted array + pair/triplet sum
- "In-place" modification required
- Palindromes or symmetry
- "Maximum/minimum something in subarray"
- Constant extra space mentioned
- "Remove duplicates" or "move all X to end"

Experienced devs see "sorted + pairs" and immediately think two pointers.

---

## Opposite Direction Pointer Technique

**Core Idea**: Start one pointer at beginning (`left = 0`), another at end (`right = n-1`). Move based on condition.

**Intuition**: Like squeezing the search space from both sides.

### Example: Container With Most Water

```cpp
int maxArea(vector<int>& height) {
    int left = 0, right = height.size() - 1;
    int maxWater = 0;
    
    while (left < right) {
        int water = min(height[left], height[right]) * (right - left);
        maxWater = max(maxWater, water);
        
        if (height[left] < height[right]) left++;
        else right--;
    }
    return maxWater;
}
```

**Dry Run** (height = [1,8,6,2,5,4,8,3,7]):
- left=0(1), right=8(7) → area=8, move left
- ... eventually finds max area 49

**Time**: O(n)  
**Space**: O(1)

---

## Same Direction Pointer Technique

**Core Idea**: Both pointers move right. Slow pointer marks the position for next valid element.

### Example: Remove Duplicates from Sorted Array

```cpp
int removeDuplicates(vector<int>& nums) {
    if (nums.empty()) return 0;
    
    int slow = 0;
    for (int fast = 1; fast < nums.size(); fast++) {
        if (nums[fast] != nums[slow]) {
            slow++;
            nums[slow] = nums[fast];
        }
    }
    return slow + 1;
}
```

**Intuition**: Fast explores, Slow builds the valid prefix.

---

## Fast and Slow Pointer Technique

**Core Idea**: One pointer moves twice as fast as the other.

**Why it works for cycle detection (Floyd's Algorithm)**: If there's a cycle, the fast pointer will eventually lap the slow one inside the cycle.

### Example: Linked List Cycle Detection

```cpp
bool hasCycle(ListNode* head) {
    ListNode* slow = head;
    ListNode* fast = head;
    
    while (fast && fast->next) {
        slow = slow->next;
        fast = fast->next->next;
        
        if (slow == fast) return true;
    }
    return false;
}
```

**Mathematical Intuition**: In a cycle of length C, fast catches slow after some steps.

---

## MUST COVER THESE IMPORTANT INTERVIEW TOPICS

### 3Sum (Classic Hard Problem)

```cpp
vector<vector<int>> threeSum(vector<int>& nums) {
    sort(nums.begin(), nums.end());
    vector<vector<int>> result;
    
    for (int i = 0; i < nums.size() - 2; i++) {
        if (i > 0 && nums[i] == nums[i-1]) continue;
        
        int left = i + 1, right = nums.size() - 1;
        while (left < right) {
            int sum = nums[i] + nums[left] + nums[right];
            if (sum == 0) {
                result.push_back({nums[i], nums[left], nums[right]});
                left++; right--;
                while (left < right && nums[left] == nums[left-1]) left++;
                while (left < right && nums[right] == nums[right+1]) right--;
            } else if (sum < 0) left++;
            else right--;
        }
    }
    return result;
}
```

**Interview Observation**: Always sort first, then fix one element + two pointers.

### Sort Colors (Dutch National Flag)

```cpp
void sortColors(vector<int>& nums) {
    int low = 0, mid = 0, high = nums.size() - 1;
    
    while (mid <= high) {
        if (nums[mid] == 0) {
            swap(nums[low], nums[mid]);
            low++; mid++;
        } else if (nums[mid] == 1) {
            mid++;
        } else {
            swap(nums[mid], nums[high]);
            high--;
        }
    }
}
```

**Intuition**: Three regions — 0s, 1s, 2s.

---

## Common Beginner Mistakes

- Moving both pointers in wrong direction → missing answers
- Forgetting to handle duplicates (especially in 3Sum)
- Off-by-one when `left < right` vs `left <= right`
- Infinite loops when pointers don't move
- Not sorting when required
- Pointer crossing without checks

**Pro Tip**: Always draw the pointers on paper first.

---

## Comparison Tables

| Technique              | Use Case                     | Time    | Space |
|------------------------|------------------------------|---------|-------|
| Opposite Direction     | Pair sums, max area          | O(n)    | O(1)  |
| Same Direction         | Remove duplicates, move zeroes | O(n)  | O(1)  |
| Fast & Slow            | Cycle detection, middle node | O(n)    | O(1)  |

---

## Real-world Usage

- Efficient pair finding in large datasets
- Text processing (palindromes, anagrams)
- Memory-efficient streaming algorithms
- Collision detection in games

---

## Practice Problems Section

**Easy**: Two Sum II, Remove Duplicates, Valid Palindrome  
**Intermediate**: Container With Most Water, 3Sum, Sort Colors  
**Advanced**: Trapping Rain Water, Merge k Sorted Lists (with pointers)

**Must-Do LeetCode**: 11, 15, 75, 125, 167, 283, 344, 977

---

## Final Revision Section - Quick Cheat Sheet

**Mental Models**:
- Opposite: Squeeze from ends
- Same: Fast explores, Slow builds result
- Fast/Slow: Tortoise and Hare

**Pattern Recognition**:
- Sorted? → Two Pointers
- Need pairs with condition? → Two Pointers
- In-place + constant space? → Two Pointers

**Interview Checklist**:
1. Can I sort the array?
2. Do I need all pairs or unique?
3. Handle duplicates carefully
4. Draw pointers movement

---

*Built after many frustrating debugging sessions. If this helped, star the repo!*

Happy coding! 🚀
