# Binary Search in C++ — Beginner to Interview Level

> Personal notes from grinding LeetCode and interview prep. Went from getting stuck on off-by-one errors to confidently solving Binary Search on Answers problems. This is the guide I wish I had.

---

## Table of Contents

- [What is Binary Search, Really?](#what-is-binary-search-really)
- [Why Binary Search Matters](#why-binary-search-matters)
- [Linear vs Binary Search](#linear-vs-binary-search)
- [Preconditions](#preconditions)
- [How to Identify Binary Search Problems](#how-to-identify-binary-search-problems)
- [Binary Search Core Logic](#binary-search-core-logic)
- [Iterative Binary Search](#iterative-binary-search)
- [Recursive Binary Search](#recursive-binary-search)
- [Lower Bound & Upper Bound](#lower-bound--upper-bound)
- [First and Last Occurrence](#first-and-last-occurrence)
- [Binary Search on Answers](#binary-search-on-answers)
- [STL Functions](#stl-functions)
- [Big Comparison & Patterns](#big-comparison--patterns)
- [Common Beginner Mistakes](#common-beginner-mistakes)
- [Interview Insights](#interview-insights)
- [Real-World Usage](#real-world-usage)
- [Cheat Sheet](#cheat-sheet)

---

## What is Binary Search, Really?

Binary Search is the art of repeatedly dividing the search space in half by asking one smart question: *"Is the answer in the left half or the right half?"*

It's not magic — it works because the data has order. Once you exploit that order, you go from checking every element to checking just a few.

---

## Why Binary Search Matters

- Turns O(n) searches into O(log n) — massive difference on large inputs
- Foundation for tons of interview problems (especially "medium" and "hard")
- Binary Search on Answers unlocks optimization problems that look impossible at first
- Shows up everywhere once you start seeing the pattern

In interviews, if a problem mentions "sorted" or "minimize/maximize something", your brain should immediately think binary search.

---

## Linear vs Binary Search

| Aspect              | Linear Search      | Binary Search          |
|---------------------|--------------------|------------------------|
| Time Complexity     | O(n)               | O(log n)               |
| Data Requirement    | Any                | Sorted                 |
| Best Case           | O(1)               | O(1)                   |
| Worst Case          | O(n)               | O(log n)               |
| Space               | O(1)               | O(1)                   |
| Real Use            | Small / unsorted   | Large sorted data      |

---

## Preconditions

1. Array must be **sorted** (ascending or descending)
2. O(1) random access (arrays/vectors, not linked lists)
3. Clear way to decide "go left" or "go right"

---

## How to Identify Binary Search Problems

Look for these signals:
- "Given a sorted array..."
- "Find the kth smallest/largest"
- "Minimum in rotated sorted array"
- "First/last occurrence of X"
- "Minimize the maximum" or "Maximize the minimum"
- Search in 2D matrix, peak finding, etc.

If sorting the input makes the problem easier — consider binary search after.

---

## Binary Search Core Logic

The heart of it:

```cpp
while (low <= high) {
    int mid = low + (high - low) / 2;  // Avoid overflow
    
    if (arr[mid] == target) {
        // Found it
    } else if (arr[mid] < target) {
        low = mid + 1;   // Answer is in right half
    } else {
        high = mid - 1;  // Answer is in left half
    }
}
```

**Golden Rule**: Always use `low + (high - low) / 2`

---

## Iterative Binary Search

### Core Idea
Keep narrowing the search window until you find the target or the window collapses.

### Intuition
Instead of scanning left to right, jump to the middle and throw away half the array every time.

### Implementation

```cpp
int binarySearch(vector<int>& arr, int target) {
    int low = 0;
    int high = arr.size() - 1;
    
    while (low <= high) {
        int mid = low + (high - low) / 2;
        
        if (arr[mid] == target) return mid;
        if (arr[mid] < target) {
            low = mid + 1;
        } else {
            high = mid - 1;
        }
    }
    return -1;
}
```

**Time**: O(log n)  
**Space**: O(1)

**Edge Cases**: Empty array, single element, target not present, duplicates.

---

## Recursive Binary Search

```cpp
int binarySearchRec(vector<int>& arr, int low, int high, int target) {
    if (low > high) return -1;
    
    int mid = low + (high - low) / 2;
    
    if (arr[mid] == target) return mid;
    if (arr[mid] < target)
        return binarySearchRec(arr, mid + 1, high, target);
    return binarySearchRec(arr, low, mid - 1, target);
}
```

**Note**: Iterative is usually preferred in interviews (stack safety).

---

## Lower Bound & Upper Bound

### Lower Bound
Smallest index where `arr[i] >= target`

```cpp
int lowerBound(vector<int>& arr, int target) {
    int low = 0, high = arr.size() - 1;
    int ans = arr.size();
    
    while (low <= high) {
        int mid = low + (high - low) / 2;
        if (arr[mid] >= target) {
            ans = mid;
            high = mid - 1;
        } else {
            low = mid + 1;
        }
    }
    return ans;
}
```

### Upper Bound
Smallest index where `arr[i] > target`

(Change condition to `arr[mid] > target`)

---

## First and Last Occurrence

```cpp
pair<int,int> firstLastOccurrence(vector<int>& arr, int target) {
    int first = lowerBound(arr, target);
    if (first == arr.size() || arr[first] != target) 
        return {-1, -1};
    
    int last = upperBound(arr, target) - 1;  // upperBound returns first > target
    return {first, last};
}
```

---

## Binary Search on Answers

**This is the killer pattern.**

### Core Idea
When you need to find the optimal value (minimize something or maximize something) and you can check if a value is feasible.

### Intuition
Instead of searching the array, you search the **range of possible answers**.

Example: "Minimum eating speed so that Koko can eat all bananas in H hours"

Search space = [1, max(bananas)]

For each mid speed, check if it's possible to finish in H hours.

```cpp
int minEatingSpeed(vector<int>& piles, int h) {
    int low = 1;
    int high = *max_element(piles.begin(), piles.end());
    int ans = high;
    
    while (low <= high) {
        int mid = low + (high - low) / 2;
        if (canEatInTime(piles, mid, h)) {
            ans = mid;
            high = mid - 1;   // Try slower
        } else {
            low = mid + 1;    // Need faster
        }
    }
    return ans;
}
```

**Key**: Write a good `isPossible(mid)` function.

---

## STL Functions

```cpp
vector<int> arr = {1, 3, 3, 5, 7};

// Returns bool
bool exists = binary_search(arr.begin(), arr.end(), 3);

// Lower bound (first >= )
auto lb = lower_bound(arr.begin(), arr.end(), 3);

// Upper bound (first > )
auto ub = upper_bound(arr.begin(), arr.end(), 3);

int first = lb - arr.begin();
int last = (ub - arr.begin()) - 1;
```

**Pro tip**: These work beautifully with custom comparators too.

---

## Big Comparison & Patterns

| Variant                    | Use Case                          | Key Change                     |
|---------------------------|-----------------------------------|--------------------------------|
| Standard Search           | Exact match                       | Return when equal              |
| Lower Bound               | First >= target                   | Update ans and search left     |
| Upper Bound               | First > target                    | Similar but strict >           |
| Binary Search on Answers  | Optimization (min/max)            | Search answer space + feasibility check |
| Rotated Sorted Array      | Search in rotated array           | Handle rotation logic          |

---

## Common Beginner Mistakes

- Using `low + high / 2` → integer overflow (use `low + (high-low)/2`)
- `while(low < high)` without careful handling → infinite loops or missed elements
- Forgetting to update answer variable in lower bound style problems
- Not handling duplicates properly
- Wrong boundary updates (`mid` instead of `mid+1` / `mid-1`)
- Ignoring edge cases like all elements same, target outside range

**Infinite loop tip**: Stick with `low <= high` for standard search.

---

## Interview Insights

1. Always ask: "Is the array sorted?"
2. Discuss time & space complexity early
3. For Binary Search on Answers — clearly explain the search space and feasibility function
4. Practice rotated array, 2D matrix, and peak finding problems
5. Mention tradeoffs between iterative and recursive

**Common LeetCode patterns**:
- Search in Rotated Sorted Array
- Find Minimum in Rotated Sorted Array
- Koko Eating Bananas
- Capacity To Ship Packages
- First Bad Version

---

## Real-World Usage

- Database indexing and queries
- Finding closest match in large datasets
- Version control ("first bad commit")
- Game AI pathfinding optimizations
- Load balancing, resource allocation
- Any place where you need fast lookup in ordered data

---

## Cheat Sheet

```
Standard Template:
low = 0, high = n-1
while (low <= high) {
    mid = low + (high - low) / 2
    if (condition) high = mid - 1;
    else low = mid + 1;
}

Lower Bound: Track smallest valid index, search left when found
Binary on Answers: low = minPossible, high = maxPossible + feasibility check
```

**Remember**:
- Sorted? → Think Binary Search
- Minimize maximum? → Binary Search on Answers

---

## Contributing

Suggestions, optimizations, or more patterns (like search in 2D matrix) are welcome!

---

*Built while preparing in Mumbai. If this helped, star the repo ⭐*
