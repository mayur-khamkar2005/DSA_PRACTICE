# Stack & Queue in C++ — Beginner to Advanced Interview Level

> My personal notes after grinding stacks, queues, and monotonic stack problems for months. These two data structures look simple but show up everywhere in interviews.

---

## Table of Contents

- [Introduction to Stack](#introduction-to-stack)
- [Introduction to Queue](#introduction-to-queue)
- [Stack Implementation](#stack-implementation)
- [Queue Implementation](#queue-implementation)
- [Core Concepts & Patterns](#core-concepts--patterns)
- [Monotonic Stack Deep Dive](#monotonic-stack-deep-dive)
- [Important Interview Topics](#important-interview-topics)
- [How to Identify Problems](#how-to-identify-problems)
- [Common Mistakes](#common-mistakes)
- [Real-world Usage](#real-world-usage)
- [Practice Problems](#practice-problems)
- [Quick Revision Cheat Sheet](#quick-revision-cheat-sheet)

---

## Introduction to Stack

Stack is a LIFO (Last In First Out) data structure. Think of a pile of plates in your kitchen — you add and remove from the top only.

**Real-world intuition:**
- Browser back button (history)
- Undo in text editors
- Function call stack in programming

The beauty of stack is its simplicity. You only care about the most recent element.

## Introduction to Queue

Queue is FIFO (First In First Out). Like people standing in a line at a ticket counter — first person to arrive gets served first.

**Real-world intuition:**
- Task scheduling in OS
- Breadth-first search (level order)
- Print job queues

## Stack Implementation

### Using Array

```cpp
class Stack {
private:
    vector<int> arr;
public:
    void push(int x) { arr.push_back(x); }
    void pop() { if (!empty()) arr.pop_back(); }
    int top() { return empty() ? -1 : arr.back(); }
    bool empty() { return arr.empty(); }
    int size() { return arr.size(); }
};
```

### Using Linked List (better for dynamic size)

```cpp
struct Node {
    int data;
    Node* next;
    Node(int val) : data(val), next(nullptr) {}
};

class StackLL {
private:
    Node* topNode;
public:
    StackLL() : topNode(nullptr) {}
    
    void push(int x) {
        Node* newNode = new Node(x);
        newNode->next = topNode;
        topNode = newNode;
    }
    
    void pop() {
        if (topNode) {
            Node* temp = topNode;
            topNode = topNode->next;
            delete temp;
        }
    }
    
    int top() { return topNode ? topNode->data : -1; }
    bool empty() { return topNode == nullptr; }
};
```

**STL way (most used in interviews):**

```cpp
stack<int> st;
st.push(10);
st.pop();
cout << st.top();
```

## Queue Implementation

### Array based Circular Queue (important for interviews)

```cpp
class CircularQueue {
private:
    vector<int> arr;
    int front, rear, capacity;
public:
    CircularQueue(int k) {
        capacity = k;
        arr.resize(k);
        front = rear = 0;
    }
    
    bool enqueue(int x) {
        if ((rear + 1) % capacity == front) return false; // full
        arr[rear] = x;
        rear = (rear + 1) % capacity;
        return true;
    }
    
    int dequeue() {
        if (front == rear) return -1; // empty
        int val = arr[front];
        front = (front + 1) % capacity;
        return val;
    }
};
```

**STL:**

```cpp
queue<int> q;
q.push(10);
q.pop();
cout << q.front();
```

**Deque (Double Ended Queue)** — most flexible:

```cpp
deque<int> dq;
dq.push_front(5);
dq.push_back(10);
dq.pop_front();
dq.pop_back();
```

## Monotonic Stack Deep Dive

This is where stacks become powerful in interviews.

**Core Idea**: Maintain elements in increasing or decreasing order.

**Intuition**: While building the stack, we remove elements that won't be useful for future queries.

### Next Greater Element (Right)

```cpp
vector<int> nextGreater(vector<int>& arr) {
    int n = arr.size();
    vector<int> nge(n, -1);
    stack<int> st;
    
    for (int i = n-1; i >= 0; i--) {
        while (!st.empty() && st.top() <= arr[i]) {
            st.pop();
        }
        if (!st.empty()) nge[i] = st.top();
        st.push(arr[i]);
    }
    return nge;
}
```

**Why it works**: We process from right to left. The stack always keeps candidates in decreasing order.

**Similar patterns**: Next Greater Left, Next Smaller, Previous Greater, etc.

## Important Interview Topics

### Stack Problems

**1. Valid Parentheses**

```cpp
bool isValid(string s) {
    stack<char> st;
    for (char c : s) {
        if (c == '(' || c == '{' || c == '[') {
            st.push(c);
        } else {
            if (st.empty()) return false;
            char top = st.top(); st.pop();
            if ((c == ')' && top != '(') || 
                (c == '}' && top != '{') || 
                (c == ']' && top != '[')) 
                return false;
        }
    }
    return st.empty();
}
```

**2. Min Stack** (Design problem - very common)

```cpp
class MinStack {
    stack<long long> st;
    long long minVal;
public:
    MinStack() { minVal = INT_MAX; }
    
    void push(int val) {
        if (val <= minVal) {
            st.push(2LL * val - minVal);
            minVal = val;
        } else {
            st.push(val);
        }
    }
    
    void pop() {
        long long top = st.top(); st.pop();
        if (top < minVal) {
            minVal = 2 * minVal - top;
        }
    }
    
    int top() { 
        long long t = st.top();
        return t < minVal ? minVal : t;
    }
    
    int getMin() { return minVal; }
};
```

**3. Largest Rectangle in Histogram** (Hard but important)

**4. Daily Temperatures** / **Stock Span**

### Queue Problems

- Sliding Window Maximum (using Deque)
- First negative in every window of size k
- Rotten Oranges (BFS)

## How to Identify Stack Problems

Look for these clues:
- Nearest greater/smaller element
- Matching brackets / nested structures
- Undo operations
- Expression evaluation (infix to postfix)
- Monotonic behavior

## How to Identify Queue Problems

- Level order / BFS style traversal
- Sliding window
- First come first serve processing
- Streaming data with "first X"

## Comparison Tables

| Feature              | Stack          | Queue           |
|----------------------|----------------|-----------------|
| Order                | LIFO           | FIFO            |
| Main Operations      | push/pop       | enqueue/dequeue |
| Real Life            | Plate pile     | Ticket line     |
| Common Use           | Recursion, undo| BFS, scheduling |

## Common Beginner Mistakes

- Not checking `empty()` before pop/top
- Wrong order in monotonic stack (pop condition)
- Forgetting to handle circular queue wrap-around
- Memory leaks in linked list implementation
- Using stack when queue (or vice versa) is needed

## Real-world Usage

- **Stack**: Browser history, function calls, text editor undo
- **Queue**: CPU process scheduling, print spooling, BFS in graphs
- **Monotonic Stack**: Stock price analysis, histogram problems
- **Deque**: Sliding window, LRU Cache implementation

## Practice Problems

**Easy:**
- Valid Parentheses
- Implement Stack using Queues
- Implement Queue using Stacks

**Intermediate:**
- Next Greater Element
- Daily Temperatures
- Min Stack
- Sliding Window Maximum

**Advanced:**
- Largest Rectangle in Histogram
- Trapping Rain Water (stack version)
- Asteroid Collision

**Must Do LeetCode:**
- 20. Valid Parentheses
- 155. Min Stack
- 739. Daily Temperatures
- 84. Largest Rectangle in Histogram
- 239. Sliding Window Maximum

---

## Quick Revision Cheat Sheet

```
Stack (LIFO):
- push, pop, top
- Monotonic: keep increasing/decreasing order

Queue (FIFO):
- enqueue, dequeue, front

Monotonic Stack Template:
for each element:
    while (stack not empty && condition)
        pop
    use top as answer
    push current
```

**Mental Models:**
- Stack → "most recent thing"
- Queue → "waiting line"
- Monotonic Stack → "candidates in order"

---

*Notes from late-night LeetCode sessions. If this helped, star the repo!*

