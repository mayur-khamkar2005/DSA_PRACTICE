# Trees in C++ — Beginner to Advanced Interview Level

> My personal notes after grinding trees for weeks. Went from getting confused with traversals to comfortably solving LCA, diameter, and BST problems in interviews. These are the observations and templates that actually helped me.

---

## Table of Contents

- [Introduction to Trees](#introduction-to-trees)
- [Binary Tree Basics](#binary-tree-basics)
- [Types of Trees](#types-of-trees)
- [Tree Traversals](#tree-traversals)
- [Important Interview Topics](#important-interview-topics)
- [Binary Search Tree (BST)](#binary-search-tree-bst)
- [How to Identify Tree Problems](#how-to-identify-tree-problems)
- [Common Mistakes](#common-mistakes)
- [Real-world Usage](#real-world-usage)
- [Practice Problems](#practice-problems)
- [Quick Revision Cheat Sheet](#quick-revision-cheat-sheet)

---

## Introduction to Trees

Trees are hierarchical data structures. Think of a family tree, file system on your computer, or organizational chart in a company. There's a root at the top, and things branch out downwards.

**Key terms:**
- **Root**: The topmost node
- **Leaf**: Nodes with no children
- **Height**: Longest path from root to leaf
- **Depth**: Distance from root to a node
- **Level**: Nodes at same distance from root

Trees are useful because they give us **log n** operations in balanced cases and naturally represent hierarchical relationships.

## Binary Tree Basics

Most interview trees are **Binary Trees** — each node has at most two children: left and right.

```cpp
struct TreeNode {
    int val;
    TreeNode* left;
    TreeNode* right;
    TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
};
```

**Mental model**: Every node is the root of its own subtree. This is the key to thinking recursively.

## Types of Trees

| Type                  | Description                              | Notes |
|-----------------------|------------------------------------------|-------|
| Binary Tree           | ≤ 2 children per node                    | Most common in interviews |
| Full Binary Tree      | 0 or 2 children                          | No single-child nodes |
| Complete Binary Tree  | All levels filled except possibly last   | Used in Heaps |
| Perfect Binary Tree   | All levels completely filled             | Rare in problems |
| Balanced              | Height difference ≤ 1                    | Good performance |
| Degenerate            | Like a linked list                       | Worst case |

## Tree Traversals

### DFS Traversals (Recursive is natural here)

**1. Inorder (Left → Root → Right)**  
Used for BST to get sorted order.

```cpp
void inorder(TreeNode* root) {
    if (!root) return;
    inorder(root->left);
    cout << root->val << " ";
    inorder(root->right);
}
```

**2. Preorder (Root → Left → Right)**  
Used for copying tree or serialization.

**3. Postorder (Left → Right → Root)**  
Used for deletion, diameter, bottom-up calculations.

### BFS - Level Order Traversal

```cpp
vector<vector<int>> levelOrder(TreeNode* root) {
    vector<vector<int>> result;
    if (!root) return result;
    
    queue<TreeNode*> q;
    q.push(root);
    
    while (!q.empty()) {
        int size = q.size();
        vector<int> level;
        for (int i = 0; i < size; i++) {
            TreeNode* node = q.front(); q.pop();
            level.push_back(node->val);
            if (node->left) q.push(node->left);
            if (node->right) q.push(node->right);
        }
        result.push_back(level);
    }
    return result;
}
```

## Important Interview Topics

### Maximum Depth / Height of Tree

**Intuition**: Height of tree = 1 + max(height of left, height of right)

```cpp
int maxDepth(TreeNode* root) {
    if (!root) return 0;
    return 1 + max(maxDepth(root->left), maxDepth(root->right));
}
```

### Diameter of Binary Tree

Longest path between any two nodes (may or may not pass through root).

**Key observation**: Diameter = max( left height + right height , max(diameter in left, diameter in right) )

### Symmetric Tree & Same Tree

Simple recursive comparison.

### Lowest Common Ancestor (LCA)

One of the most important problems.

**Intuition**: If both nodes are in different subtrees → current root is LCA.

### Binary Tree Maximum Path Sum

Hard problem. Need to consider paths that go through current node.

## Binary Search Tree (BST)

**Property**: Left subtree < root < right subtree (for all nodes)

**Inorder traversal gives sorted order** — this is gold in interviews.

**Search, Insert, Delete** — all O(h) where h is height.

**Validate BST**: Inorder should be strictly increasing (handle duplicates carefully).

**Kth Smallest Element**: Inorder traversal with counter.

## Recursive Thinking in Trees

Trees scream recursion. 
- Every node is a smaller version of the same problem.
- Solve left and right subtrees.
- Combine results at current node.
- Base case: null node or leaf.

**Trust the recursion** — assume smaller subtrees are solved correctly.

## How to Identify Tree Problems

Look for words like:
- Binary tree, root, node, subtree
- Path from root to leaf
- Level order, top/bottom view
- Ancestor, descendant
- Balanced, diameter, height
- Serialize/deserialize

If problem involves hierarchy or parent-child → think tree.

## Comparison Tables

**DFS vs BFS**

| Aspect       | DFS (Recursion)       | BFS (Queue)             |
|--------------|-----------------------|-------------------------|
| Traversal    | Depth first           | Level by level          |
| Space        | O(h)                  | O(w) - width            |
| Use case     | Path problems         | Shortest path (unweighted) |

**Binary Tree vs BST**

- BST has ordering property → enables efficient search/insert/delete

## Common Beginner Mistakes

- Null pointer dereference
- Forgetting to handle null children
- Wrong base case (`if(!root) return;` vs `if(!root) return 0;`)
- Mixing up traversal orders
- Not using reference for result collection
- Assuming tree is balanced

## Real-world Usage

- File systems (directories)
- DOM in browsers
- Decision trees in ML
- Database indexing (B+ trees)
- Huffman coding
- Routing algorithms

## Practice Problems

**Easy:**
- Maximum Depth
- Same Tree
- Symmetric Tree

**Intermediate:**
- Diameter, LCA, Zigzag, Views

**Advanced:**
- Serialize/Deserialize, Max Path Sum, Flatten to Linked List

**Must Do LeetCode:**
- 94, 102, 104, 226, 236 (LCA), 124 (Max Path Sum), 297 (Serialize)

---

## Quick Revision Cheat Sheet

```
Traversals:
- Pre: Root Left Right
- In:  Left Root Right (BST sorted)
- Post: Left Right Root (delete)

Recursive Template:
if (!root) return ...;
... left
... right
combine
```

**Mental Model**: Every subtree is a smaller tree. Solve bottom-up.

---

*Built while preparing for interviews. Star if it helped you!*

