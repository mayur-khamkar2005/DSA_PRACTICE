# Recursion in C++ — Beginner to Advanced Interview Level

> My personal notes after finally "getting" recursion. Spent countless hours staring at call stacks and recursion trees before things clicked. This is the guide I wish I had when I was scared of recursive functions.

---

## Table of Contents

- [Introduction to Recursion](#introduction-to-recursion)
- [Understanding the Call Stack](#understanding-the-call-stack)
- [Base Case & Recursive Case](#base-case-understanding)
- [Recursive Thinking Framework](#recursive-thinking-framework)
- [Basic Recursive Problems](#basic-recursive-problems)
- [Recursion on Arrays & Strings](#recursion-on-arrays)
- [Advanced Concepts](#advanced-concepts)
- [How to Identify Recursion Problems](#how-to-identify-recursion-problems)
- [Common Mistakes](#common-beginner-mistakes)
- [Recursion vs Iteration](#recursion-vs-iteration)
- [Practice Problems](#practice-problems)
- [Quick Revision Cheat Sheet](#final-revision)

---

## Introduction to Recursion

Recursion is when a function calls itself to solve smaller versions of the same problem. Sounds simple, but it took me a while to stop fearing it.

**Real-world intuition**: Think of Russian nesting dolls. To open the biggest doll, you open the next smaller one, and so on, until you reach the tiniest doll (base case). Then you come back out putting everything together.

Recursion exists because many problems have a natural **self-similar** structure — trees, directories, divide and conquer algorithms, etc.

**Why it feels difficult initially**:
- Hard to visualize the call stack
- Fear of infinite recursion
- Difficulty trusting that the recursive call will return the right answer

---

## Understanding the Call Stack

Every recursive call creates a new **stack frame** with its own local variables and parameters.

```cpp
void print(int n) {
    if (n == 0) return;        // base case
    cout << n << " ";          // work before recursive call
    print(n-1);                // recursive call
    cout << n << " ";          // work after recursive call (returning phase)
}
```

**Execution flow for print(3)**:

1. print(3) → prints 3, calls print(2)
2. print(2) → prints 2, calls print(1)
3. print(1) → prints 1, calls print(0)
4. print(0) → base case, returns
5. Back to print(1) → prints 1 (returning phase)
6. Back to print(2) → prints 2
7. Back to print(3) → prints 3

**Output**: `3 2 1 1 2 3`

**Mental model**: The call stack grows during the "going down" phase and shrinks during the "coming back up" phase.

---

## Base Case Understanding

The base case is your **stopping condition**. Without it, you get infinite recursion and stack overflow.

**Good base cases**:
- Smallest possible input (n == 0, head == nullptr, empty string)
- Problem that can be solved directly without further recursion

**Common mistake**: Writing base case that is too broad or missing some edge cases.

---

## Recursive Thinking Framework

1. **Identify the smallest subproblem** (base case)
2. **Assume the recursive call works** for smaller input (this is the key leap of faith)
3. **Define how to combine results** from recursive calls
4. **Reduce the problem size** in every call

**Example**: Sum of first n natural numbers

- Base: sum(0) = 0
- Recursive: sum(n) = n + sum(n-1)

---

## Basic Recursive Problems

### Factorial

```cpp
long long factorial(int n) {
    if (n == 0 || n == 1) return 1;     // base case
    return n * factorial(n - 1);        // recursive case
}
```

**Recursion Tree**:
```
factorial(4)
   → 4 * factorial(3)
        → 3 * factorial(2)
             → 2 * factorial(1)
                  → 1
```

### Fibonacci (Classic example of inefficiency)

```cpp
int fib(int n) {
    if (n <= 1) return n;
    return fib(n-1) + fib(n-2);
}
```

**Observation**: This has exponential time because of massive overlapping subproblems → leads naturally into DP.

---

## Recursion on Arrays

### Reverse Array (In-place)

```cpp
void reverseArray(vector<int>& arr, int left, int right) {
    if (left >= right) return;           // base case
    
    swap(arr[left], arr[right]);
    reverseArray(arr, left + 1, right - 1);
}
```

**Dry Run** on `[1, 2, 3, 4]`:
- Call(0,3) → swap 1↔4 → [4,2,3,1] → Call(1,2)
- Call(1,2) → swap 2↔3 → [4,3,2,1] → Call(2,1) → base case

---

## Recursion on Strings

### Generate All Subsequences

This is where recursion starts showing its power.

```cpp
void generateSubsequences(string s, int index, string current) {
    if (index == s.length()) {
        cout << current << endl;
        return;
    }
    
    // Exclude current character
    generateSubsequences(s, index + 1, current);
    
    // Include current character
    generateSubsequences(s, index + 1, current + s[index]);
}
```

**Intuition**: At every character, you have **two choices** — take it or leave it. This branching creates the recursion tree.

---

## Parameterized vs Functional Recursion

- **Functional**: Returns the answer (like factorial, fib)
- **Parameterized**: Passes the answer as parameter (useful in backtracking)

**Parameterized example** (sum of array):

```cpp
void sumArray(vector<int>& arr, int index, int& sum) {
    if (index == arr.size()) return;
    sum += arr[index];
    sumArray(arr, index + 1, sum);
}
```

---

## Tail Recursion

When the recursive call is the **last operation** in the function.

```cpp
// Tail recursive factorial
long long factTail(int n, long long result = 1) {
    if (n == 0) return result;
    return factTail(n - 1, n * result);
}
```

**Advantage**: Some compilers can optimize tail recursion into iteration (less stack usage).

---

## Recursive Tree Visualization

For problems with multiple recursive calls, always draw the tree:

```
                  fib(5)
             /             \
         fib(4)           fib(3)
        /      \         /     \
    fib(3)    fib(2)   fib(2)  fib(1)
```

This helps calculate time complexity and understand overlapping subproblems.

---

## Time & Space Complexity in Recursion

- **Time**: Count total number of calls in recursion tree
- **Space**: Maximum depth of recursion tree (call stack)

**Tip**: For single recursive call → O(n) space. For branching → exponential time/space.

---

## Recursion vs Iteration

| Aspect              | Recursion                     | Iteration                    |
|---------------------|-------------------------------|------------------------------|
| Code Readability    | Often cleaner                 | Can be verbose               |
| Memory Usage        | O(depth) stack                | O(1) usually                 |
| Performance         | Slower due to function calls  | Faster                       |
| Problem Suitability | Tree/Graph, Divide & Conquer  | Simple loops                 |

**My rule of thumb**: Use recursion when it makes the logic natural. Convert to iteration only if stack limit becomes an issue.

---

## How to Identify Recursion Problems

Look for these clues:
- "All possible combinations/permutations"
- "Generate all ways to..."
- "Explore every possibility"
- Problems with tree-like or hierarchical structure
- Divide a problem into smaller identical subproblems

Experienced devs see recursion when the problem can be broken down into "solve for n by solving for n-1".

---

## Common Beginner Mistakes

- Missing or incorrect base case → infinite recursion
- Not reducing problem size → stack overflow
- Forgetting to return result in functional recursion
- Modifying global variables incorrectly
- Not handling all edge cases (empty input, single element)
- Overcomplicating the recursive relation

**Pro tip**: Always test with smallest inputs first (n=0, n=1, empty list).

---

## Real-world Usage

- File system traversal (directories contain subdirectories)
- DOM tree in web browsers
- Expression tree evaluation in compilers
- Backtracking algorithms
- Tree and Graph traversals
- Divide & Conquer (Merge Sort, Quick Sort)

---

## Practice Problems

### Easy
- Factorial, Fibonacci
- Sum of digits, Power function
- Reverse array/string

### Intermediate
- Generate all subsets
- Letter combinations of phone number
- Tower of Hanoi

### Advanced
- Wildcard matching
- Regular expression matching
- N-Queens (leads to backtracking)

**Must-do LeetCode**:
- 206. Reverse Linked List
- 21. Merge Two Sorted Lists
- 78. Subsets
- 46. Permutations

---

## Quick Revision Cheat Sheet

```
Recursive Framework:
1. Base Case (smallest input)
2. Recursive Call (smaller subproblem)
3. Combine results (if needed)

Trust the recursive call!
Draw recursion tree for complex cases.
Always handle empty/null cases.
```

**Mental Models**:
- Russian dolls
- Call stack as plates
- Branching choices at every step

---

*Built while grinding recursion problems late at night. If this helped, star the repo!*

