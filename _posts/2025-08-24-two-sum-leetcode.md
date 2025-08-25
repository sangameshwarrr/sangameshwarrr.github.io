---
layout: post
title: "Two Sum — The First Puzzle Every Coder Must Solve"
description: "Learn the optimal O(n) hashmap solution to Two Sum with step‑by‑step intuition, dry runs, pitfalls, and code in Python, Java, JavaScript, and Kotlin."
date: 2025-08-25 15:01:37 +0530
categories: [DSA, LeetCode]
tags: [two-sum, arrays, hashmap, algorithms, python, java, javascript, kotlin]

---

> *“Every expert was once a beginner who solved Two Sum.”*

---

## 🎯 Why this problem matters

If you’re starting out with data structures and algorithms, **Two Sum** is the warm-up drill that trains your brain to think in terms of *time complexity*, *trade‑offs*, and *pattern recognition*. You’ll meet the same idea again in 3Sum, 4Sum, subarray problems, and hash-based lookups.

---

## 🧩 Problem Statement

Given an integer array `nums` and an integer `target`, return **the indices** of the *two numbers* such that they add up to `target`.

- Assume **exactly one valid answer** exists.
- You **cannot** use the same element twice.
- Return the answer in **any order**.

### Examples

```
Input:  nums = [2,7,11,15], target = 9
Output: [0,1]  // 2 + 7 = 9

Input:  nums = [3,2,4], target = 6
Output: [1,2]  // 2 + 4 = 6

Input:  nums = [3,3], target = 6
Output: [0,1]
```

### Constraints

- `2 <= nums.length <= 10^4`
- `-10^9 <= nums[i], target <= 10^9`
- Exactly **one** valid answer exists.

---

## 🐢 Baseline (Brute Force) — O(n²)

Try every pair. It’s simple, readable, and a good starting point.

```python
def two_sum_bruteforce(nums, target):
    for i in range(len(nums)):
        for j in range(i + 1, len(nums)):
            if nums[i] + nums[j] == target:
                return [i, j]
```

- **Time:** O(n²) — nested loops
- **Space:** O(1)

Good for learning, not for large inputs.

---

## ⚡ Optimal (HashMap) — O(n)

**Idea:** As you scan numbers left → right, ask: *Which number do I need to complete the pair?*  
That number is the **complement** = `target - nums[i]`. If we’ve seen the complement before, we’re done.

```python
def two_sum(nums, target):
    seen = {}  # value -> index
    for i, num in enumerate(nums):
        comp = target - num
        if comp in seen:
            return [seen[comp], i]
        seen[num] = i
```

- **Time:** O(n) — each element processed once
- **Space:** O(n) — stores seen values

### Dry Run

For `nums = [2, 7, 11, 15], target = 9`

| i | num | comp = target - num | seen before? | seen map after      | return |
|---|-----|---------------------|--------------|---------------------|--------|
| 0 | 2   | 7                   | no           | {2: 0}              | –      |
| 1 | 7   | 2                   | **yes**      | {2: 0}              | [0, 1] |

---

## 🧠 Why it works (correctness sketch)

- **Loop invariant:** At index `i`, `seen` contains each value `nums[k]` with its index `k < i`.
- If a valid pair ends at `i`, the other index `j < i` has value `nums[j] = target - nums[i]`, which would already be in `seen` — so we return immediately.
- Since the problem guarantees **exactly one** solution, we’ll find it during the scan.

---

## 🧪 Minimal Tests

```python
assert two_sum([2,7,11,15], 9)  in ([0,1], [1,0])
assert two_sum([3,2,4], 6)      in ([1,2], [2,1])
assert two_sum([3,3], 6)        in ([0,1], [1,0])
assert two_sum([-1, -2, -3, -4], -6) in ([1,3], [3,1])  # -2 + -4
```

---

## 🧩 Edge Cases & Pitfalls

- **Duplicates:** e.g., `[3,3]` with target `6` → map ensures distinct indices (we store first 3, match with second 3).
- **Don’t use same element twice:** We only ever match with **previous** elements (stored in map), so indices are different.
- **Large values:** Python’s `int` won’t overflow, but in languages with fixed-width ints, be mindful of overflow in `target - num` (Java handles this fine for `int` within constraints).
- **Multiple answers present?** The problem guarantees one answer; any valid pair is fine.

---

## 💻 Implementations in Popular Languages

### Python (concise)
```python
from typing import List

def two_sum(nums: List[int], target: int) -> List[int]:
    seen = {}
    for i, num in enumerate(nums):
        comp = target - num
        if comp in seen:
            return [seen[comp], i]
        seen[num] = i
    raise ValueError("No solution")
```

### Java
```java
import java.util.*;

class Solution {
    public int[] twoSum(int[] nums, int target) {
        Map<Integer, Integer> seen = new HashMap<>();
        for (int i = 0; i < nums.length; i++) {
            int comp = target - nums[i];
            if (seen.containsKey(comp)) {
                return new int[]{ seen.get(comp), i };
            }
            seen.put(nums[i], i);
        }
        throw new IllegalArgumentException("No solution");
    }
}
```

### JavaScript
```javascript
function twoSum(nums, target) {
  const seen = new Map(); // value -> index
  for (let i = 0; i < nums.length; i++) {
    const comp = target - nums[i];
    if (seen.has(comp)) return [seen.get(comp), i];
    seen.set(nums[i], i);
  }
  throw new Error("No solution");
}
```

### Kotlin
```kotlin
class Solution {
    fun twoSum(nums: IntArray, target: Int): IntArray {
        val seen = HashMap<Int, Int>()
        for (i in nums.indices) {
            val comp = target - nums[i]
            val j = seen[comp]
            if (j != null) return intArrayOf(j, i)
            seen[nums[i]] = i
        }
        throw IllegalArgumentException("No solution")
    }
}
```

---

## ⏱️ Complexity

- **Time:** O(n) — single pass with O(1) average-time hashmap lookups.
- **Space:** O(n) — stores up to n elements in worst case.

---

## 🧭 Interview Talk Track (what to say out loud)

1. “I’ll start with a brute-force O(n²) to confirm understanding.”
2. “To optimize, I’ll use a hashmap to remember values I’ve seen. For each `num`, I check if `target - num` already exists.”
3. “This guarantees O(n) time and handles duplicates while ensuring distinct indices.”
4. “I can explain correctness via a loop invariant: at index `i` the map contains all indices `< i`.”
5. “If the array is sorted, I can also do a two-pointer O(n) solution without extra space (but indices refer to the *sorted* order, which differs from the original).”

---

## ➕ Variants & Follow‑ups

- **Two Sum (sorted)** → use two pointers, O(1) space.
- **3Sum / 4Sum** → sorting + two pointers + dedupe.
- **Count pairs** instead of returning indices.
- **Return the numbers** instead of indices.
- **Subarray problems** (similar complement trick with prefix sums).

---

## 🧠 Practice Prompts

- Implement both brute-force and hashmap versions, write unit tests, and benchmark on arrays of size 10³, 10⁴.
- Modify the function to return **the pair of values** (not indices).
- Solve **Two Sum II** (sorted) on your own using two pointers.

---

## ❤️ Closing Thoughts

Two Sum is where you start building your *algorithmic muscle*. Master this pattern, and you’ll spot it everywhere. Keep iterating, keep testing, and remember: clarity + correctness + complexity is the winning combo.

If this helped, **share it with a friend** and **clap** so others discover it.  
If you’d like to support my work: [buymeacoffee.com/sangamesh6j](https://www.buymeacoffee.com/sangamesh6j) ☕

---

### Appendix: Tiny Test Harness (Python)

```python
def run_tests():
    tests = [
        (([2,7,11,15], 9), {tuple([0,1]), tuple([1,0])}),
        (([3,2,4], 6),     {tuple([1,2]), tuple([2,1])}),
        (([3,3], 6),       {tuple([0,1]), tuple([1,0])}),
        (([-1,-2,-3,-4], -6), {tuple([1,3]), tuple([3,1])})
    ]
    for (nums, target), answers in tests:
        out = tuple(two_sum(nums, target))
        assert out in answers, f"Failed: nums={{nums}}, target={{target}}, got={{out}}"
    print("All tests passed!")
```
