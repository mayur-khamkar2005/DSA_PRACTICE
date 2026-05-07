# Arrays in C++ — Beginner to Advanced Interview Level

> Notes + implementations I built while practicing arrays for coding interviews.  
> Arrays look easy initially, but most interview patterns actually start from arrays.

---

# Table of Contents

- Introduction to Arrays
- Array Fundamentals
- Arrays vs Vectors
- Prefix Sum
- Kadane’s Algorithm
- Sliding Window Basics
- Two Pointer Basics
- Dutch National Flag Algorithm
- Merge Intervals
- Majority Element
- Best Time to Buy and Sell Stock
- Rearrangement Problems
- Subarray Problems
- Maximum Product Subarray
- Rotate Array
- Product of Array Except Self
- Missing Number Problems
- Duplicate Detection
- Common Mistakes
- Practice Problems
- Final Cheat Sheet

---

# Introduction to Arrays

Arrays are the foundation of DSA.

Most interview topics eventually connect back to:
- arrays
- indexing
- traversal
- memory access

If your array intuition is weak:
everything later feels harder.

---

# What is an Array?

An array stores elements in:
```text
contiguous memory
```

Example:

```cpp
int arr[5] = {1, 2, 3, 4, 5};
```

Memory looks like:

```text
|1|2|3|4|5|
```

Everything stays together in memory.

That is why:
```cpp
arr[i]
```

is extremely fast.

---

# Why Arrays are Fast

Accessing:

```cpp
arr[i]
```

takes:

```cpp
O(1)
```

Because arrays use direct memory calculation.

Formula internally:

```text
base_address + (index * size)
```

This is the real reason arrays are powerful.

---

# Array Traversal

```cpp
for(int i = 0; i < n; i++) {
    cout << arr[i] << " ";
}
```

Time Complexity:

```cpp
O(n)
```

---

# Array Fundamentals

## Insertion

Middle insertion is expensive.

Why?

Because shifting happens.

```text
1 2 3 4
```

Insert `10` at index 1:

```text
1 10 2 3 4
```

Elements shift right.

---

## Deletion

Deletion also shifts elements.

Time:

```cpp
O(n)
```

---

## Searching

## Linear Search

```cpp
O(n)
```

## Binary Search (sorted array)

```cpp
O(log n)
```

---

# Arrays vs Vectors

| Arrays | Vectors |
|---|---|
| Fixed size | Dynamic size |
| Faster slightly | More flexible |
| Manual sizing | Auto resize |
| Less features | STL support |

---

## Vector Example

```cpp
vector<int> nums;

nums.push_back(10);
nums.push_back(20);
```

In interviews:
use vectors mostly.

---

# Prefix Sum

One of the most important array patterns.

---

# Core Intuition

Instead of recalculating sums repeatedly:
store cumulative sums.

---

# Example

```text
1 2 3 4 5
```

Prefix Sum:

```text
1 3 6 10 15
```

Now:
sum from index 1 to 3:

```text
10 - 1 = 9
```

---

# Prefix Sum Code

```cpp
vector<int> prefix(n);

prefix[0] = arr[0];

for(int i = 1; i < n; i++) {
    prefix[i] = prefix[i - 1] + arr[i];
}
```

---

# Kadane’s Algorithm

Most famous array algorithm.

---

# Problem

Maximum subarray sum.

---

# Core Intuition

If current sum becomes negative:
discard it.

Negative sum only hurts future answers.

---

# Code

```cpp
int maxSubArray(vector<int>& nums) {

    int current = 0;
    int maximum = nums[0];

    for(int num : nums) {

        current += num;

        maximum = max(maximum, current);

        if(current < 0) {
            current = 0;
        }
    }

    return maximum;
}
```

---

# Kadane Dry Run

```text
[-2,1,-3,4,-1,2,1,-5,4]
```

Best subarray:

```text
[4,-1,2,1]
```

Answer:

```text
6
```

---

# Sliding Window Basics

Used in:
- subarray problems
- longest sequence
- fixed-size ranges

---

# Core Intuition

Instead of recalculating window:
slide it.

---

# Fixed Window Example

```cpp
int windowSum = 0;

for(int i = 0; i < k; i++) {
    windowSum += arr[i];
}

for(int i = k; i < n; i++) {

    windowSum += arr[i];
    windowSum -= arr[i - k];
}
```

---

# Two Pointer Basics

Most important optimization pattern.

---

# Core Intuition

Use two indices:
instead of nested loops.

---

# Example

Sorted array pair sum.

```cpp
int left = 0;
int right = n - 1;

while(left < right) {

    int sum = arr[left] + arr[right];

    if(sum == target) return true;

    else if(sum < target) left++;

    else right--;
}
```

---

# Dutch National Flag Algorithm

Problem:
sort:
```text
0 1 2
```

---

# Core Intuition

Maintain:
- low
- mid
- high

Zones.

---

# Code

```cpp
void sortColors(vector<int>& nums) {

    int low = 0;
    int mid = 0;
    int high = nums.size() - 1;

    while(mid <= high) {

        if(nums[mid] == 0) {
            swap(nums[low], nums[mid]);
            low++;
            mid++;
        }
        else if(nums[mid] == 1) {
            mid++;
        }
        else {
            swap(nums[mid], nums[high]);
            high--;
        }
    }
}
```

---

# Merge Intervals

Very common interview problem.

---

# Core Intuition

Sort intervals first.

Then merge overlaps.

---

# Pattern Recognition

If you see:
- intervals
- ranges
- overlap

Think:
```text
sorting + traversal
```

---

# Majority Element

## Boyer Moore Voting Algorithm

Core idea:

Majority element cancels others.

---

# Code

```cpp
int majorityElement(vector<int>& nums) {

    int candidate = 0;
    int count = 0;

    for(int num : nums) {

        if(count == 0) {
            candidate = num;
        }

        if(num == candidate) count++;
        else count--;
    }

    return candidate;
}
```

---

# Best Time to Buy and Sell Stock

## Core Intuition

Track minimum price.

Calculate max profit continuously.

---

# Code

```cpp
int maxProfit(vector<int>& prices) {

    int mini = prices[0];
    int profit = 0;

    for(int price : prices) {

        mini = min(mini, price);

        profit = max(profit, price - mini);
    }

    return profit;
}
```

---

# Rearrangement Problems

Examples:
- move zeroes
- alternate positive negative
- rearrange by sign

Mostly solved using:
- two pointers
- extra arrays
- swapping

---

# Subarray Problems

Very important topic.

---

# Core Intuition

Subarray:
```text
contiguous
```

Subsequence:
```text
not necessarily contiguous
```

Most beginners confuse both.

---

# Maximum Product Subarray

Harder than Kadane.

Why?

Because:
negative × negative = positive.

Need:
- current max
- current min

---

# Rotate Array

## Reverse Method

```cpp
reverse(nums.begin(), nums.end());

reverse(nums.begin(), nums.begin() + k);

reverse(nums.begin() + k, nums.end());
```

---

# Product of Array Except Self

Important interview classic.

---

# Core Intuition

Use:
- prefix product
- suffix product

Avoid division.

---

# Missing Number Problems

Patterns:
- XOR
- Sum formula
- Hashing

---

# XOR Trick

```cpp
int ans = 0;

for(int i = 0; i <= n; i++) {
    ans ^= i;
}

for(int num : nums) {
    ans ^= num;
}
```

Remaining answer:
missing number.

---

# Duplicate Detection Problems

Patterns:
- HashSet
- Floyd Cycle
- Sorting

---

# Sorting + Arrays Connection

Sorting helps in:
- pair problems
- interval problems
- duplicate detection
- closest elements

Most optimized array solutions begin with:

```cpp
sort(arr.begin(), arr.end());
```

---

# Common Interview Patterns

| Pattern | Common Problems |
|---|---|
| Prefix Sum | Range Sum |
| Sliding Window | Longest Subarray |
| Two Pointers | Pair Sum |
| Sorting | Intervals |
| Hashing | Duplicates |
| Kadane | Maximum Subarray |

---

# Common Beginner Mistakes

## 1. Off-by-One Errors

Very common in:
```cpp
i < n
```

vs

```cpp
i <= n
```

---

## 2. Index Out of Bounds

Most common crash reason.

---

## 3. Confusing Subarray vs Subsequence

Subarray:
continuous.

Subsequence:
not continuous.

---

## 4. Overflow Mistakes

Use:
```cpp
long long
```

when sums become large.

---

# Comparison Tables

# Array vs Linked List

| Array | Linked List |
|---|---|
| Fast indexing | Slow indexing |
| Contiguous memory | Dynamic nodes |
| Better cache | Flexible size |

---

# Brute Force vs Optimized

| Problem | Brute Force | Optimized |
|---|---|---|
| Pair Sum | O(n²) | O(n) |
| Maximum Subarray | O(n²) | O(n) |
| Range Sum | O(n²) | O(1) query |

---

# Real-world Usage

Arrays are used in:
- image processing
- game engines
- matrices
- caching
- analytics
- databases
- graphics programming

---

# Practice Problems

## Easy
- Two Sum
- Best Time to Buy and Sell Stock
- Move Zeroes

## Medium
- Product of Array Except Self
- Maximum Product Subarray
- Merge Intervals

## Hard
- Trapping Rain Water
- Median of Two Sorted Arrays
- First Missing Positive

---

# Interview Insights

Most array interview problems are actually:
- pattern recognition tests
- optimization tests

Not syntax tests.

The real skill:
recognizing:
- sliding window
- two pointers
- prefix sum
- hashing
- sorting patterns

quickly.

---

# Final Cheat Sheet

| Pattern | Use Case |
|---|---|
| Prefix Sum | Range queries |
| Kadane | Maximum subarray |
| Sliding Window | Subarray optimization |
| Two Pointers | Pair/triplet problems |
| Sorting | Intervals & duplicates |
| Hashing | Frequency counting |

---

# Final Notes

If your array fundamentals are strong:
learning:
- strings
- sliding window
- DP
- graphs

becomes much easier.

Arrays are not a beginner topic.

They are the base of almost everything in DSA.
