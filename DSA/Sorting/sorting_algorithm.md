# Sorting Algorithms in C++ — Beginner to Interview Level

> Personal notes + implementations I built while grinding DSA. Covers everything from "what even is sorting" to the stuff that actually shows up in interviews. If you're preparing for placements or just want to finally *get* how these work — this is for you.

---

## Table of Contents

- [What is Sorting, Really?](#what-is-sorting-really)
- [Why Sorting Matters](#why-sorting-matters)
- [How to Spot a Sorting Problem](#how-to-spot-a-sorting-problem)
- [Bubble Sort](#bubble-sort)
- [Selection Sort](#selection-sort)
- [Insertion Sort](#insertion-sort)
- [Merge Sort](#merge-sort)
- [Quick Sort](#quick-sort)
- [STL sort()](#stl-sort)
- [Big Comparison Table](#big-comparison-table)
- [When to Use What](#when-to-use-what)
- [Common Beginner Mistakes](#common-beginner-mistakes)
- [Interview Insights](#interview-insights)
- [Real-World Usage](#real-world-usage)
- [Cheat Sheet](#cheat-sheet)

---

## What is Sorting, Really?

Sorting is just arranging elements in a specific order — usually ascending or descending. That's it. The interesting part is *how* you do it, and what tradeoffs you're making.

Every sorting algorithm answers the same question differently: **"How do I figure out which element goes where?"**

Some compare elements pairwise. Some use the structure of the data. Some sacrifice memory for speed. Understanding that these are design decisions — not magic — is what separates someone who memorized algorithms from someone who actually understands them.

---

## Why Sorting Matters

You'd be surprised how often problems become trivial once the data is sorted.

- Binary search only works on sorted data
- Finding duplicates becomes O(n) after sorting instead of O(n²)
- Two-pointer technique usually needs sorted arrays
- Merging intervals, finding closest pair, kth element — all easier when sorted
- Databases sort constantly (indices, ORDER BY queries, join operations)

In interviews, if you're stuck, sorting the input is often a useful first step. It's a standard trick.

---

## How to Spot a Sorting Problem

Not every problem explicitly says "sort this." Here are signals to look for:

- Problem asks for elements in order, or "find the kth smallest/largest"
- You need to compare adjacent elements or find pairs
- The brute force involves nested loops comparing everything to everything
- Input is described as "unsorted" (hint hint)
- You're dealing with intervals, ranges, or scheduling
- Problem mentions "minimum difference" between elements

If you see any of these, consider sorting first and checking if it simplifies things.

---

## Bubble Sort

### Core Idea
Repeatedly swap adjacent elements if they're in the wrong order. The largest element "bubbles up" to its correct position after each pass.

### Intuition
Imagine pushing the heaviest bubble to the top of water. After one full pass, the largest element is guaranteed to be at the end. After two passes, the two largest are sorted. Keep going.

### Step-by-Step

1. Start from index 0
2. Compare `arr[i]` and `arr[i+1]`
3. If `arr[i] > arr[i+1]`, swap them
4. Move to next pair
5. After one full pass, the last element is in place
6. Repeat, shrinking the unsorted region by 1 each time
7. If no swaps happened in a pass → array is already sorted (early exit)

### Dry Run

```
arr = [5, 3, 8, 1]

Pass 1:
[5,3] → swap → [3,5,8,1]
[5,8] → no swap
[8,1] → swap → [3,5,1,8]   ← 8 is in place

Pass 2:
[3,5] → no swap
[5,1] → swap → [3,1,5,8]   ← 8 still in place

Pass 3:
[3,1] → swap → [1,3,5,8]   ← done

Result: [1, 3, 5, 8]
```

### Complexity

| | Best | Average | Worst |
|---|---|---|---|
| Time | O(n) | O(n²) | O(n²) |
| Space | O(1) | O(1) | O(1) |

> Best case O(n) only with the early-exit optimization (check if no swaps happened).

- **Stable:** ✅ Yes — equal elements never swap, so relative order is preserved
- **In-place:** ✅ Yes

### Pros & Cons

**Pros:**
- Simplest to implement and understand
- Detects already-sorted arrays in O(n) with optimization
- No extra memory needed

**Cons:**
- O(n²) on average — unusable for large inputs
- Does more swaps than Selection Sort for the same result
- Nobody uses this in production (except maybe embedded systems with tiny n)

### Implementation

```cpp
#include <iostream>
#include <vector>
using namespace std;

void bubbleSort(vector<int>& arr) {
    int n = arr.size();
    for (int i = 0; i < n - 1; i++) {
        bool swapped = false;
        for (int j = 0; j < n - i - 1; j++) {
            if (arr[j] > arr[j + 1]) {
                swap(arr[j], arr[j + 1]);
                swapped = true;
            }
        }
        // Early exit: already sorted
        if (!swapped) break;
    }
}

int main() {
    vector<int> arr = {64, 34, 25, 12, 22, 11, 90};
    bubbleSort(arr);
    for (int x : arr) cout << x << " ";
    // Output: 11 12 22 25 34 64 90
    return 0;
}
```

---

## Selection Sort

### Core Idea
In each pass, find the minimum element from the unsorted portion and place it at the beginning of that portion.

### Intuition
You're selecting the smallest element and putting it where it belongs. Unlike Bubble Sort which swaps constantly, Selection Sort does exactly **n-1 swaps** total — one per pass.

### Step-by-Step

1. Consider the whole array as unsorted
2. Find the minimum element in the unsorted portion
3. Swap it with the first element of the unsorted portion
4. Shrink the unsorted portion by 1 from the left
5. Repeat

### Dry Run

```
arr = [29, 10, 14, 37, 13]

Pass 1: min = 10 (index 1) → swap with index 0
→ [10, 29, 14, 37, 13]

Pass 2: min = 13 (index 4) → swap with index 1
→ [10, 13, 14, 37, 29]

Pass 3: min = 14 (index 2) → already in place
→ [10, 13, 14, 37, 29]

Pass 4: min = 29 (index 4) → swap with index 3
→ [10, 13, 14, 29, 37]

Result: [10, 13, 14, 29, 37]
```

### Complexity

| | Best | Average | Worst |
|---|---|---|---|
| Time | O(n²) | O(n²) | O(n²) |
| Space | O(1) | O(1) | O(1) |

> No early exit possible — it always does the same number of comparisons regardless of input.

- **Stable:** ❌ No — swapping can change the relative order of equal elements
- **In-place:** ✅ Yes

### Pros & Cons

**Pros:**
- Minimum number of swaps (O(n) swaps) — useful when writes/swaps are expensive
- Simple to implement
- Works well for small arrays

**Cons:**
- Always O(n²) — can't detect sorted input
- Not stable
- Worse than Insertion Sort in practice for nearly-sorted data

### Implementation

```cpp
#include <iostream>
#include <vector>
using namespace std;

void selectionSort(vector<int>& arr) {
    int n = arr.size();
    for (int i = 0; i < n - 1; i++) {
        int minIdx = i;
        for (int j = i + 1; j < n; j++) {
            if (arr[j] < arr[minIdx]) {
                minIdx = j;
            }
        }
        if (minIdx != i) {
            swap(arr[i], arr[minIdx]);
        }
    }
}

int main() {
    vector<int> arr = {29, 10, 14, 37, 13};
    selectionSort(arr);
    for (int x : arr) cout << x << " ";
    // Output: 10 13 14 29 37
    return 0;
}
```

---

## Insertion Sort

### Core Idea
Build a sorted subarray one element at a time. For each new element, insert it into its correct position within the already-sorted portion.

### Intuition
Think of how you sort playing cards in your hand. You pick up a new card and slide it into the right place among the cards you're already holding. That's insertion sort.

### Step-by-Step

1. Start with the second element (first element is trivially sorted)
2. Pick the current element as `key`
3. Compare it with elements to its left, shifting them right until you find its correct position
4. Insert `key` there
5. Move to the next element

### Dry Run

```
arr = [5, 2, 4, 6, 1, 3]

i=1: key=2, 5>2 → shift → [5,5,4,6,1,3] → insert → [2,5,4,6,1,3]
i=2: key=4, 5>4 → shift → [2,5,5,6,1,3] → insert → [2,4,5,6,1,3]
i=3: key=6, 5<6 → no shift → [2,4,5,6,1,3]
i=4: key=1, all shift right → [2,4,5,6,6,3] → [2,4,5,5,6,3] → ... → insert → [1,2,4,5,6,3]
i=5: key=3, shift until 2 → insert → [1,2,3,4,5,6]

Result: [1, 2, 3, 4, 5, 6]
```

### Complexity

| | Best | Average | Worst |
|---|---|---|---|
| Time | O(n) | O(n²) | O(n²) |
| Space | O(1) | O(1) | O(1) |

> Best case is O(n) for already-sorted input — it just scans without shifting.

- **Stable:** ✅ Yes
- **In-place:** ✅ Yes

### Pros & Cons

**Pros:**
- Fast on nearly-sorted data (real-world inputs are often nearly sorted)
- Online algorithm — can sort as elements arrive, one at a time
- Low overhead; great for small n
- Used inside Timsort and Introsort for small subarrays

**Cons:**
- O(n²) worst case
- Poor on reverse-sorted data

### Implementation

```cpp
#include <iostream>
#include <vector>
using namespace std;

void insertionSort(vector<int>& arr) {
    int n = arr.size();
    for (int i = 1; i < n; i++) {
        int key = arr[i];
        int j = i - 1;
        while (j >= 0 && arr[j] > key) {
            arr[j + 1] = arr[j];
            j--;
        }
        arr[j + 1] = key;
    }
}

int main() {
    vector<int> arr = {5, 2, 4, 6, 1, 3};
    insertionSort(arr);
    for (int x : arr) cout << x << " ";
    // Output: 1 2 3 4 5 6
    return 0;
}
```

---

## Merge Sort

### Core Idea
Divide the array in half recursively until you have subarrays of size 1. Then merge them back together in sorted order.

### Intuition
A single element is trivially sorted. If you have two sorted halves, merging them into one sorted array is easy — just compare the fronts and pick the smaller one. Merge Sort builds on this.

### Step-by-Step

1. If array has 1 or 0 elements, return (base case)
2. Find the midpoint and split into left and right halves
3. Recursively sort the left half
4. Recursively sort the right half
5. Merge the two sorted halves into one sorted array

**Merging two sorted arrays:**
- Use two pointers, one for each half
- Pick the smaller element and advance that pointer
- Copy remaining elements when one pointer exhausts its half

### Dry Run

```
arr = [38, 27, 43, 3, 9, 82, 10]

Split:
[38, 27, 43, 3] | [9, 82, 10]
[38,27] [43,3]   [9,82] [10]
[38][27] [43][3] [9][82] [10]

Merge up:
[27,38]  [3,43]  [9,82]  [10]
[3,27,38,43]     [9,10,82]
[3,9,10,27,38,43,82]
```

### Complexity

| | Best | Average | Worst |
|---|---|---|---|
| Time | O(n log n) | O(n log n) | O(n log n) |
| Space | O(n) | O(n) | O(n) |

> Guaranteed O(n log n) always — no bad inputs. The O(n) space is for the temporary arrays during merging.

- **Stable:** ✅ Yes — equal elements maintain their relative order during merging
- **In-place:** ❌ No (standard implementation)

### Pros & Cons

**Pros:**
- Consistent O(n log n) in all cases
- Stable sort
- Works well for linked lists (no random access needed)
- Naturally suited for external sorting (sorting data that doesn't fit in RAM)

**Cons:**
- Requires O(n) extra space
- Slower in practice than Quick Sort for in-memory data due to cache performance
- Recursion stack overhead

### Implementation

```cpp
#include <iostream>
#include <vector>
using namespace std;

void merge(vector<int>& arr, int left, int mid, int right) {
    int n1 = mid - left + 1;
    int n2 = right - mid;

    vector<int> L(arr.begin() + left, arr.begin() + mid + 1);
    vector<int> R(arr.begin() + mid + 1, arr.begin() + right + 1);

    int i = 0, j = 0, k = left;
    while (i < n1 && j < n2) {
        if (L[i] <= R[j]) arr[k++] = L[i++];
        else arr[k++] = R[j++];
    }
    while (i < n1) arr[k++] = L[i++];
    while (j < n2) arr[k++] = R[j++];
}

void mergeSort(vector<int>& arr, int left, int right) {
    if (left >= right) return;
    int mid = left + (right - left) / 2;
    mergeSort(arr, left, mid);
    mergeSort(arr, mid + 1, right);
    merge(arr, left, mid, right);
}

int main() {
    vector<int> arr = {38, 27, 43, 3, 9, 82, 10};
    mergeSort(arr, 0, arr.size() - 1);
    for (int x : arr) cout << x << " ";
    // Output: 3 9 10 27 38 43 82
    return 0;
}
```

---

## Quick Sort

### Core Idea
Pick a pivot element. Partition the array so everything less than the pivot is on the left and everything greater is on the right. Recursively sort both sides.

### Intuition
After partitioning, the pivot is in its final sorted position. You don't need to move it again. You've reduced the problem to sorting two smaller arrays — independently. That's the recursive insight.

### Step-by-Step (Lomuto Partition)

1. Choose the last element as pivot
2. Initialize `i = low - 1` (index of smaller element boundary)
3. For each element from `low` to `high-1`:
   - If element ≤ pivot: increment `i`, swap `arr[i]` with current element
4. Swap pivot (`arr[high]`) with `arr[i+1]`
5. Pivot is now in its correct position at index `i+1`
6. Recursively sort left and right subarrays

### Dry Run

```
arr = [10, 80, 30, 90, 40, 50, 70], pivot = 70

i = -1
j=0: 10 ≤ 70 → i=0, swap arr[0] and arr[0] → [10,80,30,90,40,50,70]
j=1: 80 > 70 → skip
j=2: 30 ≤ 70 → i=1, swap arr[1] and arr[2] → [10,30,80,90,40,50,70]
j=3: 90 > 70 → skip
j=4: 40 ≤ 70 → i=2, swap arr[2] and arr[4] → [10,30,40,90,80,50,70]
j=5: 50 ≤ 70 → i=3, swap arr[3] and arr[5] → [10,30,40,50,80,90,70]

Place pivot: swap arr[4] and arr[6] → [10,30,40,50,70,90,80]
                                                    ^
                                               pivot in place

Recursively sort [10,30,40,50] and [90,80]
```

### Complexity

| | Best | Average | Worst |
|---|---|---|---|
| Time | O(n log n) | O(n log n) | O(n²) |
| Space | O(log n) | O(log n) | O(n) |

> Worst case happens when the pivot is always the smallest or largest element — like sorted input with bad pivot choice. Randomized pivot selection avoids this in practice.

- **Stable:** ❌ No
- **In-place:** ✅ Yes (O(log n) stack space for recursion)

### Pros & Cons

**Pros:**
- Fastest in practice for average case (cache-friendly, in-place)
- O(log n) space (way better than Merge Sort's O(n))
- Used in most standard library implementations (as part of Introsort)

**Cons:**
- O(n²) worst case with bad pivot selection
- Not stable
- Tricky to implement correctly (off-by-one errors are very common)
- Recursive — stack overflow risk on extremely large inputs without optimization

### Implementation

```cpp
#include <iostream>
#include <vector>
using namespace std;

int partition(vector<int>& arr, int low, int high) {
    int pivot = arr[high];
    int i = low - 1;

    for (int j = low; j < high; j++) {
        if (arr[j] <= pivot) {
            i++;
            swap(arr[i], arr[j]);
        }
    }
    swap(arr[i + 1], arr[high]);
    return i + 1;
}

void quickSort(vector<int>& arr, int low, int high) {
    if (low < high) {
        int pi = partition(arr, low, high);
        quickSort(arr, low, pi - 1);
        quickSort(arr, pi + 1, high);
    }
}

int main() {
    vector<int> arr = {10, 80, 30, 90, 40, 50, 70};
    quickSort(arr, 0, arr.size() - 1);
    for (int x : arr) cout << x << " ";
    // Output: 10 30 40 50 70 80 90
    return 0;
}
```

**Randomized pivot (recommended for interviews):**

```cpp
int partition(vector<int>& arr, int low, int high) {
    // Random pivot to avoid worst case on sorted input
    int randIdx = low + rand() % (high - low + 1);
    swap(arr[randIdx], arr[high]);

    int pivot = arr[high];
    int i = low - 1;
    for (int j = low; j < high; j++) {
        if (arr[j] <= pivot) {
            i++;
            swap(arr[i], arr[j]);
        }
    }
    swap(arr[i + 1], arr[high]);
    return i + 1;
}
```

---

## STL sort()

### Core Idea
`std::sort()` in C++ uses **Introsort** — a hybrid of Quick Sort, Heap Sort, and Insertion Sort. It's engineered to avoid worst-case scenarios while staying fast in practice.

- Uses Quick Sort for the main sorting
- Switches to Heap Sort if recursion depth gets too deep (prevents O(n²) on bad pivots)
- Switches to Insertion Sort for small subarrays (< ~16 elements) since it's faster there

### Usage

```cpp
#include <algorithm>
#include <vector>
using namespace std;

// Basic: ascending order
sort(arr.begin(), arr.end());

// Descending order
sort(arr.begin(), arr.end(), greater<int>());

// Custom comparator (sort by absolute value)
sort(arr.begin(), arr.end(), [](int a, int b) {
    return abs(a) < abs(b);
});

// Sort array (C-style)
int arr[] = {5, 2, 8, 1};
sort(arr, arr + 4);

// Sort only part of the array
sort(arr.begin() + 2, arr.begin() + 6); // sort indices [2, 6)

// Stable version — use stable_sort() if order of equal elements matters
stable_sort(arr.begin(), arr.end());
```

### Custom Comparator for Structs

```cpp
struct Student {
    string name;
    int grade;
};

vector<Student> students = {{"Alice", 90}, {"Bob", 85}, {"Charlie", 90}};

// Sort by grade descending, then name ascending
sort(students.begin(), students.end(), [](const Student& a, const Student& b) {
    if (a.grade != b.grade) return a.grade > b.grade;
    return a.name < b.name;
});
```

### Complexity

| | Time | Space | Stable |
|---|---|---|---|
| `sort()` | O(n log n) avg/worst | O(log n) | ❌ No |
| `stable_sort()` | O(n log² n) or O(n log n) with extra memory | O(n) or O(log n) | ✅ Yes |

> In almost all competitive programming and interview code, just use `sort()`. Only reach for `stable_sort()` when stability actually matters.

---

## Big Comparison Table

| Algorithm | Best | Average | Worst | Space | Stable | In-place |
|-----------|------|---------|-------|-------|--------|----------|
| Bubble Sort | O(n) | O(n²) | O(n²) | O(1) | ✅ | ✅ |
| Selection Sort | O(n²) | O(n²) | O(n²) | O(1) | ❌ | ✅ |
| Insertion Sort | O(n) | O(n²) | O(n²) | O(1) | ✅ | ✅ |
| Merge Sort | O(n log n) | O(n log n) | O(n log n) | O(n) | ✅ | ❌ |
| Quick Sort | O(n log n) | O(n log n) | O(n²) | O(log n) | ❌ | ✅ |
| STL sort() | O(n log n) | O(n log n) | O(n log n) | O(log n) | ❌ | ✅ |

---

## When to Use What

| Situation | Best Choice | Why |
|-----------|-------------|-----|
| General purpose, large input | `std::sort()` | Fastest in practice |
| Need stable sort | `std::stable_sort()` or Merge Sort | Preserves order of equal elements |
| Nearly sorted data | Insertion Sort | O(n) on sorted, low overhead |
| Very small arrays (n < 20) | Insertion Sort | Low constant factor beats everything |
| Memory is tight | Quick Sort | O(log n) stack only |
| External sorting (disk) | Merge Sort | Sequential access pattern, works in passes |
| Sorting linked list | Merge Sort | No random access needed |
| Counting swaps | Selection Sort | Exactly n-1 swaps |
| Writing sort from scratch in interview | Merge Sort or Quick Sort | Shows you understand divide-and-conquer |

---

## Common Beginner Mistakes

**Bubble Sort**
- Forgetting the early-exit `swapped` flag — you're missing out on O(n) best case
- Using `j < n` instead of `j < n - i - 1` — leads to index out of bounds or redundant comparisons

**Selection Sort**
- Swapping even when `minIdx == i` — unnecessary, but harmless
- Confusing Selection Sort as stable — it isn't; swapping disrupts relative order

**Insertion Sort**
- Using `arr[j] > key` with `>=` makes it unstable (equal elements shift unnecessarily)
- Off-by-one in the while condition: `j >= 0` is crucial, don't write `j > 0`

**Merge Sort**
- Calculating mid as `(left + right) / 2` can overflow for large indices — use `left + (right - left) / 2`
- Not handling the base case properly (`left >= right`, not just `left == right`)
- Modifying the original array indices incorrectly in the merge step

**Quick Sort**
- Always choosing first or last element as pivot on sorted input → guaranteed O(n²)
- Getting the partition boundaries wrong — most bugs come from `<=` vs `<` in the loop
- Forgetting to handle `low < high` check in recursive calls

**STL sort()**
- Passing wrong iterators (`arr.end() - 1` instead of `arr.end()`)
- Writing comparators that aren't strict weak ordering (e.g., `<=` instead of `<`) — causes undefined behavior

---

## Interview Insights

**Common questions:**

1. **"Why is Quick Sort preferred over Merge Sort in practice?"**
   - Better cache performance (in-place), lower space usage (O(log n) vs O(n)), and similar average-case performance.

2. **"When would you use Merge Sort over Quick Sort?"**
   - When stability is required. When sorting linked lists. When worst-case O(n log n) is a hard requirement. When sorting data that doesn't fit in memory.

3. **"What's the best sorting algorithm?"**
   - It depends. This is the correct answer. Discuss tradeoffs — there's no universally best algorithm.

4. **"Can you sort in O(n)?"**
   - Yes, for special cases: Counting Sort, Radix Sort, Bucket Sort. These aren't comparison-based, so they bypass the O(n log n) lower bound for comparison sorts.

5. **"What's the lower bound for comparison-based sorting?"**
   - O(n log n). Any algorithm that sorts only by comparing elements must do at least this many comparisons.

**Quick tips:**
- If asked to implement a sort, always clarify if stability matters
- Mention time complexity, space complexity, and stability unprompted — it shows depth
- For large, random data → Quick Sort. For data with structure → think before sorting
- Interviewers often ask about the worst case of Quick Sort — know it and know how randomization helps

---

## Real-World Usage

| Context | Algorithm Used | Reason |
|---------|---------------|--------|
| C++ `std::sort()` | Introsort (QS + HS + IS) | Best overall performance |
| Python `sorted()`, Java `Arrays.sort(objects)` | Timsort (MS + IS) | Stable, fast on real-world data |
| Java `Arrays.sort(primitives)` | Dual-pivot Quick Sort | Cache performance for primitives |
| Database ORDER BY | Various (often merge-based) | Stability + external sort support |
| Linux kernel | Heapsort | No stack overflow risk in kernel space |
| Git diff | Patience diff (merge-based) | Optimized for human-readable diffs |

Real-world sorts are almost never "pure" implementations of one algorithm. They're hybrid — switching strategies based on input size and characteristics. Timsort, for example, detects natural runs in the data and merges them. That's why it's blazing fast on partially sorted data.

---

## Cheat Sheet

```
┌─────────────────────────────────────────────────────────────────┐
│                  SORTING QUICK REFERENCE                        │
├──────────────┬──────────────┬─────────┬───────┬────────────────┤
│  Algorithm   │    Time      │  Space  │Stable │  Best For      │
├──────────────┼──────────────┼─────────┼───────┼────────────────┤
│ Bubble       │ O(n²)        │  O(1)   │  Yes  │ Teaching only  │
│ Selection    │ O(n²)        │  O(1)   │  No   │ Min swaps      │
│ Insertion    │ O(n) – O(n²) │  O(1)   │  Yes  │ Small/sorted   │
│ Merge        │ O(n log n)   │  O(n)   │  Yes  │ Stable, lists  │
│ Quick        │ O(n log n)*  │ O(logn) │  No   │ General fast   │
│ STL sort()   │ O(n log n)   │ O(logn) │  No   │ Default choice │
└──────────────┴──────────────┴─────────┴───────┴────────────────┘
* Quick Sort worst case is O(n²) — use randomized pivot
```

**One-liners to remember:**

- Bubble: keep swapping neighbors if out of order
- Selection: find min, put it in place, repeat
- Insertion: pick card, slide it into your hand
- Merge: split till size 1, merge back sorted
- Quick: pivot in place, recurse on both sides
- STL: just use it unless you have a good reason not to

---

## Contributing

Found a bug or have a cleaner implementation? PRs welcome. If you want to add Heap Sort, Counting Sort, or Radix Sort — there's a natural next section for that.

---

*Built while studying for placement season. If this helped you, give it a star ⭐*
