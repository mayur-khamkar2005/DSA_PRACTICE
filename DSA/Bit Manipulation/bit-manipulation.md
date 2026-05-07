# Bit Manipulation in C++ — Beginner to Advanced Interview Level

> Personal notes + battle-tested implementations from 300+ LeetCode problems. From "what even is a bit" to solving hard bitmask DP problems in interviews. If you're preparing for FAANG/MAANG or just want to stop fearing bitwise operations — this is for you.

![C++](https://img.shields.io/badge/C%2B%2B-17%2B-blue) ![DSA](https://img.shields.io/badge/DSA-Bit%20Manipulation-orange) ![Interview](https://img.shields.io/badge/Interview-Ready-brightgreen)

---

## Table of Contents

- [Why Bit Manipulation Matters](#why-bit-manipulation-matters)
- [Binary Number Basics](#binary-number-basics)
- [Core Bitwise Operators](#core-bitwise-operators)
- [Shifts: Left & Right](#shifts-left--right)
- [Common Bit Tricks](#common-bit-tricks)
- [Power of Two Checks](#power-of-two-checks)
- [Counting Set Bits](#counting-set-bits)
- [XOR — The Magic Operator](#xor--the-magic-operator)
- [Single Number Problems](#single-number-problems)
- [Missing Number Problems](#missing-number-problems)
- [Bitmasking & Subsets](#bitmasking--subsets)
- [Advanced Techniques & Interview Tricks](#advanced-techniques--interview-tricks)
- [Common Beginner Mistakes](#common-beginner-mistakes)
- [Big Comparison & When to Use](#big-comparison--when-to-use)
- [Practice Problems (Curated)](#practice-problems-curated)
- [Cheat Sheet](#cheat-sheet)

---

## Why Bit Manipulation Matters

Bit manipulation is one of the **highest ROI** topics in competitive programming and interviews.

- Solves problems in O(1) or O(log n) that would otherwise be O(n)
- Used heavily in optimization, graphics, cryptography, embedded systems
- Many "clever" interview questions boil down to bits
- Once you internalize the intuition, you start seeing bit solutions everywhere

**Real talk**: At first it feels like black magic. After 50 problems, it becomes one of your strongest weapons.

---

## Binary Number Basics

Every integer is stored as bits in memory.

- **1 byte = 8 bits**
- Positive numbers use two's complement representation (sign bit for negatives)

**Binary Visualization**:

```
Decimal: 13
Binary : 00001101   (8-bit for clarity)

Positions (from right, 0-based):
Bit 0 (LSB): 1 → 2^0 = 1
Bit 1:       0 → 2^1 = 0
Bit 2:       1 → 2^2 = 4
Bit 3:       1 → 2^3 = 8
Bit 4-7:     0
Total = 8 + 4 + 1 = 13
```

**Key Powers of 2**:
- `1 << 0 = 1`
- `1 << 1 = 2`
- `1 << 2 = 4`
- `1 << 3 = 8`
- ...
- `1 << n = 2^n`

---

## Core Bitwise Operators

| Operator | Name     | Effect                          | Example (a=5: 0101, b=3: 0011) |
|----------|----------|---------------------------------|---------------------------------|
| `&`      | AND      | 1 only if both 1                | `5 & 3 = 1` (0001)             |
| `|`      | OR       | 1 if at least one 1             | `5 | 3 = 7` (0111)             |
| `^`      | XOR      | 1 if bits differ                | `5 ^ 3 = 6` (0110)             |
| `~`      | NOT      | Flips all bits                  | `~5 = ...` (careful with sign) |
| `<<`     | Left Shift  | Multiply by 2, shift left    | `5 << 1 = 10`                  |
| `>>`     | Right Shift | Divide by 2 (floor), shift right | `5 >> 1 = 2`                |

### Visual Dry Run - AND, OR, XOR

```
a = 5:  0101
b = 3:  0011
----------------
AND:    0001  → 1
OR:     0111  → 7
XOR:    0110  → 6
```

---

## Shifts: Left & Right

- `x << k` = x * (2^k)
- `x >> k` = x / (2^k) (integer division, arithmetic shift for signed)

**Important**:
- Use `1LL << k` when k can be up to 60+ to avoid overflow
- Right shift on negative numbers (implementation-defined in older C++, but arithmetic in practice)

```cpp
// Safe left shift
long long safeShift = 1LL << 40;
```

---

## Common Bit Tricks

### 1. Check if number is odd/even
```cpp
bool isOdd(int x) { return x & 1; }        // LSB is 1
bool isEven(int x) { return !(x & 1); }
```

### 2. Set a bit at position k (0-based)
```cpp
x |= (1 << k);
```

### 3. Clear (unset) a bit at position k
```cpp
x &= ~(1 << k);
```

### 4. Toggle a bit at position k
```cpp
x ^= (1 << k);
```

### 5. Check if k-th bit is set
```cpp
bool isSet = x & (1 << k);
```

**Dry Run - Set Bit**:
```
x = 5:   0101
k = 1    (want to set bit 1)
1<<1 =   0010
x |=     0111 → 7
```

---

## Power of Two Checks

```cpp
bool isPowerOfTwo(int n) {
    return n > 0 && (n & (n - 1)) == 0;
}
```

**Why it works**:
```
n    = 8: 1000
n-1  = 7: 0111
n & (n-1) = 0000
```

Any power of two has exactly one set bit. Subtracting 1 flips all bits after it.

---

## Counting Set Bits

### Brian Kernighan’s Algorithm (Best)
```cpp
int countSetBits(int n) {
    int count = 0;
    while (n) {
        n = n & (n - 1);  // removes lowest set bit
        count++;
    }
    return count;
}
```

**Dry Run**:
```
n = 13: 1101
13 & 12 = 1100 (12)  → count=1
12 & 11 = 1000 (8)   → count=2
8  & 7  = 0000       → count=3
```

**C++20**: `__builtin_popcount(n)` or `__builtin_popcountll(n)`

---

## XOR — The Magic Operator

**Key Properties**:
1. `x ^ x = 0`
2. `x ^ 0 = x`
3. XOR is associative and commutative
4. `a ^ b ^ a = b`

These properties solve many "single number" type problems.

---

## Single Number Problems

### Single Number I (LeetCode 136)
Every element appears twice except one. Find it.

**Solution**: XOR all numbers
```cpp
int singleNumber(vector<int>& nums) {
    int res = 0;
    for (int num : nums) res ^= num;
    return res;
}
```

### Single Number II (LeetCode 137)
Every element appears 3 times except one.

**Bitwise solution** (track bits modulo 3):

```cpp
int singleNumber(vector<int>& nums) {
    int ones = 0, twos = 0;
    for (int num : nums) {
        ones = (ones ^ num) & ~twos;
        twos = (twos ^ num) & ~ones;
    }
    return ones;
}
```

---

## Missing Number Problems

### Missing Number (LeetCode 268)
```cpp
int missingNumber(vector<int>& nums) {
    int n = nums.size();
    int xor_all = n;
    for (int i = 0; i < n; i++) {
        xor_all ^= i ^ nums[i];
    }
    return xor_all;
}
```

---

## Bitmasking & Subsets

**Generate all subsets** (Power Set):

```cpp
vector<vector<int>> subsets(vector<int>& nums) {
    int n = nums.size();
    vector<vector<int>> result;
    int total = 1 << n;                    // 2^n subsets
    
    for (int mask = 0; mask < total; mask++) {
        vector<int> subset;
        for (int i = 0; i < n; i++) {
            if (mask & (1 << i)) {
                subset.push_back(nums[i]);
            }
        }
        result.push_back(subset);
    }
    return result;
}
```

**Dry Run (nums = [1,2])**:
- mask 00 → {}
- mask 01 → {1}
- mask 10 → {2}
- mask 11 → {1,2}

**Bitmask DP** is extremely powerful for problems like Traveling Salesman, Knapsack with constraints, etc.

---

## Advanced Techniques & Interview Tricks

1. **Get lowest set bit**: `x & -x` (two's complement trick)
2. **Clear lowest set bit**: `x &= (x - 1)`
3. **Swap two variables without temp**: `a ^= b; b ^= a; a ^= b;`
4. **Check opposite signs**: `(a ^ b) < 0`
5. **Modulo with power of 2**: `x & (m-1)` where m is power of 2
6. **Extract last digit in binary**: `x & 1`

**Gray Code**:
```cpp
int grayCode(int n) {
    return n ^ (n >> 1);
}
```

---

## Common Beginner Mistakes

- Using `1 << 32` or higher without `long long` (undefined behavior)
- Forgetting that `~x` flips **all** bits (including sign bits) → use `x ^ ((1LL<<k)-1)` for lower k bits
- Assuming right shift is logical (>>>) for signed ints
- Off-by-one in bit positions
- Not handling negative numbers when required
- Writing non-portable code relying on specific compiler behavior

---

## Big Comparison & When to Use

| Technique          | Time Complexity     | Use Case                          |
|--------------------|---------------------|-----------------------------------|
| Basic Bit Ops      | O(1)                | Flags, permissions, state         |
| Brian Kernighan    | O(number of set bits) | Count bits efficiently         |
| Full Bitmask       | O(2^n * n)          | n ≤ 20 subsets / DP               |
| XOR Tricks         | O(n)                | Single/missing numbers            |
| `__builtin` funcs  | O(1) or O(log n)    | Fastest in competitive coding     |

**Rule of Thumb**:
- n ≤ 20 → Bitmasking is often viable
- Need unique/frequency tricks → XOR
- State compression → Bits

---

## Practice Problems (Curated)

**Easy**:
- Single Number (136)
- Missing Number (268)
- Number of 1 Bits (191)

**Medium**:
- Single Number II (137)
- Subsets (78)
- Bitwise AND of Numbers Range (201)
- Sum of Two Integers (371)

**Hard**:
- Maximum XOR of Two Numbers in an Array (421)
- Minimum Number of Flips to Convert Binary Matrix (1284)
- Find Minimum Time to Finish All Jobs (1723) — Bitmask DP

**Bonus**:
- LRU Cache design (bit tricks for ordering)
- Sudoku Solver (bitmask optimization)

---

## Cheat Sheet

```cpp
// Core Operations
x & (1 << k)          // Check kth bit
x | (1 << k)          // Set kth bit
x & ~(1 << k)         // Clear kth bit
x ^ (1 << k)          // Toggle kth bit
x & (x - 1)           // Remove lowest set bit
x & -x                // Get lowest set bit
n & (n - 1) == 0      // Power of two
__builtin_popcountll(x) // Count set bits (fast)

// XOR Magic
all ^ = num           // Single number
i ^ nums[i]           // Missing number
```

**One-liners to remember**:
- "XOR everything" → single/missing number
- "n & (n-1)" → power of two or clear lowest bit
- "mask & (1<<i)" → check subset inclusion
- "x & -x" → isolate lowest set bit

---

## Contributing

Found a better implementation or a new trick? PRs are welcome! Especially for advanced Bitmask DP patterns and Gray Code applications.

---

*Built while grinding LeetCode Bit Manipulation tag (rated 1800+ problems). If this helped you understand bits better, drop a star ⭐*

**Happy Bit Hacking!** 🚀
