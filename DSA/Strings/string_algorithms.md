# String Algorithms in C++ — Beginner to Advanced Interview Level

> Personal notes + implementations I built while preparing for coding interviews.  
> This README focuses on understanding patterns, not memorizing code blindly.

---

# Table of Contents

- Introduction to Strings
- Character Arrays vs Strings
- Frequency Maps
- Palindrome Problems
- Anagram Problems
- Sliding Window on Strings
- KMP Algorithm
- Rabin Karp
- Z Algorithm
- String Hashing
- Common Interview Patterns
- Common Mistakes
- Practice Problems
- Cheat Sheet

---

# Introduction to Strings

Strings are everywhere in interviews.

- Search bars
- Chat systems
- Compilers
- DNA matching
- Pattern searching
- Autocomplete systems

Most beginners think strings are “easy arrays of characters”.

They are not.

String problems usually test:
- pattern recognition
- optimization
- sliding window
- hashing
- two pointers
- preprocessing

---

# Character Arrays vs Strings

| Character Array | std::string |
|---|---|
| Ends with `\0` | Dynamic object |
| Manual handling | Easy methods |
| Error-prone | Safer |
| Fixed size usually | Resizable |

```cpp
char arr[] = "hello";

string s = "hello";
```

In interviews:
use `string` most of the time.

---

# String Traversal

```cpp
string s = "mayur";

for(char ch : s) {
    cout << ch << " ";
}
```

Time Complexity:
```cpp
O(n)
```

---

# Frequency Maps

Very common interview pattern.

Used in:
- anagrams
- duplicates
- sliding window
- counting characters

---

## Using Array

```cpp
vector<int> freq(26, 0);

for(char ch : s) {
    freq[ch - 'a']++;
}
```

Fastest for lowercase alphabets.

---

## Using HashMap

```cpp
unordered_map<char, int> mp;

for(char ch : s) {
    mp[ch]++;
}
```

Useful when:
- uppercase exists
- symbols exist
- unicode-like problems

---

# Palindrome Problems

## Core Intuition

A palindrome reads same from both sides.

Use:
```cpp
Two Pointers
```

---

## Example

```cpp
madam
```

- m == m
- a == a

Palindrome.

---

## Code

```cpp
bool isPalindrome(string s) {

    int left = 0;
    int right = s.size() - 1;

    while(left < right) {

        if(s[left] != s[right]) {
            return false;
        }

        left++;
        right--;
    }

    return true;
}
```

---

# Anagram Problems

## Intuition

Same characters.
Different order.

Example:

```cpp
listen
silent
```

---

## Sorting Approach

```cpp
sort(s1.begin(), s1.end());
sort(s2.begin(), s2.end());

return s1 == s2;
```

Time:
```cpp
O(n log n)
```

---

## Frequency Approach

```cpp
vector<int> freq(26, 0);

for(char ch : s1) freq[ch - 'a']++;
for(char ch : s2) freq[ch - 'a']--;

for(int count : freq) {
    if(count != 0) return false;
}
```

Time:
```cpp
O(n)
```

---

# Sliding Window on Strings

One of the most important interview patterns.

Used in:
- longest substring
- minimum window
- unique characters

---

# Longest Substring Without Repeating Characters

## Intuition

Expand window.
If duplicate appears:
shrink window.

---

## Code

```cpp
int lengthOfLongestSubstring(string s) {

    unordered_map<char, int> mp;

    int left = 0;
    int ans = 0;

    for(int right = 0; right < s.size(); right++) {

        mp[s[right]]++;

        while(mp[s[right]] > 1) {
            mp[s[left]]--;
            left++;
        }

        ans = max(ans, right - left + 1);
    }

    return ans;
}
```

---

# KMP Algorithm

## Why KMP Exists

Naive matching repeats unnecessary work.

KMP avoids rechecking characters.

---

# LPS Array Intuition

LPS:
```text
Longest Prefix which is also Suffix
```

Pattern:
```text
ababaca
```

LPS:
```text
0 0 1 2 3 0 1
```

---

# KMP Matching Logic

When mismatch happens:
- do NOT restart from beginning
- use LPS info

This is the real magic.

---

## KMP Complexity

| Operation | Complexity |
|---|---|
| Build LPS | O(m) |
| Pattern Match | O(n) |

---

## KMP Code

```cpp
vector<int> buildLPS(string pattern) {

    int n = pattern.size();

    vector<int> lps(n, 0);

    int len = 0;
    int i = 1;

    while(i < n) {

        if(pattern[i] == pattern[len]) {

            len++;
            lps[i] = len;
            i++;
        }
        else {

            if(len != 0) {
                len = lps[len - 1];
            }
            else {
                lps[i] = 0;
                i++;
            }
        }
    }

    return lps;
}
```

---

# Rabin Karp

## Core Idea

Use hashing for pattern matching.

Instead of comparing full strings repeatedly:
compare hashes first.

---

# Rolling Hash Intuition

Window moves:
- remove left contribution
- add right contribution

Very similar to sliding window.

---

## Complexity

Average:
```cpp
O(n + m)
```

Worst:
```cpp
O(n * m)
```

(due to collisions)

---

# Z Algorithm Basics

## Core Idea

Create Z-array.

Each index stores:
```text
Longest substring starting from that index
which matches prefix.
```

Useful for:
- pattern matching
- repeated prefix problems

---

# String Hashing

## Why Hashing Helps

Instead of comparing strings repeatedly:
compare numbers (hashes).

Used in:
- plagiarism detection
- substring queries
- Rabin Karp
- competitive programming

---

# Common Interview Patterns

| Pattern | Common Topics |
|---|---|
| Two Pointers | Palindrome |
| Sliding Window | Substrings |
| Frequency Map | Anagrams |
| Hashing | Pattern Matching |
| Prefix/Suffix | KMP |
| Rolling Hash | Rabin Karp |

---

# Common Beginner Mistakes

## 1. Off-by-One Errors

Very common in substrings.

---

## 2. Wrong Sliding Window Shrinking

People shrink too early or too late.

---

## 3. Case Sensitivity Issues

```cpp
A != a
```

---

## 4. KMP LPS Confusion

Most beginners memorize.
Very few understand WHY fallback works.

---

# Practice Problems

## Easy
- Valid Anagram
- Valid Palindrome
- Reverse String

## Medium
- Longest Substring Without Repeating Characters
- Group Anagrams
- Minimum Window Substring

## Hard
- Edit Distance
- Distinct Subsequences
- Regular Expression Matching

---

# Interview Insights

Most string interview problems are actually:
- sliding window
- hashmap
- two pointers
- DP
- pattern matching

The real skill is:
recognizing the pattern quickly.

---

# Final Cheat Sheet

| Topic | Best Pattern |
|---|---|
| Palindrome | Two Pointers |
| Unique Characters | Sliding Window |
| Anagram | Frequency Map |
| Pattern Search | KMP |
| Fast Matching | Rabin Karp |
| Prefix Matching | Trie / KMP |

---

# Final Notes

If you're struggling with strings:
don't start from KMP immediately.

Master first:
1. Traversal
2. Frequency Maps
3. Sliding Window
4. Two Pointers

Then advanced algorithms become much easier.
