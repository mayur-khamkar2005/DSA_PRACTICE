# Sliding Window in C++ — Beginner to Interview Level

> My personal notes after solving 50+ sliding window problems on LeetCode. Finally stopped using brute force and started seeing the pattern. This is the guide I wish existed when I was stuck with TLEs.

---

## Table of Contents

- [What is Sliding Window, Really?](#what-is-sliding-window-really)
- [Why Sliding Window Matters](#why-sliding-window-matters)
- [Brute Force vs Sliding Window](#brute-force-vs-sliding-window)
- [How to Identify Sliding Window Problems](#how-to-identify-sliding-window-problems)
- [Fixed Size Window](#fixed-size-window)
- [Dynamic / Variable Size Window](#dynamic--variable-size-window)
- [Two Pointers Connection](#two-pointers-connection)
- [Window Expansion & Shrinking Logic](#window-expansion--shrinking-logic)
- [Common Sliding Window Patterns](#common-sliding-window-patterns)
- [Frequency Map Based Window](#frequency-map-based-window)
- [Longest / Smallest Subarray Problems](#longest--smallest-subarray-problems)
- [String Based Sliding Window Problems](#string-based-sliding-window-problems)
- [Big Comparison Table](#big-comparison-table)
- [Common Beginner Mistakes](#common-beginner-mistakes)
- [Interview Insights](#interview-insights)
- [Real-World Usage](#real-world-usage)
- [When NOT to Use Sliding Window](#when-not-to-use-sliding-window)
- [Cheat Sheet](#cheat-sheet)

---

## What is Sliding Window, Really?

Sliding Window is a technique where you maintain a **contiguous subarray** (a "window") and move it across the array/string by expanding or shrinking it from the ends. 

Instead of checking every possible subarray (which is O(n²)), you reuse calculations from the previous window. That's the magic.

---

## Why Sliding Window Matters

- Turns O(n²) solutions into O(n) or O(n log n)
- Extremely common in interviews (especially at FAANG)
- Works beautifully with strings, arrays, and subarray/substring problems
- Pairs perfectly with hashmaps, frequency counters, and two pointers

Once you get the intuition, a lot of "hard" problems suddenly become medium.

---

## Brute Force vs Sliding Window

| Aspect              | Brute Force             | Sliding Window              |
|---------------------|-------------------------|-----------------------------|
| Time Complexity     | O(n²) or worse          | O(n) typically              |
| Approach            | Check all subarrays     | Maintain one valid window   |
| Memory              | O(1)                    | O(1) or O(k) for freq maps  |
| Code Complexity     | Simple but slow         | Trickier but efficient      |

---

## How to Identify Sliding Window Problems

Look for these keywords:
- "Longest substring/subarray"
- "Smallest window that contains..."
- "Maximum/minimum sum of k consecutive elements"
- "At most K distinct characters"
- "Subarray with sum exactly K"
- "Minimum length subarray with sum >= target"

If the problem involves **contiguous** elements and asks for optimal length or count — sliding window is worth trying.

---

## Fixed Size Window

### Core Idea
Window size is fixed (usually given as `k`). Move the window one step at a time, adding new element and removing old one.

### Simple Intuition
Like looking through a fixed-size frame moving across a long picture. You only care about what's inside the frame right now.

### Step-by-Step
1. Initialize window from 0 to k-1
2. Compute result for first window
3. For each next position:
   - Add the new element (right)
   - Remove the element going out (left)
   - Update result

### Dry Run Example
**Problem**: Max sum of subarray of size 3  
`arr = [2, 1, 5, 1, 3, 2]`

- Window [2,1,5] → sum=8
- Slide → remove 2, add 1 → [1,5,1] sum=7
- Slide → remove 1, add 3 → [5,1,3] sum=9 ← max
- Slide → remove 5, add 2 → [1,3,2] sum=6

### Complexity
- **Time**: O(n)
- **Space**: O(1)

### Clean Implementation
```cpp
int maxSumFixed(vector<int>& arr, int k) {
    int n = arr.size();
    if (n < k) return -1;
    
    int maxSum = 0, windowSum = 0;
    
    // First window
    for (int i = 0; i < k; i++) {
        windowSum += arr[i];
    }
    maxSum = windowSum;
    
    // Slide the window
    for (int i = k; i < n; i++) {
        windowSum += arr[i] - arr[i - k];
        maxSum = max(maxSum, windowSum);
    }
    return maxSum;
}
```

**Edge cases**: k=1, k=n, n < k, all negative numbers.

---

## Dynamic / Variable Size Window

### Core Idea
Window size changes based on a condition. Expand right pointer, shrink left when condition breaks.

### Simple Intuition
You're trying to find the "best" window. Grow it as much as you can while it's valid, then shrink from left when it becomes invalid.

### Step-by-Step
1. Move right pointer to expand window
2. Check if window is still valid
3. While invalid → move left pointer to shrink
4. Update answer whenever window is valid

### Dry Run Example
**Problem**: Longest substring without repeating characters  
`s = "abcabcbb"`

- r=0: "a" → valid
- r=1: "ab" → valid
- r=2: "abc" → valid (len=3)
- r=3: "abca" → invalid (a repeats) → shrink left until valid
- Continue...

### Implementation
```cpp
int lengthOfLongestSubstring(string s) {
    unordered_map<char, int> lastSeen;
    int left = 0, maxLen = 0;
    
    for (int right = 0; right < s.size(); right++) {
        if (lastSeen.count(s[right]) && lastSeen[s[right]] >= left) {
            left = lastSeen[s[right]] + 1;
        }
        lastSeen[s[right]] = right;
        maxLen = max(maxLen, right - left + 1);
    }
    return maxLen;
}
```

**Time**: O(n)  
**Space**: O(min(n, charset_size))

---

## Two Pointers Connection

Sliding Window is basically a **special case of Two Pointers** where both pointers only move forward. 

- Two Pointers: more general (pointers can move in any way)
- Sliding Window: left only moves right when needed, right always moves right → monotonic movement

---

## Window Expansion & Shrinking Logic

**Key Principle**: 
- Right pointer always expands (greedily)
- Left pointer only shrinks when condition is violated
- Never move left backwards

**Common Mistake**: Shrinking left too early or too late.

---

## Common Sliding Window Patterns

1. **Fixed Size** — Max/Min sum, average, etc.
2. **Longest window with at most K constraint**
3. **Smallest window with all required characters**
4. **Subarray with sum == K or >= K**
5. **Frequency / Distinct elements**

---

## Frequency Map Based Window

Very common for string problems.

```cpp
// Smallest window containing all characters of T
string minWindow(string s, string t) {
    unordered_map<char, int> required, window;
    for (char c : t) required[c]++;
    
    int left = 0, valid = 0, minLen = INT_MAX, start = 0;
    
    for (int right = 0; right < s.size(); right++) {
        char c = s[right];
        window[c]++;
        if (required.count(c) && window[c] == required[c]) valid++;
        
        while (valid == required.size() && left <= right) {
            if (right - left + 1 < minLen) {
                minLen = right - left + 1;
                start = left;
            }
            char leftChar = s[left];
            window[leftChar]--;
            if (required.count(leftChar) && window[leftChar] < required[leftChar]) valid--;
            left++;
        }
    }
    return minLen == INT_MAX ? "" : s.substr(start, minLen);
}
```

---

## Longest / Smallest Subarray Problems

- **Longest**: Track max length when condition satisfied
- **Smallest**: Track min length when condition satisfied + shrink aggressively

**Pro tip**: For minimum length, shrink as soon as condition is met.

---

## String Based Sliding Window Problems

- Longest substring without repeating chars
- Minimum window substring
- Longest repeating character replacement
- Find all anagrams in a string

These usually need frequency maps or counters.

---

## Big Comparison Table

| Type                  | Time     | Space     | Use Case                          | Example Problems                  |
|-----------------------|----------|-----------|-----------------------------------|-----------------------------------|
| Fixed Size            | O(n)     | O(1)      | Max sum of k elements             | Max Average Subarray              |
| Variable Size         | O(n)     | O(1)/O(k) | Longest with at most K distinct   | Longest Substring w/o repeat      |
| Frequency Map         | O(n)     | O(charset)| Smallest window with all chars    | Minimum Window Substring          |
| Sum Based             | O(n)     | O(1)      | Subarray sum equals K             | Subarray Sum Equals K             |

---

## Common Beginner Mistakes

- Moving both pointers incorrectly (especially left pointer)
- Forgetting to update answer inside the while loop
- Infinite loops when left doesn't move properly
- Using `while` instead of `if` for shrinking (or vice versa)
- Not handling empty string/array
- Off-by-one when calculating length (`right - left` vs `right - left + 1`)

**Infinite loop fix**: Always ensure left moves forward when shrinking.

---

## Interview Insights

1. Always clarify constraints (string vs array, duplicates allowed?)
2. Start with brute force, then optimize to sliding window
3. Clearly explain when you expand and when you shrink
4. Mention time complexity improvement
5. Practice both fixed and variable size patterns

Common follow-ups:
- What if window can be non-contiguous?
- How would you modify for circular array?

---

## Real-World Usage

- Network packet processing (fixed window rate limiting)
- String matching and DNA sequence analysis
- Stock price analysis (max profit in k days)
- Video streaming (buffering windows)
- Anomaly detection in time series data

---

## When NOT to Use Sliding Window

- When subarrays don't need to be contiguous
- When order doesn't matter (use hashmap instead)
- Very small input size (brute force is fine)
- Complex conditions that can't be maintained incrementally

---

## Difference between Sliding Window and Two Pointers

- **Sliding Window**: Window is always contiguous, left moves only right
- **Two Pointers**: More general, can be used for pair sums, linked lists, etc.

Sliding Window = Optimized Two Pointers for contiguous segments.

---

## Cheat Sheet

```
Template:

int left = 0;
for(int right = 0; right < n; right++) {
    // add arr[right] to window
    
    while (window invalid && left <= right) {
        // remove arr[left] from window
        left++;
    }
    
    // update answer with current valid window
    ans = max(ans, right - left + 1);
}
```

**Quick Tips:**
- Right pointer expands greedily
- Left shrinks only when needed
- Update answer after every valid window
- Use freq map for characters / distinct count

---

*Built while preparing for interviews in Mumbai. Star if it helped you save hours of debugging!*

