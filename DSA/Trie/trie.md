# Trie in C++ — Beginner to Advanced Interview Level

> Personal notes + clean implementations from solving 100+ Trie problems on LeetCode. From "what the hell is a Trie" to building autocomplete, word search, and bitmask + Trie hybrids in interviews. If you're tired of TLE on string problems — this is your weapon.

![C++](https://img.shields.io/badge/C%2B%2B-17%2B-blue) ![DSA](https://img.shields.io/badge/DSA-Trie-green) ![Interview](https://img.shields.io/badge/Interview-Ready-brightgreen)

---

## Table of Contents

- [What is a Trie?](#what-is-a-trie)
- [Why Trie Matters](#why-trie-matters)
- [Prefix Tree Intuition](#prefix-tree-intuition)
- [Trie Node Structure](#trie-node-structure)
- [Core Operations](#core-operations)
  - [Insert](#insert)
  - [Search](#search)
  - [Prefix Search](#prefix-search)
  - [Delete](#delete)
- [Real-World Use Cases](#real-world-use-cases)
- [Trie vs HashMap](#trie-vs-hashmap)
- [Memory Tradeoffs](#memory-tradeoffs)
- [Advanced Patterns & Interview Problems](#advanced-patterns--interview-problems)
- [Common Beginner Mistakes](#common-beginner-mistakes)
- [Clean C++ Implementation](#clean-c-implementation)
- [Practice Problems (Curated)](#practice-problems-curated)
- [Cheat Sheet](#cheat-sheet)

---

## What is a Trie?

A **Trie** (pronounced "try") is a tree-like data structure used to store a dynamic set of strings efficiently. It is also called a **Prefix Tree**.

Each node represents a single character, and the path from root to a node represents a prefix of the word.

---

## Why Trie Matters

- Extremely fast prefix-based searches (O(L) where L = length of word)
- Perfect for autocomplete, spell checkers, dictionaries
- Solves many string problems faster than HashMap or brute force
- Used in IP routing, XOR maximization (with binary Trie), etc.

Once you master Tries, problems like Word Search II, Replace Words, Longest Word in Dictionary become straightforward.

---

## Prefix Tree Intuition

Imagine a phone keypad or a family tree for words.

**Visual Example** (Words: "cat", "car", "dog"):

```
      Root
     /    \
   c        d
  /         \
 a           o
/ \          \
t   r          g
|   |
*   *
```

- Path `c→a→t` = "cat"
- Path `c→a→r` = "car"
- Shared prefix "ca" is stored only once → huge space saving for common prefixes

**Key Insight**: Characters are stored on **edges**, nodes represent prefixes.

---

## Trie Node Structure

```cpp
struct TrieNode {
    TrieNode* children[26];  // Assuming lowercase English letters
    bool isEndOfWord = false;
    
    TrieNode() {
        for (int i = 0; i < 26; i++) {
            children[i] = nullptr;
        }
    }
};
```

You can also use `unordered_map<char, TrieNode*>` for dynamic alphabets (slower but more flexible).

---

## Core Operations

### Insert

```cpp
void insert(TrieNode* root, const string& word) {
    TrieNode* node = root;
    for (char ch : word) {
        int idx = ch - 'a';
        if (node->children[idx] == nullptr) {
            node->children[idx] = new TrieNode();
        }
        node = node->children[idx];
    }
    node->isEndOfWord = true;
}
```

**Dry Run - Insert("cat")**:
1. Start at root
2. 'c' → create child[2], move
3. 'a' → create child[0], move
4. 't' → create child[19], mark `isEndOfWord = true`

### Search (Exact Word)

```cpp
bool search(TrieNode* root, const string& word) {
    TrieNode* node = root;
    for (char ch : word) {
        int idx = ch - 'a';
        if (node->children[idx] == nullptr) return false;
        node = node->children[idx];
    }
    return node->isEndOfWord;
}
```

### Prefix Search (Starts With)

```cpp
bool startsWith(TrieNode* root, const string& prefix) {
    TrieNode* node = root;
    for (char ch : prefix) {
        int idx = ch - 'a';
        if (node->children[idx] == nullptr) return false;
        node = node->children[idx];
    }
    return true;  // We reached the end of prefix → exists
}
```

**Intuition**: For prefix search, we don't care about `isEndOfWord`. As long as the path exists, the prefix is present.

---

## Delete Operation Basics

```cpp
bool deleteWord(TrieNode* root, const string& word, int depth = 0) {
    if (!root) return false;
    
    if (depth == word.length()) {
        if (!root->isEndOfWord) return false;
        root->isEndOfWord = false;
        return true;  // Can add logic to prune nodes if no children
    }
    
    int idx = word[depth] - 'a';
    if (root->children[idx] && deleteWord(root->children[idx], word, depth + 1)) {
        // Optional: prune if node has no children and not end of word
        if (!root->children[idx]->isEndOfWord && allChildrenNull(root->children[idx])) {
            delete root->children[idx];
            root->children[idx] = nullptr;
        }
        return true;
    }
    return false;
}
```

---

## Real-World Use Cases

- **Autocomplete / Search Suggestions**
- **Spell Checker**
- **Word Break / Dictionary problems**
- **Longest Common Prefix**
- **IP Routing Tables**
- **Binary Trie for Maximum XOR**

---

## Trie vs HashMap

| Feature              | Trie                          | HashMap (unordered_set)     |
|----------------------|-------------------------------|-----------------------------|
| Prefix Search        | O(L) - Excellent              | O(N) - Poor                 |
| Insert/Search        | O(L)                          | O(L) average                |
| Memory               | Higher (nodes)                | Lower                       |
| Space Efficiency     | Great for common prefixes     | Better for unique strings   |
| Wildcard / Pattern   | Easy to extend                | Hard                        |

**Use Trie when**: You need frequent prefix queries.

---

## Memory Tradeoffs

- Each node: 26 pointers + bool ≈ 200+ bytes (on 64-bit)
- For 10^5 words of avg length 10 → significant memory
- Optimization: Use map<char, TrieNode*> or compress nodes

---

## Advanced Patterns & Interview Problems

1. **Word Search II** (LeetCode 212) — Trie + DFS on board
2. **Replace Words** (LeetCode 648)
3. **Longest Word in Dictionary** (LeetCode 720)
4. **Implement Trie** (LeetCode 208)
5. **Maximum XOR Pair** (Binary Trie)
6. **Design Search Autocomplete System**

**Common Pattern**: Insert all dictionary words into Trie → Query board / string efficiently.

---

## Common Beginner Mistakes

- Forgetting to handle uppercase / special characters
- Memory leaks (not deleting nodes)
- Using `children[26]` without checking null
- Confusing `isEndOfWord` with prefix existence
- Not using `const string&` in functions (performance)
- Off-by-one in recursion depth

---

## Clean C++ Implementation

Here's a complete, production-ready Trie class:

```cpp
#include <bits/stdc++.h>
using namespace std;

class Trie {
private:
    struct TrieNode {
        TrieNode* children[26];
        bool isEndOfWord;
        TrieNode() : isEndOfWord(false) {
            memset(children, 0, sizeof(children));
        }
    };
    
    TrieNode* root;
    
public:
    Trie() {
        root = new TrieNode();
    }
    
    void insert(const string& word) {
        TrieNode* node = root;
        for (char ch : word) {
            int idx = ch - 'a';
            if (node->children[idx] == nullptr) {
                node->children[idx] = new TrieNode();
            }
            node = node->children[idx];
        }
        node->isEndOfWord = true;
    }
    
    bool search(const string& word) {
        TrieNode* node = root;
        for (char ch : word) {
            int idx = ch - 'a';
            if (node->children[idx] == nullptr) return false;
            node = node->children[idx];
        }
        return node->isEndOfWord;
    }
    
    bool startsWith(const string& prefix) {
        TrieNode* node = root;
        for (char ch : prefix) {
            int idx = ch - 'a';
            if (node->children[idx] == nullptr) return false;
            node = node->children[idx];
        }
        return true;
    }
    
    // Bonus: Longest Common Prefix using Trie
    string longestCommonPrefix(vector<string>& strs) {
        if (strs.empty()) return "";
        for (string& s : strs) insert(s);
        
        TrieNode* node = root;
        string lcp = "";
        while (true) {
            int count = 0;
            int nextIdx = -1;
            for (int i = 0; i < 26; i++) {
                if (node->children[i]) {
                    count++;
                    nextIdx = i;
                }
            }
            if (count != 1 || node->isEndOfWord) break;
            lcp += (char)('a' + nextIdx);
            node = node->children[nextIdx];
        }
        return lcp;
    }
};

int main() {
    Trie trie;
    trie.insert("apple");
    trie.insert("app");
    cout << trie.search("apple") << endl;     // 1
    cout << trie.search("app") << endl;       // 1
    cout << trie.startsWith("app") << endl;   // 1
    return 0;
}
```

---

## Practice Problems (Curated)

**Easy**:
- Implement Trie (Prefix Tree) - 208
- Longest Common Prefix - 14

**Medium**:
- Word Search II - 212
- Replace Words - 648
- Design Add and Search Words Data Structure - 211

**Hard**:
- Maximum XOR of Two Numbers in an Array - 421 (Binary Trie)
- Word Squares
- Palindrome Pairs

---

## Cheat Sheet

```cpp
// Core Operations
insert(word)          → O(L)
search(word)          → O(L)
startsWith(prefix)    → O(L)

// Node
children[26]
isEndOfWord

// Tips
- Always handle only lowercase unless specified
- Use Trie for prefix-heavy queries
- Combine with DFS for board/search problems
```

**One-liners to remember**:
- "Shared prefixes = shared nodes"
- "Path from root = prefix"
- "isEndOfWord marks complete word"

---

## Contributing

Suggestions for Binary Trie, Compressed Trie, or Aho-Corasick welcome! PRs open.

---

*Built while preparing for placement season and grinding LeetCode Hard string problems. If this helped, give it a ⭐*

**Happy Trie-ing!** 🌳
