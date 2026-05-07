# Linked Lists in C++ — Beginner to Advanced Interview Level

> My personal notes after fighting with null pointers and losing head references for weeks. These are the exact patterns and mental models that finally clicked for me.

---

## Table of Contents

- [Introduction to Linked Lists](#introduction-to-linked-lists)
- [Array vs Linked List](#array-vs-linked-list)
- [Types of Linked Lists](#types-of-linked-lists)
- [Node Structure in C++](#node-structure-in-c)
- [Core Operations](#core-operations)
- [Fast & Slow Pointer Technique](#fast-and-slow-pointer-technique)
- [Dummy Node Technique](#dummy-node-technique)
- [Must-Know Problems](#must-know-problems)
- [Problem Recognition Tips](#problem-recognition)
- [Common Mistakes](#common-beginner-mistakes)
- [Real-world Usage](#real-world-usage)
- [Practice Problems](#practice-problems)
- [Quick Revision Cheat Sheet](#quick-revision-cheat-sheet)

---

## Introduction to Linked Lists

Linked List is basically a chain of nodes where each node points to the next one. Unlike arrays, elements don't sit next to each other in memory.

**Why they exist**: Arrays are fixed size and inserting/deleting in the middle is expensive (O(n) shifts). Linked lists solve this by using pointers.

**Mental Model**: Think of train coaches connected by couplings. You can add or remove coaches easily if you have access to the right spot, but finding a specific coach takes time unless you start from the beginning.

---

## Array vs Linked List

| Feature              | Array                      | Linked List                     |
|----------------------|----------------------------|---------------------------------|
| Memory Allocation    | Contiguous                 | Scattered                       |
| Access Time          | O(1)                       | O(n)                            |
| Insertion/Deletion   | O(n) in middle             | O(1) if you have the pointer    |
| Size                 | Fixed / Resizing costly    | Dynamic                         |
| Cache Friendly       | Excellent                  | Poor                            |
| Extra Memory         | None                       | Pointer per node                |

**Observation**: Use Linked List when you need frequent insertions/deletions and don't care much about random access.

---

## Types of Linked Lists

### 1. Singly Linked List
- Each node has `data` + `next` pointer
- Can only traverse forward
- Most common in interviews

### 2. Doubly Linked List
- Has `prev` + `next`
- Easier deletion and bidirectional traversal
- Used in LRU Cache

### 3. Circular Linked List
- Last node points back to first
- Useful for round-robin scheduling

---

## Node Structure in C++

```cpp
class ListNode {
public:
    int val;
    ListNode* next;
    ListNode(int x) : val(x), next(nullptr) {}
};

// For Doubly Linked List
class DoublyNode {
public:
    int val;
    DoublyNode* prev;
    DoublyNode* next;
    DoublyNode(int x) : val(x), prev(nullptr), next(nullptr) {}
};
```

**Pro Tip**: Always use constructor to avoid forgetting to initialize `next` to `nullptr`.

---

## Core Operations

### Insertion at Head

```cpp
ListNode* insertAtHead(ListNode* head, int val) {
    ListNode* newNode = new ListNode(val);
    newNode->next = head;
    return newNode;  // new head
}
```

### Insertion at End (with tail pointer is better)

### Deletion, Search, etc. — similar pointer dance.

---

## Fast and Slow Pointer Technique

This is a **game changer**.

**Intuition**: Two pointers moving at different speeds. When fast reaches end, slow is at middle. If there's a cycle, they will meet.

**Classic Use Cases**:
- Find middle
- Detect cycle (Floyd’s Tortoise and Hare)
- Remove nth node from end

```cpp
// Middle of Linked List
ListNode* middleNode(ListNode* head) {
    ListNode* slow = head;
    ListNode* fast = head;
    
    while (fast && fast->next) {
        slow = slow->next;
        fast = fast->next->next;
    }
    return slow;
}
```

---

## Dummy Node Technique

**Why it helps**: Avoids special cases for head modification.

```cpp
// Merge Two Sorted Lists
ListNode* mergeTwoLists(ListNode* l1, ListNode* l2) {
    ListNode* dummy = new ListNode(0);
    ListNode* tail = dummy;
    
    while (l1 && l2) {
        if (l1->val < l2->val) {
            tail->next = l1;
            l1 = l1->next;
        } else {
            tail->next = l2;
            l2 = l2->next;
        }
        tail = tail->next;
    }
    
    tail->next = l1 ? l1 : l2;
    return dummy->next;  // Important: return dummy->next
}
```

---

## Must-Know Problems

### 1. Reverse Linked List

**Iterative (Preferred in interviews)**

```cpp
ListNode* reverseList(ListNode* head) {
    ListNode* prev = nullptr;
    ListNode* curr = head;
    
    while (curr) {
        ListNode* nextTemp = curr->next;
        curr->next = prev;
        prev = curr;
        curr = nextTemp;
    }
    return prev;
}
```

**Recursive version** — good for understanding but watch stack depth.

### 2. Detect Cycle + Find Start of Cycle

### 3. Remove Nth Node From End

Use fast-slow + dummy.

### 4. Merge K Sorted Lists → Use Priority Queue (connects to Heaps)

### 5. Copy List with Random Pointer (Tricky with hashmap or interweaving)

---

## Problem Recognition Tips

Look for these keywords:
- "reverse", "middle", "cycle", "palindrome", "intersection"
- "nth from end", "k-group"
- In-place modification with O(1) extra space
- Sequential access pattern

**Experienced developers** immediately think "pointer manipulation + dummy/fast-slow" when they see linked list.

---

## Common Beginner Mistakes

- Losing reference to head
- Not storing `next` before changing pointers
- Null pointer dereference
- Forgetting to handle empty list or single node
- Memory leaks (in real code)
- Modifying while traversing without temp pointer

---

## Real-world Usage

- Browser back/forward (doubly linked)
- Music playlist (circular)
- Undo/Redo functionality
- LRU Cache implementation
- OS process scheduling queues

---

## Practice Problems

**Easy**:
- Reverse Linked List
- Merge Two Sorted Lists
- Palindrome Linked List

**Intermediate**:
- Remove Nth Node From End
- Linked List Cycle II
- Odd Even Linked List

**Advanced**:
- Merge K Sorted Lists
- Reverse Nodes in k-Group
- Copy List with Random Pointer

**Must Do LeetCode**:
- 206, 19, 141, 142, 21, 23, 25, 138, 2, 445

---

## Quick Revision Cheat Sheet

```
Key Techniques:
1. Dummy Node → handle head changes easily
2. Fast & Slow → middle, cycle, nth from end
3. Three pointers → reverse, swap nodes
4. HashMap → random pointer, intersection

Always ask:
- Can I modify in-place?
- Do I need dummy?
- Is there a cycle?
- Need fast-slow?
```

**Pointer Movement Golden Rule**: Always store next before changing links.

---

*Built after many painful debugging sessions. If this helped, star it ⭐*

